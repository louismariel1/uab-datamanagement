# UAB 106566 — Data Management: Complete Model Answers and Explanations

Universitat Autònoma de Barcelona · Bachelor's Degree in Artificial Intelligence · Academic year 2026/2027

This solution covers all five sections of your proposed 120-minute practice exam, with model answers, executable-style SQL and PySpark code, Redis commands, design justifications, common mistakes, and a marking guide.

The exam is a proposed practice paper, so these answers are intended as a technically sound study reference, not an official UAB marking scheme.

## Exam overview

| Section | Topic | Points |
| --- | --- | --- |
| 1 | Data management fundamentals | 12 |
| 2 | SQL and relational data | 18 |
| 3 | Data warehousing, OLAP and ETL | 22 |
| 4 | Redis and low-latency access | 22 |
| 5 | Apache Spark and MLlib | 26 |
| Total |  | 100 |

The most important exam technique is to do three things in every answer:

1. State the correct concept or solution.
2. Explain why it works.
3. Identify a limitation, assumption or trade-off when relevant.

# Section 1 — Data management fundamentals

15 minutes · 12 points

## Question 1A — Short answers (6 points)

### 1. Vertical scaling versus horizontal scaling (2 points)

Vertical scaling (scale up) means increasing the resources of one machine, such as its CPU, RAM or storage capacity.

Horizontal scaling (scale out) means adding more machines to distribute storage or processing across several nodes.

For example, a system processing millions of independent product-view events could use horizontal scaling to distribute incoming events and processing across multiple servers. This can increase capacity beyond the limits of one machine.

Trade-off: Vertical scaling is often simpler to manage, but has hardware and capacity limits. Horizontal scaling can offer greater capacity and fault tolerance, but introduces distributed coordination, network communication and operational complexity.

### 2. OLTP versus OLAP (2 points)

OLTP (Online Transaction Processing) systems handle frequent operational transactions, such as creating an order, updating inventory or recording a payment. They typically support short queries, concurrent users and transactional consistency.

OLAP (Online Analytical Processing) systems support analytical queries over large volumes of historical data, such as calculating monthly revenue by country or comparing sales across years. They are often designed for aggregations, scans and multidimensional analysis.

| Property | OLTP | OLAP |
| --- | --- | --- |
| Main purpose | Run business operations | Analyse business performance |
| Typical workload | Many short reads and writes | Fewer, complex analytical queries |
| Data | Current operational records | Historical and integrated data |
| Typical design | Normalised relational schema | Star schema or columnar analytical storage |
| Example | Processing an order | Calculating annual revenue by region |

A strong answer recognises that these are workload and design distinctions, not absolute rules: modern systems can support both kinds of workloads to varying degrees.

### 3. Partitioning versus replication (2 points)

Partitioning divides a dataset into smaller parts distributed across different nodes. For example, customer records might be partitioned by customer ID so that different nodes store different subsets. Partitioning helps scale storage and processing, although a poor partitioning strategy can create data skew.

Replication stores copies of data on multiple nodes. It can improve availability, resilience and read throughput. However, replicas consume extra storage and require a mechanism to manage updates and consistency.

The key distinction is:

- Partitioning distributes different pieces of the data.
- Replication stores copies of data.

A distributed system may use both simultaneously.

## Question 1B — System design (6 points)

The shopping platform generates 10 million product-view events daily and has three requirements.

Data Engineering Data Pipeline - AIEngineer.net

1. Store events for several years

Object storage / data lakeUse durable object storage such as Amazon S3, Azure Data Lake Storage or Google Cloud Storage, with events stored in partitioned formats such as Parquet.

Why: Object storage is economical for large historical datasets and supports retention, reprocessing and downstream analytics.

Trade-off: It is not designed for very low-latency, individual-key lookups in the same way as an in-memory key-value store.

Warehouse, Lakehouse, or Vector/Real-Time Store? A 2025 Technical Guide | by Abhishek Kumar Gupta | Medium | Tech with Abhishek

2. Generate daily sales and traffic reports

Data warehouse + ETL/ELTLoad validated events and sales records into a warehouse such as BigQuery, Snowflake or a suitable Spark-based analytical platform.

Why: Warehouses optimise analytical scans, aggregations, historical reporting and queries grouped by date, product or region.

Trade-off: Data ingestion and transformation add cost and may introduce reporting latency. A daily report does not necessarily require real-time processing.

How to Build Your Own Redis (From Scratch) — And Actually Understand It | by Gauravnardia | Medium

3. Display recently viewed products in under 50 ms

RedisMaintain each user's recently viewed products in a Redis list or another suitable Redis data structure.

Why: Redis provides in-memory data access, efficient list operations and expiration support. It is a strong choice for low-latency user-specific data.

Trade-off: Memory is more expensive than object storage, and persistence, replication and eviction must be configured according to the required durability. A sub-50-ms target must be measured end to end, including the network and application.

The three requirements have different access patterns, so a single storage technology is not necessarily optimal.

A reasonable architecture is:

- Capture incoming events through a durable event-ingestion layer.
- Retain raw historical events in a data lake.
- Transform data into warehouse tables for reporting.
- Update Redis with each user's recent product views.

Marking guide: Award up to 2 points for each requirement, considering both a suitable technology and a justified trade-off.

# Section 2 — SQL and relational data

20 minutes · 18 points

The tables are `Customers(customer_id, name, country)` and `Orders(order_id, customer_id, amount, status)`.

## Question 2A — SQL queries (12 points)

### 1. Total completed-order amount for every customer (4 points)

sql
```
SELECT
    c.name,
    COALESCE(SUM(o.amount), 0) AS total_completed_amount
FROM Customers AS c
LEFT JOIN Orders AS o
    ON c.customer_id = o.customer_id
   AND o.status = 'completed'
GROUP BY
    c.customer_id,
    c.name
ORDER BY c.customer_id;
```

Expected output

| name | total\_completed\_amount |
| --- | --- |
| Ana | 200 |
| Ben | 0 |
| Carla | 150 |
| David | 0 |

Explanation

- `LEFT JOIN` retains every customer, even if they have no matching completed order.
- Putting `o.status = 'completed'` in the `ON` condition ensures that customers without completed orders remain in the result.
- `SUM(o.amount)` adds the matching order amounts.
- `COALESCE(..., 0)` replaces a `NULL` aggregate result with zero.
- Grouping by the customer identifier and name produces one result per customer.

Important exam trap: If you place `o.status = 'completed'` in the `WHERE` clause instead, customers with no matching completed orders are filtered out, effectively defeating the purpose of the outer join.

### 2. Countries with completed-order revenue greater than 200 (4 points)

sql
```
SELECT
    c.country,
    SUM(o.amount) AS total_revenue
FROM Customers AS c
JOIN Orders AS o
    ON c.customer_id = o.customer_id
WHERE o.status = 'completed'
GROUP BY c.country
HAVING SUM(o.amount) > 200
ORDER BY total_revenue DESC;
```

Expected output: No rows.

The completed orders are:

- Ana, Spain: $120 + 80 = 200$
- Carla, Spain: $150$
- Ben, France: no completed orders
- David, Italy: no orders

Spain therefore has completed-order revenue of $350$, so Spain should appear in the result. The correct expected output is:

| country | total\_revenue |
| --- | --- |
| Spain | 350 |

Explanation

- `WHERE` filters individual orders before aggregation.
- `GROUP BY` aggregates the remaining orders by country.
- `HAVING` filters the resulting groups.
- `ORDER BY ... DESC` sorts revenue from highest to lowest.

The essential distinction is that `WHERE` filters rows, whereas `HAVING` filters groups.

### 3. Customers who have never placed a completed order (4 points)

A clear solution uses `NOT EXISTS`:

sql
```
SELECT
    c.customer_id,
    c.name
FROM Customers AS c
WHERE NOT EXISTS (
    SELECT 1
    FROM Orders AS o
    WHERE o.customer_id = c.customer_id
      AND o.status = 'completed'
);
```

Expected output

| customer\_id | name |
| --- | --- |
| 2 | Ben |
| 4 | David |

Ben has only a cancelled order, and David has no orders. Neither has a completed order.

Why use `NOT EXISTS`?

It tests whether a matching completed order exists for each customer. It avoids accidentally counting cancelled or pending orders as completed.

An alternative is a `LEFT JOIN` followed by a check for a missing matching order:

sql
```
SELECT c.customer_id, c.name
FROM Customers AS c
LEFT JOIN Orders AS o
    ON c.customer_id = o.customer_id
   AND o.status = 'completed'
WHERE o.order_id IS NULL;
```

Both approaches are valid.

## Question 2B — Data integrity (6 points)

### 1. Primary key and foreign key (2 points)

- `Customers.customer_id` is the primary key of `Customers`.
- `Orders.order_id` is the primary key of `Orders`.
- `Orders.customer_id` is a foreign key referencing `Customers.customer_id`.

A primary key uniquely identifies each row and cannot be `NULL`. A foreign key enforces the relationship between an order and its customer, assuming the constraint is enabled.

### 2. Referential integrity (2 points)

If an order references a nonexistent customer, inserting or updating that order fails when the foreign-key constraint is enforced and no valid matching customer exists.

For example, an order with `customer_id = 99` cannot be inserted if customer 99 is absent.

Deletion and update behaviour depends on the configured foreign-key action, such as `RESTRICT`, `CASCADE` or `SET NULL`, subject to the schema's nullability and database support.

### 3. Incorrect sums after joining one-to-many tables (2 points)

Suppose an order of €100 has three associated rows in an `OrderItems` or `OrderEvents` table. Joining the order to all three rows can produce three copies of the €100 order amount. A subsequent `SUM(Orders.amount)` then returns €300 instead of €100.

Remedies include:

- Aggregate the many-side table to the required grain before joining.
- Aggregate orders independently, then join the results.
- Use `SUM(DISTINCT amount)` only in cases where its semantics are demonstrably correct; it is not a general solution because different orders may have equal amounts.

The general rule is to check the grain and cardinality of every join before aggregating.

# Section 3 — Data warehousing, dimensional modelling and ETL

25 minutes · 22 points

## Question 3A — Design a star schema (8 points)

### 1. Central fact table and dimensions (3 points)

A suitable design is a star schema with one central sales fact table and five surrounding dimensions.

### FactSales

Measures + dimension keys

DateKey · ProductKey · StoreKey · CustomerKey · ChannelKey

Quantity · SalesAmount · DiscountAmount · CostAmount

#### DimDate

Day, month, quarter, year

#### DimProduct

Product, brand, category

#### DimStore

Store, city, region

#### DimCustomer

Customer, segment, country

#### DimChannel

Online, physical store, other channels

The fact table contains sales events and their measurable outcomes. The dimension tables contain descriptive attributes used for filtering, grouping and reporting.

Typical keys include:

- `FactSales.DateKey` → `DimDate.DateKey`
- `FactSales.ProductKey` → `DimProduct.ProductKey`
- `FactSales.StoreKey` → `DimStore.StoreKey`
- `FactSales.CustomerKey` → `DimCustomer.CustomerKey`
- `FactSales.ChannelKey` → `DimChannel.ChannelKey`

In a warehouse, dimension tables often use surrogate keys, such as `ProductKey`, to distinguish warehouse records from source-system product IDs.

### 2. Define the grain precisely (2 points)

A good grain statement is:

One row in `FactSales` represents one product line within one sales transaction, at a particular store or sales channel, for a particular customer and transaction date.

For example, an order containing three different products produces three fact rows if each product is a separate transaction line.

The grain determines which measures can be stored and how they can be aggregated. Transaction ID and transaction-line ID may be retained as degenerate identifiers in the fact table if useful.

A weaker answer would say merely "one row per sale." That is ambiguous: does a sale mean a transaction, an order line, or a daily total? A precise grain avoids double counting.

### 3. Measures and additivity (3 points)

| Measure | Type | Explanation |
| --- | --- | --- |
| Quantity sold | Additive | Can be summed across products, dates and stores. |
| Sales amount | Additive | Monetary line amounts can be summed across dimensions, assuming consistent currency and accounting rules. |
| Cost amount | Additive | Costs can be summed at the defined transaction-line grain. |
| Discount amount | Additive | Discount amounts can be summed when consistently defined. |
| Profit margin (%) | Non-additive | Recalculate as total profit divided by total sales; do not sum percentages. |
| Inventory balance | Semi-additive | Can be summed across products or stores at a point in time, but generally not across dates. |

For instance, total profit margin should be calculated as:

$$
\text{Profit Margin} =
\frac{\sum \text{Profit}}{\sum \text{Sales Amount}}\times100
$$

It should not be calculated by summing or simply averaging individual transaction margins.

Key distinction to remember:

- Additive: summable across all relevant dimensions.
- Semi-additive: summable across some dimensions but not others.
- Non-additive: not meaningfully summed; usually needs a formula or other aggregation.

## Question 3B — OLAP operations (6 points)

### 1. Roll-up (2 points)

Roll-up aggregates data to a higher level in a hierarchy.

Example: aggregate daily sales into monthly sales, or aggregate store-level sales into regional sales.

sql
```
SELECT
    d.year,
    d.month,
    SUM(f.sales_amount) AS monthly_sales
FROM FactSales AS f
JOIN DimDate AS d
    ON f.date_key = d.date_key
GROUP BY d.year, d.month
ORDER BY d.year, d.month;
```

Explanation: Many detailed rows are combined into fewer summary rows.

### 2. Drill-down (2 points)

Drill-down moves from a summary to a more detailed level.

Example: begin with annual sales and drill down to quarters, months and individual days. Another example is drilling from country-level sales to region and then to store.

Explanation: Drill-down reveals detail behind a summary and helps identify where a trend or anomaly originated.

Roll-up and drill-down are opposite directions through a hierarchy.

### 3. Slice or dice (2 points)

Slice fixes one dimension to a particular value. For example, analyse sales for the year 2026 only.

Dice selects a subset across multiple dimensions. For example, analyse online sales for selected product categories in Spain and France during the first quarter.

Illustrative SQL for a dice operation:

sql
```
SELECT
    p.category,
    c.country,
    SUM(f.sales_amount) AS sales
FROM FactSales AS f
JOIN DimProduct AS p
    ON f.product_key = p.product_key
JOIN DimCustomer AS c
    ON f.customer_key = c.customer_key
JOIN DimChannel AS ch
    ON f.channel_key = ch.channel_key
JOIN DimDate AS d
    ON f.date_key = d.date_key
WHERE ch.channel_name = 'Online'
  AND c.country IN ('Spain', 'France')
  AND d.quarter = 1
  AND d.year = 2026
GROUP BY p.category, c.country;
```

The query filters the cube to a smaller subset and then aggregates the selected data.

## Question 3C — ETL pipeline (8 points)

ETL stands for Extract, Transform, Load.

A robust solution should validate data before loading it into the warehouse and should handle retries without creating duplicate facts.

1. Extract

Read from online sales, physical stores and the third source system

2. Stage raw data

Preserve source records, ingestion timestamps, batch IDs and source transaction IDs

3. Clean and validate

Resolve product IDs, deduplicate, standardise dates and validate required fields

Valid records

Load into dimensions and fact tables

Rejected records

Error log, quarantine and remediation

### 1. Extraction and staging (2 points)

- Extract data from each source, using incremental extraction where possible.
- Record source identifiers, extraction time, batch ID and ingestion metadata.
- Stage raw records in durable storage before transformation.
- Preserve the original data so errors can be investigated and transformations rerun.

If a source supports change-data capture (CDC), it can help identify inserted, updated or deleted records. Otherwise, scheduled incremental extracts or controlled full extracts may be appropriate.

### 2. Handling missing values, duplicates and dates (3 points)

Missing product IDs

- Attempt to recover the ID from a trusted source mapping or another reliable attribute.
- If the product cannot be identified, quarantine the record or map it to a designated "Unknown Product" dimension member, depending on business requirements.
- Do not silently discard a potentially valuable sale without recording the reason.

Duplicated transactions

- Identify duplicates using a stable transaction identifier, ideally including the source system and line identifier when IDs are not globally unique.
- Apply a deterministic rule for selecting the authoritative version, such as the latest valid update.
- Check whether the transaction has already been loaded.

Inconsistent date formats

- Parse dates using known source-specific formats.
- Convert valid values into a standard date or timestamp representation.
- Apply consistent time-zone rules when timestamps represent real instants.
- Quarantine invalid dates rather than guessing silently.

For example, `2026-03-04` can mean March 4 in ISO format, while `03/04/2026` is ambiguous without knowing the source convention.

### 3. Error logging and safe reruns (3 points)

Error logging: Record the source system, source record ID, batch ID, error code, error message, processing timestamp and original payload or a reference to it. This allows failed records to be investigated and reprocessed.

Idempotency: Re-running the same batch should not create duplicate transactions. Use a stable business key, enforce appropriate uniqueness constraints where supported, and use an idempotent upsert or `MERGE` operation.

An illustrative SQL pattern is:

sql
```
MERGE INTO FactSales AS target
USING StagedSales AS source
ON target.source_system = source.source_system
AND target.transaction_id = source.transaction_id
AND target.transaction_line_id = source.transaction_line_id

WHEN MATCHED THEN
    UPDATE SET
        target.sales_amount = source.sales_amount,
        target.quantity = source.quantity

WHEN NOT MATCHED THEN
    INSERT (
        source_system,
        transaction_id,
        transaction_line_id,
        sales_amount,
        quantity
    )
    VALUES (
        source.source_system,
        source.transaction_id,
        source.transaction_line_id,
        source.sales_amount,
        source.quantity
    );
```

This is illustrative SQL; exact `MERGE` syntax and concurrency guarantees vary by database. The source should also be deduplicated before the merge, and a reliable transaction strategy should protect related dimension and fact updates.

Excellent answer: Explains that deduplication alone is not enough. A rerun must also be safe when a previous execution partially succeeded.

# Section 4 — Redis and low-latency data access

25 minutes · 22 points

## Question 4A — Data structures (6 points)

### 1. Recommended data structure (2 points)

A Redis List is a natural choice because it supports ordered entries and efficient operations at both ends of the list.

For user `U7`, the key could be:

text
```
recently_viewed:U7
```

Example value, from most recent to least recent:

text
```
[P42, P17, P09, P31, P08]
```

A list supports pushing an item to the front, removing entries, trimming to a fixed length and setting a TTL.

However, a Redis list does not enforce uniqueness automatically. An additional step is needed to remove an existing occurrence before inserting the new one.

### 2. Why a string or set is insufficient on its own (2 points)

- A string can store a serialized list, but updating one element generally requires application-side parsing and rewriting the value unless the data is encoded in another way.
- A set enforces uniqueness but does not maintain the required recency order.
- A list maintains order but permits duplicates.

Therefore, a list plus a deduplication operation is a straightforward solution. A sorted set is another possible design if each item has a suitable score representing recency, but score ties and uniqueness semantics must be handled deliberately.

### 3. Purpose of TTL (2 points)

TTL stands for Time To Live. It defines how long a key remains before Redis expires it.

For this service, a TTL of 24 hours means the key expires after 24 hours without a refresh, assuming each successful view update resets the expiration.

This limits the lifetime of inactive users' data and helps control memory usage. TTL is key-level expiration here, so it expires the entire user's list, not each product independently.

## Question 4B — Redis commands (8 points)

### 1–4. Add `P42`, ensure uniqueness, retain five items and refresh expiry

The required operations are conceptually:

1. Remove existing occurrences of `P42`.
2. Insert `P42` at the front.
3. Trim the list to five entries.
4. Set the TTL to 24 hours.

Redis commands:

redis
```
LREM recently_viewed:U7 0 P42
LPUSH recently_viewed:U7 P42
LTRIM recently_viewed:U7 0 4
EXPIRE recently_viewed:U7 86400
```

Explanation

- `LREM key 0 P42` removes all occurrences of `P42`.
- `LPUSH` inserts `P42` at the head of the list.
- `LTRIM key 0 4` keeps positions 0 through 4, which means at most five entries.
- `EXPIRE key 86400` sets the key's expiration to 86,400 seconds, or 24 hours.

These commands are correct when executed without competing updates, but they should not be treated as a concurrency-safe solution when issued separately.

## Question 4C — Reliability and scalability (8 points)

### 1. Why separate commands can cause race conditions (3 points)

Redis executes each individual command atomically, but a sequence of commands from a client is not automatically atomic as a group.

Suppose two requests update the same user concurrently:

- Request A removes `P42`.
- Request B inserts `P17`.
- Request A inserts `P42`.
- Request B trims the list and refreshes the TTL.

The final ordering can differ from the ordering expected by either request. With multiple concurrent operations, a read-modify-write sequence can violate the intended uniqueness or list-length rules if those rules are not protected by a single atomic operation.

This is a race condition: the result depends on the interleaving of operations.

### 2. Make the update atomic (2 points)

Use a Redis Lua script executed with `EVAL` or a loaded script invoked with `EVALSHA`. Redis runs the script atomically relative to other commands on the same server.

For example:

lua
```
-- KEYS[1] = recently_viewed:U7
-- ARGV[1] = product ID, e.g. P42
-- ARGV[2] = TTL in seconds, e.g. 86400
-- ARGV[3] = maximum list size, e.g. 5

local key = KEYS[1]
local product = ARGV[1]
local ttl = tonumber(ARGV[2])
local max_size = tonumber(ARGV[3])

redis.call('LREM', key, 0, product)
redis.call('LPUSH', key, product)
redis.call('LTRIM', key, 0, max_size - 1)
redis.call('EXPIRE', key, ttl)

return redis.call('LRANGE', key, 0, -1)
```

Invoke it with the key, product ID, TTL and list size as arguments.

This script ensures that deduplication, insertion, trimming and expiration refresh happen together. For production use, validate the arguments, define the behaviour for invalid values, and use the correct Redis Cluster key-routing conventions.

### 3. Redis as the only permanent activity store (3 points)

Benefit: Redis can provide very low-latency reads and writes, with convenient data structures and expiration. It can simplify a service whose principal need is a fast, recent-activity view.

Limitation: Redis is commonly used as an in-memory data store or cache. Memory costs more than archival object storage, and activity data can be lost depending on persistence settings, failures, replication behaviour and recovery configuration. Expiring the list after 24 hours also deliberately removes it.

For durable customer activity, a better architecture is often to retain canonical events in a durable database, event log or data lake, while using Redis as a fast, expiring access layer.

Exam takeaway: A Redis list provides the right basic structure, but the atomicity of the complete update is just as important as the individual commands.

# Section 5 — Apache Spark and data processing

35 minutes · 26 points

## Question 5A — PySpark DataFrames (10 points)

Assume `transactions` is a PySpark DataFrame containing the specified columns.

### 1. Remove duplicate transactions using `transaction_id` (2 points)

python
```
transactions = transactions.dropDuplicates(["transaction_id"])
```

This keeps one row per distinct `transaction_id`.

Important limitation: `dropDuplicates()` does not guarantee which row is retained when duplicate IDs have different values in other columns. If records can conflict, use a deterministic rule, such as keeping the latest record according to a reliable update timestamp.

For example:

python
```
from pyspark.sql import functions as F
from pyspark.sql.window import Window

w = Window.partitionBy("transaction_id").orderBy(
    F.col("updated_at").desc_nulls_last()
)

transactions = (
    transactions
    .withColumn("_rn", F.row_number().over(w))
    .filter(F.col("_rn") == 1)
    .drop("_rn")
)
```

This second example assumes the input contains an `updated_at` column. It may require a distributed shuffle to calculate the window.

### 2. Create the `revenue` column (2 points)

python
```
from pyspark.sql import functions as F

transactions = transactions.withColumn(
    "revenue",
    F.col("quantity") * F.col("unit_price")
)
```

`withColumn()` returns a new DataFrame with the specified column expression. It does not modify the original DataFrame in place.

The result represents line revenue if `unit_price` is the price per unit and the transaction contains no additional adjustments that must be included.

### 3. Total revenue by country, highest first (3 points)

python
```
from pyspark.sql import functions as F

revenue_by_country = (
    transactions
    .groupBy("country")
    .agg(F.sum("revenue").alias("total_revenue"))
    .orderBy(F.col("total_revenue").desc())
)

revenue_by_country.show()
```

Explanation

- `groupBy("country")` defines the aggregation groups.
- `sum("revenue")` calculates total revenue per country.
- `alias()` gives the aggregated column a meaningful name.
- `orderBy(...desc())` sorts the totals from highest to lowest.

This aggregation typically requires a shuffle because records for the same country must be brought together for aggregation.

### 4. Filter missing customer IDs and invalid quantities (3 points)

A robust solution explicitly checks that the customer ID is present and the quantity is positive and valid.

python
```
from pyspark.sql import functions as F

transactions = transactions.filter(
    F.col("customer_id").isNotNull()
    & F.col("quantity").isNotNull()
    & (F.col("quantity") > 0)
)
```

This assumes `quantity` has already been parsed as a numeric column. If it comes from a CSV as a string, parse it first:

python
```
transactions = transactions.withColumn(
    "quantity",
    F.expr("try_cast(quantity AS DOUBLE)")
)

transactions = transactions.filter(
    F.col("customer_id").isNotNull()
    & F.col("quantity").isNotNull()
    & (F.col("quantity") > 0)
)
```

`try_cast` is supported in modern Spark SQL versions; verify the version in use. Malformed quantities become `NULL` rather than causing the cast to fail.

A strong production pipeline would quarantine malformed records for investigation rather than simply discarding them.

### Complete solution in one pipeline

The following combines the four requirements in a readable sequence, assuming the quantity is already numeric and duplicate IDs are interchangeable:

python
```
from pyspark.sql import functions as F

clean_transactions = (
    transactions
    .dropDuplicates(["transaction_id"])
    .filter(
        F.col("customer_id").isNotNull()
        & F.col("quantity").isNotNull()
        & (F.col("quantity") > 0)
    )
    .withColumn(
        "revenue",
        F.col("quantity") * F.col("unit_price")
    )
)

revenue_by_country = (
    clean_transactions
    .groupBy("country")
    .agg(F.sum("revenue").alias("total_revenue"))
    .orderBy(F.col("total_revenue").desc())
)

revenue_by_country.show()
```

Note that this pipeline removes duplicates before validating the quantity. In production, the order of cleaning and deduplication should be chosen according to the business rules for identifying the authoritative transaction record.

## Question 5B — Spark execution model (6 points)

### 1. Transformation versus action (2 points)

A transformation defines a new DataFrame or RDD from an existing one.

Examples:

python
```
filtered = transactions.filter(F.col("quantity") > 0)
```

python
```
selected = transactions.select("customer_id", "product_id")
```

An action requests a result or causes computation to be executed.

Examples:

python
```
transactions.show()
```

python
```
count = transactions.count()
```

The distinction matters because transformations are generally lazy, whereas actions trigger execution of the computation required to produce their results.

### 2. Lazy evaluation (2 points)

Lazy evaluation means Spark records transformations as a logical plan rather than immediately executing each operation.

For example:

python
```
result = (
    transactions
    .filter(F.col("quantity") > 0)
    .groupBy("country")
    .agg(F.sum("quantity").alias("total_quantity"))
)

result.show()
```

Spark can optimise the combined plan before executing it. Depending on the operations, optimisations can include predicate pushdown, column pruning, expression simplification and physical-plan selection.

Benefit: Spark can avoid unnecessary work and organise execution more efficiently than executing every transformation independently.

### 3. Narrow versus wide transformations (2 points)

| Property | Narrow transformation | Wide transformation |
| --- | --- | --- |
| Data dependency | Each output partition depends on a limited number of input partitions, typically one | An output partition may depend on many input partitions |
| Shuffle | Usually no shuffle required | Often requires a shuffle |
| Example | `filter()`, `select()`, `withColumn()` | `groupBy().agg()`, `distinct()`, many joins |
| Cost | Often relatively inexpensive | Network transfer, serialization and disk spill may be significant |

A narrow transformation can often execute within an existing partition without redistributing records.

A wide transformation often redistributes data across the cluster so records with the same grouping key or matching join key can be processed together.

Exam tip: Do not state that every join always causes a shuffle. Broadcast joins and other physical execution strategies can avoid a full shuffle of both sides.

## Question 5C — Performance and machine learning (10 points)

### 1. Join a large transaction dataset with a small product-category table (3 points)

Use a broadcast join when the product-category table is small enough to fit safely in memory on each participating executor.

python
```
from pyspark.sql import functions as F

transactions_with_category = transactions.join(
    F.broadcast(product_categories),
    on="product_id",
    how="left"
)
```

Why it works: Spark broadcasts the small table to executors, allowing them to join their local transaction partitions against it. This can avoid shuffling the much larger transaction dataset by the join key.

When appropriate:

- The product-category table is genuinely small after filtering and selecting the necessary columns.
- The table can be distributed and held in executor memory without causing memory pressure.
- The join keys and data types are compatible.

Trade-off: Broadcasting a table that is too large can create memory pressure or cause the job to fail. For larger tables, a shuffle join or another strategy may be more suitable.

The physical execution plan can be inspected with:

python
```
transactions_with_category.explain("formatted")
```

### 2. Data skew and mitigation (3 points)

Data skew occurs when some partitions receive substantially more data or work than others.

For example, if 40% of transactions have the same product ID, a join or aggregation by product ID may send a disproportionate amount of data to one partition.

This can cause:

- A few slow tasks that delay the whole stage.
- Excessive memory usage or disk spilling.
- Out-of-memory errors.
- Poor cluster utilisation while other tasks finish early.

Mitigation: Apply a suitable strategy based on the operation.

- Enable Adaptive Query Execution (AQE) and skew-join optimisation where supported.
- Salt heavily skewed join keys when appropriate, replicating the corresponding smaller-side records correctly.
- Pre-aggregate data before joining when doing so preserves the required semantics.
- Broadcast a genuinely small side of a join when feasible.

Example configuration for AQE:

python
```
spark.conf.set(
    "spark.sql.adaptive.enabled",
    "true"
)

spark.conf.set(
    "spark.sql.adaptive.skewJoin.enabled",
    "true"
)
```

AQE can adapt parts of the physical plan based on runtime statistics, but it is not a universal solution to every skew problem.

### 3. Design a Spark MLlib pipeline to predict next-week purchases (4 points)

This is a binary classification problem.

The target variable is:

- `1` if the customer makes at least one purchase during the next week.
- `0` otherwise.

The features could include purchases in previous weeks, recent activity counts, historical spending, recency of last purchase and customer tenure.

1. Define the prediction dataset

One row per customer at a prediction cutoff date

2. Build historical features and label

Past activity = features; purchase in the next seven days = label

3. Split into train and test sets

Prefer a chronological split for future-purchase prediction

4. Fit the ML pipeline

Feature assembly → classifier → probability and prediction

5. Evaluate on held-out data

AUC, precision, recall or F1, as appropriate

#### Example PySpark MLlib implementation

Assume a prepared DataFrame `customer_features` contains numeric features and a numeric binary label called `label`.

python
```
from pyspark.ml import Pipeline
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.evaluation import BinaryClassificationEvaluator

feature_cols = [
    "purchases_last_7_days",
    "purchases_last_30_days",
    "spend_last_30_days",
    "days_since_last_purchase"
]

assembler = VectorAssembler(
    inputCols=feature_cols,
    outputCol="features"
)

classifier = LogisticRegression(
    featuresCol="features",
    labelCol="label"
)

pipeline = Pipeline(stages=[
    assembler,
    classifier
])

train, test = customer_features.randomSplit(
    [0.8, 0.2],
    seed=42
)

model = pipeline.fit(train)
predictions = model.transform(test)

evaluator = BinaryClassificationEvaluator(
    labelCol="label",
    rawPredictionCol="rawPrediction",
    metricName="areaUnderROC"
)

auc = evaluator.evaluate(predictions)
print("Test ROC AUC:", auc)
```

This is an illustrative implementation. It assumes feature columns are numeric and valid, missing values have been handled, and the label is correctly defined. A production pipeline might also include imputation, categorical encoding, feature scaling or hyperparameter tuning.

Why ROC AUC? It measures how well the model ranks positive cases above negative cases across classification thresholds. It is often useful when the purchase class is relatively rare, although precision-recall metrics may be more informative under severe class imbalance.

#### Critical consideration: data leakage

The features must use information available at the prediction cutoff date only. If the goal is to predict purchases during the next seven days, purchases from that future period must not be included in the input features.

For realistic evaluation, a chronological split is often preferable to a random split. For example, train on earlier weeks and evaluate on later weeks. This better simulates deployment, where the model predicts future behaviour using past observations.

# Final revision sheet

The following are the high-value concepts to memorise and understand before attempting the exam under timed conditions.

1. Database fundamentals

- OLTP: operational transactions.
- OLAP: analytical queries.
- Partitioning: divide data.
- Replication: copy data.
- Choose storage based on durability, latency, scale and access patterns.

2. SQL

- `LEFT JOIN` preserves unmatched left-side rows.
- `COALESCE` replaces `NULL` with a chosen value.
- `WHERE` filters rows; `HAVING` filters groups.
- `NOT EXISTS` finds rows with no matching record.
- Check join cardinality before summing measures.

3. Warehousing and ETL

- Define the fact-table grain before selecting measures.
- Additive measures can be summed across their applicable dimensions.
- Roll-up summarises; drill-down increases detail; slice/dice filters an analytical subset.
- Validate data, log failures, and make reruns idempotent.

4. Redis

- Lists preserve order but allow duplicates.
- Sets enforce uniqueness but do not preserve recency order.
- `LPUSH`, `LREM`, `LTRIM` and `EXPIRE` cover the core operations.
- Use Lua scripting or another suitable atomic mechanism for concurrent updates.
- Distinguish a low-latency cache from durable historical storage.

5. Spark and MLlib

- Transformations build plans; actions trigger computation.
- Lazy evaluation allows plan optimisation.
- Wide transformations often require shuffles.
- Broadcast joins can avoid shuffling a large table when the other side is small.
- Data skew creates straggler tasks.
- ML pipelines need valid labels, leakage-free features, appropriate splits and meaningful evaluation.

## Suggested marking rubric

Use the following rubric to assess the quality of your written answers. It is a suggested practice rubric, not an official UAB grading policy.

| Section | Full-credit standard |
| --- | --- |
| 1 — Fundamentals | Correct definitions, suitable technology choices and explicit trade-offs |
| 2 — SQL | Correct results, joins, aggregation, `NULL` handling and integrity constraints |
| 3 — Warehouse | Precise fact grain, valid dimensions and measures, appropriate OLAP and reliable ETL |
| 4 — Redis | Correct ordering, uniqueness, trimming, TTL and atomicity |
| 5 — Spark | Correct code, execution-model understanding, performance reasoning and a leakage-free ML design |

A useful self-assessment rule is to deduct points not only for incorrect answers, but also for missing justifications, ambiguous fact-table grains, non-atomic Redis updates and ML pipelines that leak future information.

## How to use this solution for the best exam preparation

1. Attempt the original five questions in 120 minutes without consulting these answers.
2. Compare your answers with the model solutions and award yourself points using the rubric.
3. Rewrite every answer that you could not explain in your own words.
4. Practise the SQL and PySpark code until you can write the essential operations from memory.
5. Spend extra revision time on any area where you can name a technology but cannot explain why it is appropriate or what its limitations are.

The strongest preparation is not memorising the code alone: it is understanding the reasoning behind each design choice, the assumptions under which it works, and the failure modes you would need to handle in a real data system.
