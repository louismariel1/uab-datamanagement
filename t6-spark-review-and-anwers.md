# T6 — Spark: Review Questions and Answers

**Course:** Big Data / Distributed Data Processing\
**Chapter:** T6 — Apache Spark\
**Purpose:** Exam preparation using lecturer-identified review questions and concepts from the lecture notes.

---

## 1\. Lecturer's Explicit Review Questions

The lecture notes explicitly identify the following conceptual questions as particularly valuable. These should receive **high priority during exam preparation**.

### Q1. Which operations trigger a shuffle in Apache Spark?

**Answer:**

A shuffle occurs when Spark must redistribute data across partitions, usually across worker nodes, to complete an operation.

Examples of operations that can trigger a shuffle include:

- `distinct()`
- `reduceByKey()`
- `groupByKey()`
- `join()`
- `sortByKey()`
- `groupBy()` and many aggregations on DataFrames
- `orderBy()` / `sort()` on DataFrames

**Explanation:**

A shuffle is necessary when an output partition depends on data from multiple input partitions. Spark must exchange data across the cluster so that related records can be processed together.

For example, consider this RDD:

```
pairs = sc.parallelize([
    ("apple", 1),
    ("banana", 1),
    ("apple", 1)
])
```

The operation:

```
pairs.reduceByKey(lambda a, b: a + b)
```

must combine values belonging to the same key. The two `"apple"` records may initially reside in different partitions, so Spark may need to redistribute data.

**Why shuffles matter:**

- They involve network communication.
- They may require disk I/O and additional memory.
- They are generally more expensive than partition-local operations.

**Exam tip:** Distinguish narrow transformations, such as `map()` and `filter()`, from wide transformations, such as `groupByKey()` and `reduceByKey()`. Wide transformations commonly require a shuffle.

**Important nuance:** Whether an operation actually shuffles data can depend on the execution plan, partitioning, and optimizations. For example, some joins can avoid a shuffle when suitable partitioning already exists.

---

### Q2. Why is `reduceByKey()` preferred over `groupByKey()`?

**Answer:**

`reduceByKey()` is generally preferred when the objective is to aggregate values for each key because it can combine values locally before redistributing data across the cluster.

**Explanation:**

Suppose the input is:

```
pairs = sc.parallelize([
    ("apple", 1),
    ("apple", 1),
    ("banana", 1),
    ("apple", 1)
])
```

Using `reduceByKey()`:

```
counts = pairs.reduceByKey(lambda a, b: a + b)
```

The result is equivalent to:

```
("apple", 3)
("banana", 1)
```

Spark can perform local aggregation within partitions before the shuffle. Instead of sending every individual value across the network, it can send partial aggregates.

By contrast:

```
grouped = pairs.groupByKey()
```

groups all values for each key into an iterable. It does not aggregate those values into a single result.

Conceptually, the grouped result is:

```
("apple", [1, 1, 1])
("banana", [1])
```

**Comparison:**

| Feature | `reduceByKey()` | `groupByKey()` |
| --- | --- | --- |
| Purpose | Aggregate values by key | Collect values by key |
| Output | One combined value per key | Iterable of values per key |
| Local aggregation | Can combine values before shuffle | Does not perform equivalent reduction |
| Network traffic | Often lower for aggregations | Can be substantially higher |
| Preferred for sums/counts | Yes | Usually no |

**Exam tip:** If asked to count words, use `reduceByKey()` rather than `groupByKey()`.

**Important condition:** The reduction function must be associative and commutative for distributed aggregation to be safely independent of the order of combination. Addition and multiplication are common examples.

---

### Q3. What happens when `collect()` is executed?

**Answer:**

`collect()` triggers the execution of the transformations needed to produce the RDD's results and returns all elements to the driver program as a local collection.

Example:

```
rdd = sc.parallelize([1, 2, 3, 4])
result = rdd.map(lambda x: x * 2).collect()

print(result)
```

Output:

```
[2, 4, 6, 8]
```

**Explanation:**

The `map()` transformation is lazy. It does not immediately calculate the doubled values. When `collect()` is called, Spark executes the required computation and transfers the results to the driver.

**Risks of****`collect()`****:**

- The complete result must fit in the driver's available memory.
- Large results can cause excessive network traffic.
- Collecting too much data can crash the driver due to memory exhaustion.

**Safer alternatives when appropriate:**

```
rdd.take(10)       # Retrieve at most 10 elements
rdd.count()        # Retrieve only the number of elements
rdd.saveAsTextFile("output")  # Write results to storage
```

**Exam tip:** `collect()` is an action, not a transformation. It returns data to the driver; it does not create a new RDD.

---

### Q4. Which operations are transformations and which are actions?

**Answer:**

**Transformations** create a new RDD from an existing RDD. They are evaluated lazily.

**Actions** trigger the execution of the computation needed to produce a result, return information to the driver, or write results to external storage.

#### Common transformations

| Operation | Purpose |
| --- | --- |
| `map()` | Apply a function to each element |
| `flatMap()` | Apply a function that can produce zero, one, or many outputs per input |
| `filter()` | Keep elements satisfying a condition |
| `sample()` | Sample elements |
| `union()` | Combine two RDDs |
| `distinct()` | Remove duplicate elements |
| `reduceByKey()` | Aggregate values by key |
| `groupByKey()` | Group values by key |
| `join()` | Combine matching key-value records |
| `sortByKey()` | Sort key-value records by key |

#### Common actions

| Operation | Purpose |
| --- | --- |
| `count()` | Return the number of elements |
| `collect()` | Return all elements to the driver |
| `first()` | Return the first element |
| `take(n)` | Return up to the first `n` elements |
| `takeOrdered(n)` | Return the smallest `n` elements by ordering |
| `reduce()` | Aggregate all elements into one value |
| `foreach()` | Execute a function on each element |
| `saveAsTextFile()` | Write elements to text files |

**Explanation with an example:**

```
numbers = sc.parallelize([1, 2, 3, 4, 5])

evens = numbers.filter(lambda x: x % 2 == 0)
doubled = evens.map(lambda x: x * 2)

result = doubled.collect()
```

Execution:

1. `filter()` defines a transformation.
2. `map()` defines another transformation.
3. `collect()` triggers the computation.
4. The driver receives `[4, 8]`.

**Exam tip:** Ask yourself whether the operation creates a new distributed dataset or requests a computed result. The former is a transformation; the latter is an action.

---

## 2\. Additional High-Priority Questions Derived from the Lecture

The following are not additional questions explicitly listed in the lecturer's review section. They are derived from the lecture objectives, examples, and key takeaways.

### Q5. Why is distributed data processing necessary for large datasets?

**Answer:**

Distributed processing divides data and computation across multiple machines, allowing a cluster to process datasets that may exceed the memory or computational capacity of a single computer.

**Explanation:**

Pandas is convenient for small and medium-sized datasets, but its usual execution model relies on the resources of a single machine.

Apache Spark distributes computation across multiple worker nodes.

Benefits include:

- **Horizontal scalability:** Add worker nodes to increase aggregate resources.
- **Parallel processing:** Different partitions can be processed concurrently.
- **Large-scale processing:** Use combined resources across a cluster.
- **Fault recovery:** Spark can recompute lost RDD partitions using lineage information.

**Exam tip:** Vertical scaling increases the resources of one machine; horizontal scaling adds machines to a system.

---

### Q6. Why can Spark be faster than Hadoop MapReduce for iterative workloads?

**Answer:**

Spark can cache intermediate datasets in memory and reuse them across operations, reducing repeated disk reads and writes.

**Explanation:**

Traditional Hadoop MapReduce workflows often materialize intermediate results on disk between processing stages. Spark can retain reusable data in memory or persist it to disk when appropriate.

This is especially beneficial for:

- Iterative machine learning.
- Repeated analysis of the same dataset.
- Interactive data exploration.
- Workloads with reusable intermediate results.

**Important qualification:** Spark is not guaranteed to be faster for every workload. Performance depends on data size, memory availability, shuffle costs, storage, configuration, and workload characteristics. The lecture's statement that Spark can be up to 100 times faster is a reported potential advantage, not a universal guarantee.

---

### Q7. Describe the architecture of Apache Spark.

**Answer:**

Spark uses a driver-and-executor architecture coordinated through a cluster manager.

- **Driver:** Runs the main application logic and coordinates execution.
- **SparkContext:** In the classic RDD API, provides an entry point for interacting with Spark and submitting work. Modern applications commonly use `SparkSession`, which provides access to Spark SQL and other APIs.
- **Cluster manager:** Allocates resources for the application. Examples include Standalone, YARN, and Kubernetes.
- **Executors:** Run tasks on worker machines and can cache or persist data.

**Execution sequence:**

1. The application starts on the driver.
2. The driver requests resources.
3. Executors are allocated.
4. Spark divides jobs into stages and tasks.
5. Executors process partitions in parallel.
6. Results are returned to the driver or written to storage.

**Exam tip:** The lecture simplifies this as Master–Worker. In a more precise explanation, the driver coordinates the application, executors run tasks, and the cluster manager allocates resources. The driver is not necessarily a dedicated master node of the cluster manager.

---

### Q8. What is an RDD, and why is it resilient?

**Answer:**

An RDD (Resilient Distributed Dataset) is an immutable, distributed collection of elements that Spark can process in parallel.

**Core properties:**

- **Resilient:** Lost partitions can often be recomputed from lineage.
- **Distributed:** Data is divided into partitions across the cluster.
- **Immutable:** Transformations create new RDDs instead of modifying existing ones.
- **Parallelizable:** Partitions can be processed concurrently.
- **Lazy:** Transformations are generally evaluated only when required by an action.

**Example:**

```
rdd1 = sc.parallelize([1, 2, 3])
rdd2 = rdd1.map(lambda x: x * 2)
```

`rdd1` remains unchanged. `rdd2` represents the transformed dataset.

**Exam tip:** RDD fault tolerance is based on lineage and recomputation; it does not mean that every partition is automatically replicated in memory.

---

### Q9. Explain lazy evaluation in Spark.

**Answer:**

Lazy evaluation means Spark records transformations without immediately executing them. The computation is triggered when an action requires a result.

Example:

```
rdd = sc.parallelize([1, 2, 3, 4])

filtered = rdd.filter(lambda x: x > 2)
mapped = filtered.map(lambda x: x * 10)

result = mapped.collect()
```

Before `collect()`, Spark has defined the computation but generally has not executed the full transformation chain. When `collect()` runs, Spark evaluates the necessary transformations.

**Advantages:**

- Allows Spark to construct an execution plan.
- Can enable optimization.
- Avoids computing intermediate results that are never needed.
- Allows multiple operations to be organized into jobs and stages.

**Exam tip:** Transformations are lazy; actions trigger execution. Not every action returns data to the driver, because actions such as `saveAsTextFile()` write results to storage.

---

### Q10. What is the difference between narrow and wide transformations?

**Answer:**

A **narrow transformation** produces each output partition from data in a single input partition. A **wide transformation** may require data from multiple input partitions and commonly requires a shuffle.

| Property | Narrow | Wide |
| --- | --- | --- |
| Partition dependency | Output depends on one input partition | Output can depend on multiple input partitions |
| Shuffle | Generally unnecessary | Commonly required |
| Communication overhead | Usually lower | Usually higher |
| Examples | `map()`, `filter()`, `flatMap()` | `groupByKey()`, `reduceByKey()`, `distinct()` |

**Explanation:**

If `filter()` removes some elements, each partition can usually process its own data independently.

For `reduceByKey()`, records with the same key may be distributed across different partitions. Spark must combine them, which commonly requires data exchange.

**Nuance:** `union()` is often classified as narrow when it simply preserves input partitions, but the exact dependency structure can vary. Focus on whether data must be redistributed across partitions.

---

### Q11. What is the difference between `map()` and `flatMap()`?

**Answer:**

`map()` produces one output element for each input element, whereas `flatMap()` can produce zero, one, or many output elements per input and flattens the resulting collections.

Example:

```
rdd = sc.parallelize([1, 2, 3])

rdd.map(lambda x: [x, x * 10]).collect()
# [[1, 10], [2, 20], [3, 30]]

rdd.flatMap(lambda x: [x, x * 10]).collect()
# [1, 10, 2, 20, 3, 30]
```

**Exam tip:** Word counting commonly uses `flatMap()` to convert lines of text into individual words.

---

### Q12. How can Spark count the characters in a book using RDDs?

**Answer:**

Read the file as lines, calculate the length of each line, and sum those lengths.

```
lines = sc.textFile("quijote.txt")

character_count = lines.map(lambda line: len(line)).sum()

print(character_count)
```

**Explanation:**

1. `textFile()` creates an RDD in which each element represents a line.
2. `map()` calculates the number of characters in each line.
3. `sum()` aggregates the lengths and triggers execution.

**Important detail:** This counts characters in the line strings but does not count newline separators removed by line-based input reading. If the question defines characters to include line terminators, the calculation must account for them explicitly.

---

### Q13. How can Spark count the frequency of each word in a book?

**Answer:**

Use `flatMap()` to produce words, map each word to `(word, 1)`, and aggregate with `reduceByKey()`.

```
lines = sc.textFile("quijote.txt")

word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

word_counts.take(10)
```

**Explanation:**

- `flatMap()` converts each line into multiple words.
- `map()` assigns each word a count of one.
- `reduceByKey()` adds the counts for identical keys.

This is the classic MapReduce pattern:

```
Input lines
    ↓
Map: (word, 1)
    ↓
Shuffle and combine matching keys
    ↓
Reduce: (word, total_count)
```

**Important detail:** `split()` is a simple tokenizer. It does not normalize capitalization or remove punctuation. For example, `"Mancha"` and `"mancha"` remain separate keys. If required, normalize the text before counting.

---

### Q14. How can Spark find the three most frequent words?

**Answer:**

First calculate word counts, then retrieve the three highest counts.

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)

print(top_three)
```

**Explanation:**

The key function sorts by descending frequency because the count is negated. The word provides an alphabetical tie-breaker.

For a small result, `takeOrdered()` is convenient because it returns only the requested number of elements to the driver.

**Exam tip:** `takeOrdered(3)` by itself returns the three smallest elements under the default ordering. To obtain the most frequent words, define the ordering explicitly or use another suitable descending-sort approach.

---

### Q15. What is PySpark?

**Answer:**

PySpark is the Python API for Apache Spark. It allows developers to write distributed data-processing applications using Python.

It supports:

- RDD-based processing.
- DataFrame and SQL operations.
- Distributed data transformation and aggregation.
- Machine learning through Spark's ML libraries.
- Batch processing and streaming APIs.

**Explanation:**

PySpark lets developers use Python to describe computations that Spark executes across a cluster. It is not simply ordinary Python running on one machine.

---

### Q16. How can an RDD be created using `SparkContext`?

**Answer:**

Two common approaches are `textFile()` and `parallelize()`.

**Read a file:**

```
lines = sc.textFile("data.txt")
```

This creates an RDD whose elements are lines from the input file.

**Parallelize a local collection:**

```
numbers = sc.parallelize([1, 2, 3, 4, 5])
```

This distributes elements from a local collection into partitions for processing.

**Explanation:**

- Use `textFile()` when reading data from a supported storage system.
- Use `parallelize()` when distributing a collection already available in the driver.

**Important detail:** The number of partitions can be influenced by the input and configuration; it should not automatically be assumed to equal the number of worker nodes.

---

### Q17. What is a DataFrame, and how does it differ from an RDD?

**Answer:**

A Spark DataFrame is a distributed table organized into named columns with associated data types. An RDD is a lower-level distributed collection of objects.

| Property | RDD | DataFrame |
| --- | --- | --- |
| Abstraction | Low-level collection | Structured table |
| Operations | Python functions and functional transformations | Column expressions, SQL-like operations |
| Optimization | Less automatic relational optimization | Optimized through Spark SQL's Catalyst optimizer |
| Ease of development | More low-level control | Often simpler for structured analysis |
| Typical use | Specialized low-level processing | Structured data and analytics |

**Explanation:**

DataFrames allow Spark to understand the structure and intent of many operations, enabling query optimization and efficient execution.

**Exam tip:** Prefer DataFrames for structured data unless low-level RDD control is necessary.

---

### Q18. How do you create a DataFrame from a JSON file and inspect its schema?

**Answer:**

```
df = spark.read.format("json").load("/data/summary.json")

df.printSchema()
```

**Explanation:**

- `spark.read.format("json")` selects the JSON data source.
- `load()` reads the data into a DataFrame.
- `printSchema()` displays column names, data types, and nested structure.

The actual schema depends on the input data and schema inference or the explicitly supplied schema.

---

### Q19. What is the difference between `select()` and `selectExpr()`?

**Answer:**

Both select DataFrame columns and can create derived columns, but they use different expression styles.

**Using****`select()`****with column expressions:**

```
from pyspark.sql.functions import col

result = df.select(
    "customer",
    "product",
    (col("quantity") * col("price")).alias("total")
)
```

**Using****`selectExpr()`****with SQL expression strings:**

```
result = df.selectExpr(
    "customer",
    "product",
    "quantity * price AS total"
)
```

**Explanation:**

- `select()` works naturally with column names and expression objects.
- `selectExpr()` allows SQL-like expressions written as strings.

Both examples produce a DataFrame with `customer`, `product`, and `total` columns.

**Exam tip:** `alias("total")` and `AS total` give a calculated column a new name.

---

### Q20. How do you filter and sort DataFrame rows?

**Answer:**

Use `where()` or `filter()` to retain rows satisfying a condition, and `orderBy()` or `sort()` to order rows.

**Filter:**

```
filtered = df.where(col("quantity") > 5)
```

Equivalent:

```
filtered = df.filter(col("quantity") > 5)
```

**Sort ascending:**

```
sorted_df = df.orderBy(col("price").asc())
```

**Sort descending:**

```
sorted_df = df.orderBy(col("price").desc())
```

**Explanation:**

These operations return new DataFrames rather than modifying the original DataFrame.

**Exam tip:** Ascending order is the default for ordinary column sorting unless descending order is specified.

---

### Q21. How do you aggregate DataFrame data using `groupBy()` and `agg()`?

**Answer:**

Use `groupBy()` to group rows by one or more columns and `agg()` to calculate aggregate values for each group.

Example: total quantity purchased by each customer.

```
from pyspark.sql.functions import sum

result = (
    df.groupBy("customer")
      .agg(sum("quantity").alias("total_quantity"))
)
```

Example: multiple aggregates.

```
from pyspark.sql.functions import avg, max, min, sum

result = (
    df.groupBy("customer")
      .agg(
          sum("quantity").alias("total_quantity"),
          avg("price").alias("average_price"),
          max("price").alias("maximum_price"),
          min("price").alias("minimum_price")
      )
)
```

**Explanation:**

1. `groupBy("customer")` defines the groups.
2. `agg()` specifies calculations applied to each group.
3. Spark returns a summarized DataFrame.

**Important performance note:** Grouping and aggregation commonly involve a shuffle because records for the same grouping key must be brought together.

---

### Q22. What is Spark Streaming, and what is a DStream?

**Answer:**

Spark Streaming is a legacy Spark streaming API that processes incoming data as a sequence of small batches. A DStream (discretized stream) represents a continuous stream as a sequence of RDDs.

**How it works:**

1. Data arrives from a source.
2. Spark collects data over a batch interval.
3. Each interval produces an RDD.
4. Spark processes the RDD using distributed operations.
5. Results are delivered or stored.

**Key concepts:**

- **DStream:** A sequence of RDDs representing data over time.
- **StreamingContext:** The entry point for the legacy Spark Streaming API; it is configured with a SparkContext and a batch interval.
- **Micro-batching:** Incoming data is processed in small time-based batches rather than as one uninterrupted operation.

**Example use cases:** Processing log events, social media messages, and incoming application events.

**Exam tip:** The legacy DStream API is RDD-based. Structured Streaming is the more modern API and integrates with Spark's DataFrame and SQL model.

---

### Q23. What is the difference between DStreams and Structured Streaming?

**Answer:**

DStreams are based on sequences of RDDs, while Structured Streaming uses the structured DataFrame/Dataset programming model.

| Property | DStreams | Structured Streaming |
| --- | --- | --- |
| Data abstraction | Sequence of RDDs | Structured DataFrames/Datasets |
| Programming model | Lower-level streaming operations | Similar to batch DataFrame/SQL operations |
| Optimization | Less integrated with relational query optimization | Uses Spark SQL's optimization infrastructure |
| Development | Often requires more low-level logic | Often easier to develop and maintain |
| Current recommendation | Legacy API | Preferred for most new Spark streaming applications |

**Explanation:**

Structured Streaming allows developers to describe transformations using familiar structured-data operations while Spark manages the streaming execution.

---

## 3\. Quick-Fire Exam Facts

| Question | Answer |
| --- | --- |
| What does RDD stand for? | Resilient Distributed Dataset |
| Are RDDs mutable? | No; they are immutable |
| What does `map()` do? | Produces one output per input element |
| What does `flatMap()` do? | Produces zero or more outputs per input and flattens them |
| What does `filter()` do? | Keeps elements satisfying a condition |
| What does `reduceByKey()` do? | Aggregates values by key |
| What does `groupByKey()` do? | Groups all values for each key |
| What does `collect()` do? | Returns all elements to the driver |
| What does `count()` return? | The number of elements |
| What does `take(n)` do? | Returns up to `n` elements |
| What is a shuffle? | Redistribution of data across partitions |
| Which transformations commonly shuffle? | `distinct()`, `reduceByKey()`, `groupByKey()`, `join()`, `sortByKey()` |
| Which API is generally preferred for structured data? | DataFrame API |
| What does `printSchema()` do? | Displays DataFrame column types and structure |
| What does `selectExpr()` accept? | SQL-like expression strings |
| What does `groupBy()` do? | Groups rows by selected columns |
| What is a DStream? | A sequence of RDDs representing a stream |
| What is the modern streaming API? | Structured Streaming |
| What does horizontal scaling mean? | Adding machines to a cluster |
| Why cache a dataset? | To reuse computed data and potentially avoid recomputation |

---

## 4\. Common Exam Traps

1. **Transformations do not normally execute immediately.** They are lazy and create new RDDs or DataFrames.
2. **Actions trigger computation.** `collect()`, `count()`, `first()`, and `saveAsTextFile()` are examples.
3. **Actions do not create a new RDD.** They return a result, perform an operation, or write data.
4. **RDDs are immutable.** A transformation creates a new RDD instead of changing the original.
5. **`collect()`****can be dangerous.** It moves all elements to the driver and may exhaust driver memory.
6. **`reduceByKey()`****is not interchangeable with****`groupByKey()`****.** One aggregates; the other groups values.
7. **Wide transformations commonly require shuffles.** Shuffles increase communication and execution costs.
8. **Spark is not always faster than Hadoop MapReduce.** The advantage depends on the workload and configuration.
9. **The number of worker nodes is not necessarily the number of partitions.** Partitions are units of distributed work.
10. **DataFrames are not simply local Pandas DataFrames.** Spark DataFrames are distributed and processed across executors.
11. **DStreams and Structured Streaming are different APIs.** DStreams are RDD-based; Structured Streaming uses the structured-data model.
12. **`takeOrdered(3)`****does not automatically mean the three most frequent items.** The ordering must be defined appropriately.

---

## 5\. Recommended Exam Study Priority

### Priority 1 — Master these

- Transformation versus action.
- Lazy evaluation and execution.
- Narrow versus wide transformations.
- Shuffles and their performance impact.
- `reduceByKey()` versus `groupByKey()`.
- `collect()` and driver memory risks.
- RDD properties and fault tolerance.

### Priority 2 — Practise coding

- Create RDDs with `textFile()` and `parallelize()`.
- Use `map()`, `flatMap()`, `filter()`, and `reduceByKey()`.
- Count characters in a text file.
- Count word frequencies.
- Find the three most frequent words.
- Create and inspect DataFrames.
- Use `select()`, `selectExpr()`, `where()`, and `orderBy()`.
- Perform grouped aggregations with `groupBy()` and `agg()`.

### Priority 3 — Understand architecture and streaming

- Driver, SparkContext/SparkSession, executors, and cluster manager.
- Spark ecosystem: Spark Core, Spark SQL, MLlib, GraphX, and streaming.
- DStreams versus Structured Streaming.
- Horizontal scalability and Spark's advantages over traditional disk-oriented processing.

---

## 6\. Final Memory Sheet

Remember these relationships:

- **RDD:** immutable + distributed + parallelizable + fault recovery through lineage.
- **Transformation:** new dataset + lazy evaluation.
- **Action:** triggers computation + returns results or writes output.
- **Narrow transformation:** partition-local dependency + generally no shuffle.
- **Wide transformation:** cross-partition dependency + commonly a shuffle.
- **`reduceByKey()`****:** aggregate by key; usually preferable for reductions.
- **`groupByKey()`****:** collect values by key; avoid when aggregation is the real objective.
- **`collect()`****:** retrieve everything to the driver; use cautiously.
- **DataFrame:** structured distributed data + SQL-like operations + Catalyst optimization.
- **Spark Streaming:** legacy micro-batch API using DStreams and RDDs.
- **Structured Streaming:** modern structured-data streaming API.

---

## 7\. Source and Scope Notes

This document is based on the supplied T6 Spark lecture notes.

- Section 1 reproduces the four questions explicitly listed under the lecturer's conceptual review section, with expanded explanations.
- Section 2 adds questions inferred from the lecture's objectives, examples, exercises, and key takeaways. These are useful study questions, but they should not be mistaken for questions explicitly written by the lecturer.
- Section 3 condenses the main definitions and distinctions for rapid revision.
- Sections 4–6 focus on common mistakes, practical coding, and last-minute recall.

The review questions and explanations are intended to support preparation for a later practice examination based on the complete set of lecture notes from all five chapters.

**Suggested filename:** `t6-spark-review-and-answers.md`

When you finish the remaining four chapters, submit their compression materials along with this review document. I can then generate a practice exam grounded in the actual lecture notes, giving extra weight to the lecturer's explicit review questions while also testing the code patterns, concepts, and exercises emphasized throughout the lectures.
