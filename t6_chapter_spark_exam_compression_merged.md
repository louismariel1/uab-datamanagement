# T6 — Apache Spark: Exam Compression

**Course:** UAB 106566 — Data Management\
**Academic year:** 2026/2027\
**Degree:** Bachelor's Degree in Artificial Intelligence\
**University:** Universitat Autònoma de Barcelona

---

## 0\. How to revise this chapter

### Learning objectives

- Explain why large-scale datasets require distributed processing.
- Describe Spark's architecture, execution model and DAG.
- Understand RDDs, transformations, actions, lazy evaluation and lineage.
- Write basic PySpark programs using RDDs and DataFrames.
- Distinguish narrow and wide transformations and explain shuffles.
- Perform DataFrame selection, filtering, sorting and aggregation.
- Apply Spark performance guidelines.
- Explain classic Spark Streaming, DStreams and Structured Streaming.
- Recall the introductory NoSQL concepts covered in the lecture.

### Revision priority

1. RDDs, transformations, actions and lazy evaluation.
2. Word-count and character-count exercises.
3. Narrow versus wide transformations and shuffles.
4. DataFrame operations and aggregations.
5. Spark architecture and performance.
6. Spark Streaming.
7. NoSQL, ACID, BASE and CAP.

# 1\. NoSQL databases — introductory recap

NoSQL is a family of database systems offering different trade-offs in data modelling, flexibility, scalability, performance and availability.

| Database model | Description | Example |
| --- | --- | --- |
| Document | Stores records as documents | MongoDB |
| Key-value | Stores values indexed by keys | Redis |
| Column-family | Organises data into column families | Distributed column-family databases |
| Graph | Represents entities and relationships as nodes and edges | Graph databases |

Redis is an in-memory key-value data store that also supports additional data structures.

### ACID, BASE and CAP

- **ACID:** Atomicity, Consistency, Isolation, Durability. Properties associated with reliable transactions.
- **BASE:** Basically Available, Soft state, Eventual consistency. A conceptual approach associated with some distributed systems.
- **CAP theorem:** During a network partition, a distributed system cannot guarantee both strict consistency and availability simultaneously.

**Exam takeaway:** No database is universally best. The choice depends on the data model, workload, consistency requirements, availability and scalability needs.

# 2\. Why distributed processing is necessary

## 2.1 Single-machine processing versus distributed processing

Pandas provides convenient Python data structures, particularly DataFrames, for data analysis.

- **Strengths:** Intuitive syntax, powerful manipulation tools and convenient local analysis.
- **Limitation:** Standard Pandas processing is performed on one machine and is limited by that machine's resources.
- **Vertical scaling:** Increasing the resources of one machine, such as CPU and RAM.
- **Horizontal scaling:** Distributing computation across multiple machines.

When datasets become very large, one machine may lack sufficient memory, storage or processing power.

Apache Spark divides data into partitions and distributes tasks across a cluster.

| Pandas on one machine | Apache Spark |
| --- | --- |
| Primarily single-machine processing | Distributed processing |
| Limited by one machine's resources | Can scale across multiple nodes |
| Convenient for local analysis | Suitable for large-scale workloads |
| No distributed worker shuffle | Some operations require network shuffles |

A **shuffle** redistributes data between partitions, often across the network. Not every Spark operation requires one.

## 2.2 Spark versus Hadoop MapReduce

Traditional Hadoop MapReduce commonly writes intermediate results to disk between processing stages.

Spark can retain and cache intermediate datasets in memory, reducing repeated disk I/O for suitable workloads.

The lecture mentions Spark being up to 100 times faster. This is a benchmark claim under particular conditions, not a universal guarantee. Performance depends on workload, configuration, memory, storage and data movement. Spark can also spill data to disk.

## 2.3 Benefits of Spark

1. **Speed:** In-memory computation and reduced disk I/O where applicable.
2. **Scalability:** Distributes processing across multiple machines.
3. **Caching:** Reuses intermediate results.
4. **High-level APIs:** Supports Python, Scala, Java and other interfaces.
5. **Broad ecosystem:** SQL, machine learning, graph processing and streaming.
6. **Fault tolerance:** Uses lineage to recompute lost partitions when the necessary source data and dependencies remain available.

## 2.4 Spark ecosystem

| Component | Purpose |
| --- | --- |
| Spark Core | Execution engine and fundamental distributed-processing functionality |
| Spark SQL | Structured data processing, SQL and DataFrames |
| Spark Streaming | Classic stream processing using DStreams |
| MLlib | Machine-learning functionality |
| GraphX | Graph-processing framework, particularly associated with Scala |

**Likely exam question:** Which Spark component would you use for SQL queries, machine learning and graph processing?

# 3\. Spark architecture and execution model

## 3.1 Main components

| Component | Role |
| --- | --- |
| Driver | Runs the application, constructs execution plans, schedules tasks and coordinates execution |
| SparkContext | Classic entry point to Spark Core and RDD functionality |
| SparkSession | Modern entry point commonly used for DataFrames and Spark SQL |
| Cluster manager | Allocates resources; examples include Spark Standalone, YARN and Kubernetes |
| Worker | Machine providing resources for distributed computation |
| Executor | Process on a worker that executes tasks and may cache intermediate data |
| Partition | Unit of distributed data processing |

The driver coordinates the application; it does not perform all distributed computation itself.

## 3.2 Application lifecycle

1. Create the Spark application and its entry point.
2. Read input data or create an RDD/DataFrame.
3. Define transformations.
4. Spark records computation dependencies and constructs an execution plan.
5. Call an action.
6. Spark schedules the necessary stages and tasks across partitions and executors.
7. Return results to the driver or write output to external storage.

**Central principle:** Transformations are normally lazy. An action triggers the computation needed to produce the requested result.

## 3.3 DAG and stages

A **DAG (Directed Acyclic Graph)** represents computation dependencies without directed cycles.

Example:

`Input RDD → filter() → map() → collect()`

- `filter()` and `map()` define transformations.
- `collect()` requests the result and triggers the necessary execution.
- Spark divides execution into stages.
- Tasks within a stage process partitions.
- Shuffle dependencies commonly separate stages.

## 3.4 MapReduce and Spark

The MapReduce paradigm separates processing conceptually into:

- **Map:** Transforms input elements into intermediate outputs.
- **Reduce:** Aggregates intermediate outputs.

Spark supports similar patterns but is not limited to a strict two-stage MapReduce model. It can execute more complex DAGs involving multiple transformations and stages.

# 4\. RDDs — Resilient Distributed Datasets

## 4.1 Definition and properties

An **RDD** is an immutable, distributed collection of elements that Spark processes in parallel.

- **Resilient:** Lost partitions can often be recomputed using lineage.
- **Distributed:** Data is divided into partitions across the cluster.
- **Immutable:** Transformations produce new RDDs instead of modifying existing ones.
- **Lazy:** Transformations normally define computation without executing immediately.
- **Parallel:** Different partitions can be processed concurrently.
- **Lineage-based:** Records dependencies that can help recover lost partitions.

RDD elements can be numbers, strings, tuples or other objects.

Example:

```
numbers = sc.parallelize([1, 2, 3, 4, 5])

doubled = numbers.map(lambda x: x * 2)
```

`numbers` remains unchanged. `doubled` represents a new RDD.

## 4.2 Creating RDDs

### From a Python collection

```
numbers = sc.parallelize([1, 2, 3, 4, 5], 4)
```

The second argument requests four partitions. Four partitions do not necessarily mean four physical worker machines.

### From a text file

```
lines = sc.textFile("data/book.txt")
```

Each RDD element normally represents one line.

### Modern PySpark setup

```
from pyspark.sql import SparkSession

spark = (
    SparkSession.builder
    .appName("DataManagement")
    .getOrCreate()
)

sc = spark.sparkContext
```

SparkSession is the usual modern entry point, while SparkContext provides access to RDD functionality.

# 5\. Transformations versus actions

This is one of the most important exam topics.

## 5.1 Transformations

A transformation defines a new RDD from an existing RDD.

- Returns a new RDD.
- Normally executes lazily.
- Adds dependencies to the computation graph.
- Does not modify the original RDD.

Examples: `map()`, `filter()`, `flatMap()`, `union()`, `distinct()`, `reduceByKey()`.

## 5.2 Actions

An action requests a result or writes data to external storage.

- Triggers the computation needed for the requested result.
- May return a value to the driver or write distributed output.
- Does not return a new RDD as its result.

Examples: `count()`, `first()`, `take()`, `collect()`, `reduce()`, `foreach()`, `saveAsTextFile()`.

| Transformation | Action |
| --- | --- |
| Defines a computation | Requests execution or output |
| Returns a new RDD | Returns a result or writes output |
| Normally lazy | Triggers the necessary computation |
| `map()` | `count()` |

Example:

```
numbers = sc.parallelize([1, 2, 3, 4, 5])

squares = numbers.map(lambda x: x * x)

print(squares.collect())
# [1, 4, 9, 16, 25]
```

`map()` defines the computation; `collect()` triggers execution and retrieves the result.

## 5.3 Essential transformations

### `map()` — one output per input

```
rdd = sc.parallelize([1, 2, 3, 4, 5])

result = rdd.map(lambda x: x * 3)

print(result.collect())
# [3, 6, 9, 12, 15]
```

### `filter()` — retain elements satisfying a condition

```
rdd = sc.parallelize([1, 2, 3, 4, 5])

result = rdd.filter(lambda x: x % 2 == 0)

print(result.collect())
# [2, 4]
```

### `flatMap()` — zero, one or multiple outputs per input

```
rdd = sc.parallelize([1, 2, 3])

result = rdd.flatMap(lambda x: [x, x * 10])

print(result.collect())
# [1, 10, 2, 20, 3, 30]
```

Unlike `map()`, `flatMap()` flattens the output collections into one RDD. It is particularly useful for converting lines of text into individual words.

### `union()` — combine RDDs

```
a = sc.parallelize([2, 4, 6])
b = sc.parallelize([1, 3, 5])

result = a.union(b)

print(result.collect())
# [2, 4, 6, 1, 3, 5]
```

`union()` does not automatically remove duplicates or globally sort the output.

### `distinct()` — remove duplicates

```
rdd = sc.parallelize([1, 2, 2, 3, 3])

result = rdd.distinct()

print(sorted(result.collect()))
# [1, 2, 3]
```

Removing duplicates across partitions commonly requires a shuffle.

### Other important transformations

- `sample()` — sample elements.
- `reduceByKey()` — aggregate values sharing a key.
- `groupByKey()` — group all values sharing a key.
- `join()` — combine matching key-value records.
- `sortByKey()` — sort key-value records by key.

## 5.4 Narrow versus wide transformations

### Narrow transformations

An output partition depends on a limited set of input partitions, usually allowing processing without redistributing data across the cluster.

Examples:

- `map()`
- `flatMap()`
- `filter()`
- `sample()`
- `union()` in the usual RDD dependency model

### Wide transformations

Output partitions may depend on data from multiple input partitions. These operations typically require a **shuffle**.

Examples:

- `distinct()`
- `reduceByKey()`
- `groupByKey()`
- `join()`
- `sortByKey()`

| Operation | Typical classification | Reason |
| --- | --- | --- |
| `map()` | Narrow | Processes elements locally |
| `flatMap()` | Narrow | Expands elements locally |
| `filter()` | Narrow | Retains or discards elements locally |
| `sample()` | Narrow | Sampling can occur within partitions |
| `union()` | Usually narrow | Combines existing partition sets |
| `distinct()` | Wide | Identifies duplicates across partitions |
| `reduceByKey()` | Wide overall | Aggregates values with matching keys |
| `groupByKey()` | Wide | Gathers values by key |
| `join()` | Often wide | Matching keys may require redistribution |
| `sortByKey()` | Wide overall | Global ordering generally requires redistribution |

These are typical classifications. Some execution strategies can avoid particular shuffles.

## 5.5 Shuffles

A shuffle redistributes data between partitions, often across the network.

It can involve:

- Network communication.
- Serialization and deserialization.
- Memory usage and disk I/O.
- Additional execution stages.

Shuffles are often expensive, so unnecessary shuffling should be minimised.

## 5.6 Essential actions

| Action | Purpose |
| --- | --- |
| `count()` | Count elements |
| `first()` | Return the first element |
| `take(n)` | Return up to the first `n` elements |
| `takeOrdered(n)` | Return the `n` smallest elements by default |
| `collect()` | Retrieve all elements at the driver |
| `reduce()` | Combine elements into one result |
| `foreach()` | Execute a function for every element |
| `saveAsTextFile()` | Write distributed text output |

Important details:

- `takeOrdered(3)` returns the three smallest elements by default unless a custom ordering is provided.
- `collect()` can exhaust driver memory on large datasets.
- `saveAsTextFile()` typically writes distributed output as multiple part files.
- `foreach()` runs operations on executors; it does not construct a normal Python list at the driver.
- An action triggers the computation needed for its requested result, not necessarily every possible computation in the application.

## 5.7 Lazy evaluation and lineage

```
rdd = sc.parallelize([1, 2, 3, 4, 5])

filtered = rdd.filter(lambda x: x > 2)
squared = filtered.map(lambda x: x * x)

print(squared.collect())
# [9, 16, 25]
```

Before `collect()`, the transformations define the computation. The action triggers the required execution.

**Fault tolerance:** If a partition is lost, Spark can often recompute it from its source data and recorded dependencies instead of keeping a complete backup of every intermediate RDD.

# 6\. Core PySpark exercises

The lecture includes three exercises based on _El Quijote_. Learn the patterns and be able to explain each step.

## 6.1 Exercise 1 — Count characters

**Task:** Count characters in the book, assuming four input partitions.

```
lines = sc.textFile("data/quijote.txt", 4)

character_counts = lines.map(lambda line: len(line))

total_characters = character_counts.reduce(
    lambda a, b: a + b
)

print(total_characters)
```

Concise alternative:

```
total_characters = lines.map(len).sum()
```

Steps:

1. `textFile()` creates an RDD of lines.
2. `map(len)` calculates the character count of each line.
3. `reduce()` or `.sum()` combines the counts.
4. The result is one total.

**Important:** `len(line)` counts characters in the line string returned by `textFile()`. Line terminators are generally not included. Account for them explicitly if the task requires counting newline separators.

## 6.2 Exercise 2 — Count occurrences of every word

```
lines = sc.textFile("data/quijote.txt", 4)

word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

word_counts.take(10)
```

Four steps:

1. Read the file as lines.
2. `flatMap(split)` converts lines into tokens.
3. `map(word → (word, 1))` assigns each occurrence a count of one.
4. `reduceByKey(+)` adds counts for matching words.

**Tokenisation caveat:** `split()` separates on whitespace. It does not automatically remove punctuation or normalise capitalisation. Therefore, `Mancha`, `Mancha,` and `mancha` may be counted separately.

An alternative that lowercases words and extracts word-like tokens:

```
import re

word_counts = (
    lines
    .flatMap(
        lambda line: re.findall(r"\b\w+\b", line.lower())
    )
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

This changes the counting rules, so results may differ from the lecture's examples.

## 6.3 Exercise 3 — Find the three most frequent words

Given the `word_counts` RDD:

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)

print(top_three)
```

Explanation:

- `pair[0]` is the word.
- `pair[1]` is the count.
- Negating the count places higher frequencies first.
- The word provides alphabetical tie-breaking.

The shorter version below also works, but tied counts do not have an explicit alphabetical tie-breaker:

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: -pair[1]
)
```

Any sample frequencies shown in lecture slides depend on the exact source text and tokenisation rules.

### Memorise the three patterns

- **Character count:** `map(len)` → `reduce()` or `.sum()`.
- **Word count:** `flatMap(split)` → `map((word, 1))` → `reduceByKey(+)`.
- **Most frequent words:** word-count RDD → `takeOrdered()` with a custom key.

# 7\. PySpark DataFrames

## 7.1 Definition and advantages

A **DataFrame** is a distributed table containing rows, named columns and an associated schema.

- Data is divided into partitions for parallel processing.
- The API supports structured operations using SQL-like expressions.
- Spark SQL can optimise structured queries through its query-planning and execution framework, including Catalyst optimisation.

RDDs are flexible low-level collections. DataFrames are generally preferable for structured analytics.

## 7.2 RDD versus DataFrame

| Feature | RDD | DataFrame |
| --- | --- | --- |
| Abstraction | Distributed collection | Structured table |
| Schema | No required tabular schema | Named columns and data types |
| Programming style | Functional, element-level processing | Declarative, SQL-like operations |
| Optimisation | Less automatic relational optimisation | Spark SQL query optimisation |
| Development | More low-level control | Usually simpler for structured analytics |
| Typical use | Custom element-level processing | Filtering, joins, aggregations and analytics |

**Default rule:** Prefer DataFrames for structured data. Use RDDs when low-level control or custom element-level processing is necessary.

## 7.3 Read JSON and inspect the schema

```
df = spark.read.format("json").load(
    "/data/genomics/json/summary.json"
)

df.printSchema()
df.show()
```

- `spark.read.format("json")` selects the JSON data source.
- `load()` reads the input into a DataFrame.
- `printSchema()` displays column names, data types and nested structure.
- `show()` displays a limited number of rows by default.

## 7.4 Select columns with `select()`

```
df.select("customer").show()

df.select("customer", "product").show()
```

`select()` returns a new DataFrame containing the requested columns or expressions.

## 7.5 Calculate columns with `col()`

```
from pyspark.sql.functions import col

df.select(
    "customer",
    "product",
    (col("quantity") * col("price")).alias("total")
).show()
```

The expression calculates `quantity × price` for each row. `alias("total")` names the resulting column.

Example from the lecture:

| Customer | Product | Quantity | Price | Total |
| --- | --- | --- | --- | --- |
| 200 | 5 | 8 | 33 | 264 |
| 200 | 8 | 5 | 111 | 555 |
| 100 | 8 | 5 | 45 | 225 |
| 100 | 8 | 5 | 59 | 295 |

## 7.6 SQL expressions with `selectExpr()`

```
df.selectExpr(
    "customer",
    "product",
    "quantity * price AS total"
).show()
```

To keep all original columns and add a calculated column:

```
df.selectExpr(
    "*",
    "quantity * price AS total"
).show()
```

Remember:

- `select()` accepts column names and Spark column expressions.
- `selectExpr()` accepts SQL expression strings.
- Both return new DataFrames.
- `"*"` selects all original columns in a SQL expression.

## 7.7 Filtering with `where()` and `filter()`

```
df.where(col("quantity") > 5).show()

df.filter(col("price") > 50).show()
```

These methods are equivalent for filtering rows.

Combine conditions using Spark column operators:

```
df.where(
    (col("quantity") > 0)
    & (col("price") > 0)
).show()
```

- `&` means logical AND.
- `|` means logical OR.
- Parenthesise individual comparisons.
- Do not use Python's `and` or `or` to combine Spark column expressions.

SQL-style expressions are also possible:

```
from pyspark.sql.functions import expr

df.where(expr("quantity > 5")).show()
```

## 7.8 Sorting with `orderBy()`

Ascending order is the default.

```
df.orderBy("price").show()

df.orderBy(col("price").desc()).show()
```

Multiple sort keys:

```
df.orderBy(
    col("customer").asc(),
    col("price").desc()
).show()
```

A distributed global sort may require a shuffle and can be expensive.

## 7.9 Aggregation with `groupBy()` and `agg()`

`groupBy()` defines the grouping columns. `agg()` defines the calculations applied to each group.

### Total quantity per customer

```
from pyspark.sql.functions import sum

df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity")
).show()
```

### Total sales value per customer

```
df.groupBy("customer").agg(
    sum(col("quantity") * col("price")).alias("total_sales")
).show()
```

### Multiple aggregate functions

```
from pyspark.sql.functions import sum, avg, max, min

df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity"),
    avg("price").alias("average_price"),
    max("price").alias("maximum_price"),
    min("price").alias("minimum_price")
).show()
```

Important aggregate functions:

- `sum()` — sum values.
- `avg()` — calculate an average.
- `max()` — find the maximum.
- `min()` — find the minimum.
- `count()` — count rows or non-null values, depending on the expression.

## 7.10 DataFrame patterns to memorise

| Requirement | Pattern |
| --- | --- |
| Read JSON | `spark.read.format("json").load(path)` |
| Inspect schema | `df.printSchema()` |
| Display rows | `df.show()` |
| Select columns | `df.select("a", "b")` |
| Calculate a column | `df.select((col("a") * col("b")).alias("total"))` |
| SQL expressions | `df.selectExpr("a", "b * c AS total")` |
| Filter rows | `df.where(col("quantity") > 0)` |
| Sort ascending | `df.orderBy("price")` |
| Sort descending | `df.orderBy(col("price").desc())` |
| Group and sum | `df.groupBy("customer").agg(sum("quantity"))` |

# 8\. Spark performance guidelines

Learn each rule, its rationale and an example.

## 8.1 Prefer `reduceByKey()` over `groupByKey()` for aggregation

```
rdd = sc.parallelize([
    ("A", 1),
    ("A", 1),
    ("B", 1),
    ("A", 1)
])

counts = rdd.reduceByKey(lambda a, b: a + b)

print(sorted(counts.collect()))
# [('A', 3), ('B', 1)]
```

`reduceByKey()` aggregates values for matching keys and can combine values locally before the shuffle, reducing data transferred across the network.

By contrast:

```
groups = rdd.groupByKey()
```

`groupByKey()` gathers all values associated with each key. This can require more network transfer and memory when many values share a key.

**Rule:** Use `reduceByKey()` for sums, counts and similar aggregations. Use `groupByKey()` when the task genuinely requires all values associated with each key.

## 8.2 Minimise shuffles

Shuffle-intensive operations commonly include:

- `distinct()`
- `groupByKey()`
- `reduceByKey()`
- Global sorting.
- Many joins.

Reduce shuffle overhead by:

- Filtering unnecessary records early.
- Avoiding redundant wide transformations.
- Choosing suitable aggregation operations.
- Using appropriate partitioning and join strategies.
- Avoiding unnecessary movement of large datasets to the driver.

Not every transformation causes a shuffle. `map()` and `filter()` are normally narrow transformations.

## 8.3 Cache datasets that will be reused

```
filtered = lines.filter(
    lambda line: "Spark" in line
).cache()

print(filtered.count())
print(filtered.take(10))
```

Caching can avoid recomputation when a costly intermediate dataset is used by multiple actions.

Important points:

- The first action computes the required data.
- `.cache()` marks the dataset for caching; it does not immediately compute every partition.
- Cached partitions can be reused when available.
- Caching consumes memory and potentially disk space.
- Lost cached partitions may need to be recomputed.
- Cache when the benefit of reuse justifies the storage cost.

## 8.4 Avoid large `collect()` operations

```
result = rdd.collect()
```

`collect()` transfers every RDD element to the driver. Large results can exhaust driver memory.

Prefer an operation suited to the task:

- `take(10)` — inspect a small sample.
- `reduce()` — calculate a single aggregate.
- `saveAsTextFile()` — write distributed output.

## 8.5 Choose an appropriate number of partitions

- **Too few partitions:** May limit parallelism and produce oversized tasks.
- **Too many tiny partitions:** Can increase scheduling overhead and create many small output files.
- **Appropriate partitioning:** Depends on data size, resources, task costs and the operation.

The number of partitions need not equal the number of worker machines. One worker can process multiple partitions.

### Performance rules — one-line revision

| Rule | Reason |
| --- | --- |
| Prefer `reduceByKey()` for aggregation | Local combining can reduce shuffle traffic |
| Minimise shuffles | Reduce communication and intermediate-data costs |
| Cache reused datasets | Avoid repeated computation |
| Avoid large `collect()` operations | Prevent driver memory exhaustion |
| Choose suitable partition counts | Balance parallelism and scheduling overhead |
| Prefer DataFrames for structured data | Enable structured query optimisation |

# 9\. Spark Streaming

## 9.1 Purpose

Spark Streaming processes incoming data streams, such as social media messages, click events, application logs and network data.

The classic Spark Streaming API processes data in **micro-batches**. Incoming events are grouped into small time-based batches rather than necessarily being processed individually as they arrive.

## 9.2 DStreams

A **DStream (Discretized Stream)** is the fundamental abstraction in the original Spark Streaming API.

- A DStream represents a time-ordered sequence of RDDs.
- Each RDD contains data associated with a particular batch interval.
- The RDDs can be processed using familiar Spark operations.
- Spark Streaming coordinates batch creation and processing.

Conceptually:

```
Continuous incoming data
          |
          v
   Batch interval 1 ---> RDD 1
   Batch interval 2 ---> RDD 2
   Batch interval 3 ---> RDD 3
          |
          v
Apply Spark operations to each batch
          |
          v
        Results
```

## 9.3 StreamingContext and batch intervals

In the classic DStream API, `StreamingContext` is the entry point for streaming functionality. It requires a SparkContext and a batch interval.

General workflow:

1. Create a SparkContext.
2. Create a StreamingContext with the desired batch interval.
3. Define the input stream.
4. Apply transformations and output operations.
5. Start the streaming computation.
6. Keep the application running as needed.

A shorter batch interval can reduce waiting time but may increase scheduling overhead. If processing cannot keep up with incoming data, the application may fall behind.

## 9.4 Example — count words received from a TCP socket

**Task:** Count words received from a server listening on port `9999`.

```
from pyspark import SparkContext
from pyspark.streaming import StreamingContext

sc = SparkContext(appName="StreamingWordCount")

ssc = StreamingContext(sc, 1)

lines = ssc.socketTextStream("localhost", 9999)

word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

word_counts.pprint()

ssc.start()
ssc.awaitTermination()
```

Explanation:

1. `SparkContext` establishes the Spark application context.
2. `StreamingContext(sc, 1)` configures a one-second batch interval.
3. `socketTextStream()` reads text from the TCP socket.
4. `flatMap()` extracts individual words.
5. `map()` converts each word into a `(word, 1)` pair.
6. `reduceByKey()` sums word counts within each batch.
7. `pprint()` displays the results.
8. `start()` starts execution.
9. `awaitTermination()` keeps the application waiting for termination.

The socket server must be running and accessible. The environment must support the classic streaming API. Production systems also need appropriate source reliability, recovery, checkpointing and processing guarantees.

## 9.5 DStreams versus Structured Streaming

| Feature | Classic Spark Streaming | Structured Streaming |
| --- | --- | --- |
| Fundamental abstraction | DStreams, a sequence of RDDs | Structured DataFrames/Datasets |
| Programming style | RDD-oriented | Declarative, SQL-like |
| Query optimisation | Less structured-query optimisation | Uses Spark SQL optimisation |
| Batch integration | Separate classic abstraction | Similar programming model to batch DataFrames |
| Recommendation | Understand for legacy applications and this lecture | Generally preferred for new applications |

Structured Streaming supports incremental processing and commonly uses micro-batch execution. Continuous processing is available only for supported configurations and operations; it is not guaranteed for every query.

**Exam takeaway:** Explain DStreams because they are taught explicitly, then distinguish them from Structured Streaming.

# 10\. Closed-book memory sheet

## 10.1 Essential definitions

| Term | Definition to reproduce |
| --- | --- |
| Apache Spark | Distributed processing engine for large-scale data |
| RDD | Immutable, partitioned collection processed in parallel |
| Transformation | Defines a new RDD or computation from existing data |
| Action | Triggers required execution and returns a result or writes output |
| Lazy evaluation | Defers computation until an action requires a result |
| DAG | Directed acyclic graph of computation dependencies |
| Narrow transformation | Output partition depends on a limited set of input partitions; normally no shuffle |
| Wide transformation | Output partition depends on multiple input partitions; typically requires a shuffle |
| Shuffle | Redistribution of data across partitions |
| Lineage | Recorded dependencies that can help Spark recompute lost partitions |
| DataFrame | Distributed table with named columns and a schema |
| Driver | Coordinates the application and schedules tasks |
| Executor | Process that executes tasks and may cache intermediate data |
| Partition | Unit of distributed data processing |
| DStream | Time-ordered sequence of RDDs in classic Spark Streaming |
| StreamingContext | Entry point for the classic DStream streaming API |
| `reduceByKey()` | Aggregates values with matching keys |
| `collect()` | Retrieves all RDD elements at the driver |

## 10.2 Essential RDD code

```
# Create RDDs
rdd = sc.parallelize([1, 2, 3, 4, 5], 4)
lines = sc.textFile("data/book.txt")

# Transformations
rdd.map(lambda x: x * 2)
rdd.filter(lambda x: x % 2 == 0)
rdd.flatMap(lambda x: [x, x * 10])
rdd.distinct()
rdd.union(other_rdd)

# Actions
rdd.count()
rdd.first()
rdd.take(3)
rdd.takeOrdered(3)
rdd.collect()
rdd.reduce(lambda a, b: a + b)

# Word count
counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

# Top three frequencies, with alphabetical tie-breaking
top_three = counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

## 10.3 Essential DataFrame code

```
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, sum

spark = (
    SparkSession.builder
    .appName("DataManagement")
    .getOrCreate()
)

df = spark.read.format("json").load("data.json")

df.printSchema()
df.show()

# Select columns
df.select("customer", "product")

# Calculate a column
df.select(
    "customer",
    (col("quantity") * col("price")).alias("total")
)

# SQL expressions
df.selectExpr(
    "*",
    "quantity * price AS total"
)

# Filter
df.where(col("quantity") > 0)

# Sort descending
df.orderBy(col("price").desc())

# Group and aggregate
df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity")
)
```

## 10.4 Five performance rules

1. Prefer `reduceByKey()` over `groupByKey()` when aggregating values.
2. Minimise unnecessary shuffles.
3. Cache expensive datasets that are reused.
4. Avoid `collect()` on large datasets.
5. Choose an appropriate number of partitions.

## 10.5 Five distinctions to know

1. Transformation versus action.
2. Narrow versus wide transformation.
3. RDD versus DataFrame.
4. Driver versus executor.
5. DStream versus Structured Streaming.

# 11\. Exam-style questions and model answers

These are practice questions derived from the supplied material, not predictions of the actual examination.

### Q1. Why is distributed processing needed?

**Answer:** A single machine has finite CPU, memory and storage. Spark distributes data into partitions and executes tasks across multiple machines, allowing horizontal scaling and processing beyond the capacity of one machine.

### Q2. Explain the roles of the driver and executors.

**Answer:** The driver runs the application, constructs execution plans, schedules tasks and coordinates execution. Executors run tasks on worker resources and can cache intermediate data.

### Q3. What is an RDD?

**Answer:** An RDD is an immutable, distributed collection of elements processed in parallel. Its lineage can help Spark recompute lost partitions.

### Q4. Explain lazy evaluation.

**Answer:** Transformations normally record computations without executing them immediately. When an action requests a result, Spark executes the computations required to produce it.

### Q5. Distinguish transformations from actions.

**Answer:** Transformations define new RDDs or computations, such as `map()` and `filter()`. Actions request results or output, such as `count()`, `collect()` and `saveAsTextFile()`. Actions trigger the required execution.

### Q6. What is the difference between narrow and wide transformations?

**Answer:** Narrow transformations normally process data locally within limited partition dependencies. Wide transformations depend on data from multiple partitions and typically require a shuffle, which introduces communication and processing overhead.

### Q7. Why is `reduceByKey()` preferred over `groupByKey()` for word counting?

**Answer:** `reduceByKey()` can combine values locally before the shuffle, reducing data transfer. `groupByKey()` gathers all values for each key and is less efficient when only an aggregate is needed.

### Q8. What happens when `collect()` is called?

**Answer:** Spark computes the required result and transfers all RDD elements to the driver as a local collection. A large result can exhaust driver memory.

### Q9. Write a word-count solution.

**Answer:**

```
word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

`flatMap()` splits lines into tokens, `map()` assigns a count of one to each token, and `reduceByKey()` sums counts for matching words.

### Q10. Find the three most frequent words.

**Answer:**

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

The negative count puts higher frequencies first, and the word provides alphabetical tie-breaking.

### Q11. Why prefer DataFrames for structured data?

**Answer:** DataFrames provide named columns, schemas and declarative operations. Spark SQL can optimise structured operations, making filtering, sorting, joining and aggregation easier to express and often more efficient.

### Q12. How do `select()` and `selectExpr()` differ?

**Answer:** `select()` accepts column names and Spark column expressions. `selectExpr()` accepts SQL expression strings. Both return new DataFrames and can select or calculate columns.

### Q13. How do `groupBy()` and `agg()` work together?

**Answer:** `groupBy()` defines the grouping columns, while `agg()` defines aggregate functions such as `sum()`, `avg()`, `min()`, `max()` and `count()`.

### Q14. What is a DStream?

**Answer:** A DStream is the classic Spark Streaming abstraction representing a stream as a time-ordered sequence of RDDs, with each RDD corresponding to a batch interval.

### Q15. How does Structured Streaming differ from DStreams?

**Answer:** Classic Spark Streaming uses DStreams and RDDs. Structured Streaming uses structured DataFrames/Datasets and Spark SQL's execution framework, providing a more declarative interface and query optimisation.

### Q16. Why does four worker nodes not necessarily mean four partitions?

**Answer:** Partitions are units of distributed processing, while workers are machines providing resources. A worker can process multiple partitions, so the number of partitions and the number of workers need not match.

### Q17. Why might caching improve performance?

**Answer:** Caching can prevent expensive transformations from being recomputed when the same dataset is reused by multiple actions. However, caching consumes storage resources and is beneficial only when the savings justify the cost.

# 12\. Common exam mistakes

- Claiming that transformations execute immediately.
- Claiming that transformations modify an existing RDD.
- Confusing the number of partitions with the number of worker machines.
- Claiming that every transformation causes a shuffle.
- Using `groupByKey()` for a simple sum when `reduceByKey()` is more appropriate.
- Calling `collect()` on a large distributed dataset without considering driver memory.
- Confusing the driver with an executor.
- Assuming `union()` removes duplicates or sorts data.
- Assuming `split()` removes punctuation or normalises capitalisation.
- Forgetting the negative sign in the `takeOrdered()` top-frequency pattern.
- Forgetting that ties need an explicit tie-breaking rule if deterministic ordering is required.
- Using Python's `and` or `or` to combine Spark column expressions.
- Confusing `groupBy()` with `agg()`.
- Describing a DStream as one RDD containing the entire continuous stream.
- Claiming that Structured Streaming always operates in continuous-processing mode.
- Treating the lecture's “up to 100 times faster” claim as a guarantee for every workload.
- Assuming `.cache()` immediately computes every partition.
- Assuming `takeOrdered(n)` returns the largest values by default.

# 13\. Final revision checklist

Before moving on to T7, make sure you can:

- [ ]Explain why Spark is useful for large datasets.
- [ ]Explain vertical versus horizontal scaling.
- [ ]Identify the roles of the driver, SparkContext, SparkSession, cluster manager, workers and executors.
- [ ]Describe Spark Core, Spark SQL, Streaming, MLlib and GraphX.
- [ ]Define RDD and explain immutability, lineage and fault tolerance.
- [ ]Create RDDs using `textFile()` and `parallelize()`.
- [ ]Classify common transformations and actions.
- [ ]Explain lazy evaluation and DAGs.
- [ ]Distinguish narrow and wide transformations and explain shuffles.
- [ ]Write the character-count exercise.
- [ ]Write the word-count exercise.
- [ ]Find the three most frequent words using `takeOrdered()`.
- [ ]Explain `reduceByKey()` versus `groupByKey()`.
- [ ]Explain when caching is useful.
- [ ]Explain why large `collect()` operations are dangerous.
- [ ]Read a JSON DataFrame and inspect its schema.
- [ ]Use `select()`, `selectExpr()`, `where()`, `filter()` and `orderBy()`.
- [ ]Aggregate values using `groupBy()` and `agg()`.
- [ ]Explain all five performance guidelines.
- [ ]Explain DStreams, StreamingContext and batch intervals.
- [ ]Compare classic Spark Streaming with Structured Streaming.
- [ ]Recall the introductory NoSQL, ACID, BASE and CAP concepts.

---

## Chapter completion

**T6 covers:** distributed computing, Spark architecture, RDDs, transformations, actions, PySpark exercises, DataFrames, performance and streaming.

**Suggested filename:** `T6-Spark-Exam-Compression.md`

Keep the original lecture material alongside this compression whenever possible. The compression is a revision aid; the lecture remains the source of truth for the course's emphasis and terminology.

### Workflow for the five chapters

- Save this file separately as the T6 compression.
- Keep the original T6 lecture notes in the repository.
- Create equivalent compression files for T7–T10, using the actual lecture material for each chapter.
- Once all five chapters are ready, use the five compressions and, ideally, the original lecture notes to construct a balanced practice examination.

The practice exam should include conceptual questions, code interpretation, debugging, short-answer questions, practical exercises, worked solutions, an answer key and a marking rubric.

**Important:** This merged document consolidates the two supplied chunks and removes repeated material. It cannot guarantee that every examinable detail from the original lecture slides is present, particularly details contained only in figures or diagrams.
