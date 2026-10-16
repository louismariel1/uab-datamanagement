# T5 — Apache Spark: Exam Compression

1. Exam compression: Condense the supplied lecture notes into definitions, key distinctions, essential code patterns, examples, likely exam questions, and common mistakes.
2. Save to GitHub: Keep each chapter's compression as a separate Markdown file, so you can revise them independently.
3. Repeat for all five chapters: Use the actual lecture material for each chapter rather than relying on generic textbook coverage.
4. Generate a practice exam: Once you provide all five compressions—and ideally retain the original lecture notes too—I can create a practice exam covering the five chapters, with a mix of conceptual questions, code interpretation, practical exercises, and an answer key.

Part 1 of 2 · Foundations, architecture, RDDs, PySpark, and DataFrames

# UAB 106566 — Data Management

## Exam Compression T6: Apache Spark

**Academic year:** 2026/2027\
**Degree:** Bachelor's Degree in Artificial Intelligence\
**University:** Universitat Autònoma de Barcelona

---

## 0\. How to revise this chapter

The lecture's principal learning objectives are:

- Explain why large-scale datasets require distributed processing.
- Describe Spark's architecture and execution model.
- Understand RDDs, transformations, actions, and lazy evaluation.
- Develop basic PySpark applications using RDDs and DataFrames.
- Perform DataFrame filtering, sorting, selection, and aggregation.
- Understand Spark Streaming and its evolution toward Structured Streaming.

**Revision priority:** RDDs and transformations/actions → word-count exercises → narrow versus wide transformations and shuffles → DataFrame operations → performance guidelines → Spark Streaming.

# 1\. Recap: NoSQL databases

This is introductory revision material; the main chapter focuses on Spark.

- **NoSQL:** A family of database systems that offers different data models and trade-offs in flexibility, scalability, performance, and availability.
- **Document database:** Stores records as documents. Example: MongoDB.
- **Key-value database:** Stores values indexed by keys. Example: Redis, an in-memory data store with additional data structures.
- **Column-family database:** Organises data into column families; useful in certain distributed workloads.
- **Graph database:** Represents entities and their relationships as nodes and edges.

### ACID, BASE, and CAP

- **ACID:** Atomicity, Consistency, Isolation, Durability. Properties associated with reliable transactions.
- **BASE:** Basically Available, Soft state, Eventual consistency. A conceptual approach associated with some distributed systems.
- **CAP theorem:** During a network partition, a distributed system cannot guarantee both strict consistency and availability simultaneously.

**Exam takeaway:** No database is universally best. Choose a database according to the application's data model, workload, consistency requirements, availability needs, and scalability requirements.

# 2\. Why distributed processing is necessary

## 2.1 The problem with a single machine

Pandas is a Python library providing convenient data structures, especially DataFrames, for data analysis.

- **Strengths:** Intuitive syntax, powerful manipulation tools, convenient local analysis.
- **Limitation:** Standard Pandas processing is performed on a single machine and is limited by that machine's resources.
- **Vertical scaling:** Increase the resources of one machine, such as its CPU or RAM.
- **Horizontal scaling:** Distribute work across multiple machines.

When datasets reach terabyte scale, one machine may lack sufficient memory, storage capacity, or processing power.

**Apache Spark** distributes data into partitions and executes tasks across a cluster, enabling processing beyond the capacity of an individual machine.

| Pandas on one machine | Apache Spark on a cluster |
| --- | --- |
| Primarily single-machine processing | Distributed processing |
| Limited by one machine's resources | Can scale using additional cluster resources |
| Convenient for local analysis | Suitable for large-scale workloads |
| No inter-worker shuffle | Some operations require network shuffles |

A shuffle is not required by every Spark operation; it occurs when data must be redistributed between partitions.

## 2.2 Spark versus Hadoop MapReduce

The lecture contrasts traditional Hadoop MapReduce with Spark:

- **Traditional Hadoop MapReduce:** Commonly writes intermediate results to disk between processing stages.
- **Spark:** Can keep and cache intermediate datasets in memory and reuse them.

Memory access is generally much faster than disk access. Spark can therefore perform very well for iterative algorithms and repeated computations.

The lecture mentions Spark being up to 100 times faster. Treat this as a benchmark claim under particular conditions, **not a universal guarantee**. Performance depends on the workload, configuration, memory, storage, and amount of data transferred. Spark can also spill data to disk.

## 2.3 Benefits of Apache Spark

1. **Speed:** In-memory computation and reduced disk I/O where applicable.
2. **Scalability:** Distribute processing across multiple nodes.
3. **Caching:** Reuse intermediate results instead of recomputing them.
4. **High-level APIs:** Support programming in Python, Scala, Java, and other supported interfaces.
5. **Broad ecosystem:** SQL, machine learning, graph processing, and streaming.
6. **Fault tolerance:** Recover lost partitions using lineage when the required source data and dependencies remain available.

## 2.4 Spark ecosystem

| Component | Main purpose |
| --- | --- |
| Spark Core | Execution engine and fundamental distributed-processing functionality |
| Spark SQL | Structured data processing, SQL, and DataFrames |
| Spark Streaming | Classic stream processing using DStreams |
| MLlib | Machine-learning functionality |
| GraphX | Graph-processing framework, particularly associated with Scala |

**Likely exam question:** Which Spark component would you use for SQL queries, machine learning, or graph processing? Explain your choices.

# 3\. Spark architecture and execution

## 3.1 Master–worker model

The lecture introduces Spark using a master–worker model:

- The master-side application coordinates work.
- Workers execute distributed computation.
- Data is divided into partitions that can be processed in parallel.

For technical accuracy, distinguish the following concepts:

| Term | Role |
| --- | --- |
| Driver | Runs the application, constructs execution plans, schedules tasks, and coordinates execution |
| SparkContext | Classic entry point to Spark Core and RDD functionality |
| SparkSession | Modern entry point commonly used for DataFrames and Spark SQL; provides access to the underlying SparkContext |
| Cluster manager | Allocates cluster resources; examples include Spark Standalone, YARN, and Kubernetes |
| Worker | Machine that provides resources for distributed computation |
| Executor | Process on a worker that executes tasks and may cache or store data |

The driver coordinates the application; it does not perform all distributed processing itself.

## 3.2 Lifecycle of a Spark application

1. Create a Spark application and its entry point.
2. Read input data or create an RDD/DataFrame.
3. Define transformations.
4. Spark records the computation dependencies in a plan or DAG.
5. Call an action.
6. Spark schedules the necessary stages and tasks across partitions and executors.
7. Results are returned to the driver or written to external storage.

**Central principle:** Transformations are normally lazy. An action triggers the computation required to produce the requested result.

## 3.3 DAG and execution

A **DAG (Directed Acyclic Graph)** represents computation dependencies without directed cycles.

For example:

`Input RDD → filter() → map() → collect()`

The `filter()` and `map()` transformations define a computation. `collect()` requests the result and triggers the execution needed to produce it.

Spark can divide execution into stages. Shuffle dependencies commonly separate stages, while tasks within a stage process partitions.

# 4\. RDDs: Resilient Distributed Datasets

## 4.1 Definition

An **RDD (Resilient Distributed Dataset)** is an immutable, distributed collection of elements that Spark can process in parallel.

Its main properties are:

- **Resilient:** Lost partitions can often be recomputed using lineage.
- **Distributed:** Elements are organised into partitions across the cluster.
- **Dataset:** Elements can be numbers, strings, tuples, or other objects.
- **Immutable:** A transformation creates a new RDD; it does not modify the original.
- **Lazy:** Transformations normally define computations without executing them immediately.
- **Parallel:** Different partitions can be processed concurrently.

Example:

```
numbers = sc.parallelize([1, 2, 3, 4, 5])

doubled = numbers.map(lambda x: x * 2)
```

`numbers` remains unchanged. `doubled` represents a new RDD containing the transformed values when evaluated.

## 4.2 Creating RDDs in PySpark

The lecture identifies two fundamental methods.

### Method A: Read a text file

```
lines = sc.textFile("data/book.txt")
```

- `sc` is a SparkContext.
- Each element normally represents one line of the file.
- Input may come from a supported local or distributed filesystem.

### Method B: Parallelise a Python collection

```
numbers = sc.parallelize([1, 2, 3, 4, 5], 4)
```

The second argument requests four partitions. **Four partitions do not necessarily mean four physical worker machines.**

A typical modern setup:

```
from pyspark.sql import SparkSession

spark = (
    SparkSession.builder
    .appName("DataManagement")
    .getOrCreate()
)

sc = spark.sparkContext
```

The lecture introduces SparkContext as the main entry point. SparkSession is commonly used in modern applications.

# 5\. Transformations versus actions

This is one of the most important exam topics.

## 5.1 Transformations

A transformation defines a new RDD from an existing RDD.

- Returns a new RDD.
- Normally executes lazily.
- Adds to the computation's dependency graph.
- Does not modify the original RDD.

Examples: `map()`, `filter()`, `flatMap()`, `union()`, `distinct()`, `reduceByKey()`.

## 5.2 Actions

An action requests a result or writes data to external storage.

- Triggers the computation required for that result.
- May return a value to the driver or write distributed output.
- Does not create a new RDD as its result.

Examples: `count()`, `first()`, `take()`, `collect()`, `reduce()`, `foreach()`, `saveAsTextFile()`.

| Transformation | Action |
| --- | --- |
| Defines a new computation | Requests its execution/result |
| Returns a new RDD | Returns a result or writes output |
| Normally lazy | Triggers required execution |
| Example: `map()` | Example: `count()` |

### Example

```
numbers = sc.parallelize([1, 2, 3, 4, 5])

squares = numbers.map(lambda x: x * x)

print(squares.collect())
# [1, 4, 9, 16, 25]
```

`map()` defines the transformation. `collect()` triggers execution and retrieves the elements.

**Exam trap:** `map()` does not immediately compute the entire result, and it does not alter `numbers`.

## 5.3 Essential RDD transformations

### `map()` — one output per input

```
rdd = sc.parallelize([1, 2, 3, 4, 5])

result = rdd.map(lambda x: x * 3)

print(result.collect())
# [3, 6, 9, 12, 15]
```

Each input element produces one output element.

### `filter()` — retain elements satisfying a condition

```
rdd = sc.parallelize([1, 2, 3, 4, 5])

result = rdd.filter(lambda x: x % 2 == 0)

print(result.collect())
# [2, 4]
```

### `flatMap()` — zero, one, or multiple outputs per input

```
rdd = sc.parallelize([1, 2, 3])

result = rdd.flatMap(lambda x: [x, x * 10])

print(result.collect())
# [1, 10, 2, 20, 3, 30]
```

Unlike `map()`, `flatMap()` flattens the output collections into one RDD.

Typical use: convert lines of text into individual words.

### `union()` — combine two RDDs

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

Other transformations covered in the lecture include `sample()`, `reduceByKey()`, `groupByKey()`, `join()`, and `sortByKey()`.

## 5.4 Narrow versus wide transformations

### Narrow transformation

An output partition depends on a limited set of input partitions, usually allowing processing without redistributing data across the cluster.

Examples typically classified as narrow:

- `map()`
- `flatMap()`
- `filter()`
- `sample()`
- `union()` in the usual RDD dependency model

### Wide transformation

An output partition may depend on data from multiple input partitions. Spark generally needs a **shuffle** to redistribute data.

Examples commonly classified as wide:

- `distinct()`
- `reduceByKey()`
- `groupByKey()`
- `join()`
- `sortByKey()`

| Operation | Typical classification | Reason |
| --- | --- | --- |
| `map()` | Narrow | Transform each element locally |
| `flatMap()` | Narrow | Expand elements locally |
| `filter()` | Narrow | Retain or discard elements locally |
| `sample()` | Narrow | Sampling can be performed within partitions |
| `union()` | Usually narrow | Combine existing partition sets |
| `distinct()` | Wide | Identify duplicates across partitions |
| `reduceByKey()` | Wide overall | Aggregate values for matching keys |
| `groupByKey()` | Wide | Gather values by key |
| `join()` | Often wide | Matching keys may require redistribution |
| `sortByKey()` | Wide overall | Global ordering generally requires redistribution |

Some physical execution strategies can avoid particular shuffles; the table describes typical behaviour.

## 5.5 Shuffles and why they matter

A **shuffle** redistributes data between partitions, often across the network.

It can involve:

- Network communication.
- Serialization and deserialization.
- Memory usage and disk I/O.
- Additional execution stages.

Shuffles are often expensive, so minimising unnecessary shuffling is an important performance objective.

## 5.6 Essential RDD actions

| Action | Purpose | Example |
| --- | --- | --- |
| `count()` | Count elements | `rdd.count()` |
| `first()` | Return the first element | `rdd.first()` |
| `take(n)` | Return up to the first `n` elements | `rdd.take(3)` |
| `takeOrdered(n)` | Return the `n` smallest elements by default ordering | `rdd.takeOrdered(3)` |
| `collect()` | Retrieve every element at the driver | `rdd.collect()` |
| `reduce()` | Combine elements into one result | `rdd.reduce(lambda a, b: a + b)` |
| `foreach()` | Execute a function for every element | `rdd.foreach(func)` |
| `saveAsTextFile()` | Write distributed text output | `rdd.saveAsTextFile("output")` |

Important details:

- `takeOrdered(3)` returns the three smallest elements by default, unless a custom ordering is supplied.
- `collect()` can exhaust driver memory for large datasets.
- `saveAsTextFile()` writes distributed output, typically as multiple part files.
- `foreach()` runs work on executors; it does not construct a normal Python list at the driver.
- Actions trigger the computations needed for their requested result, not necessarily every possible computation in an application.

## 5.7 Lazy evaluation and lineage

Spark records transformation dependencies and uses them to build execution plans.

```
rdd = sc.parallelize([1, 2, 3, 4, 5])

filtered = rdd.filter(lambda x: x > 2)
squared = filtered.map(lambda x: x * x)

print(squared.collect())
# [9, 16, 25]
```

Before `collect()`, the transformations define the computation. The action triggers the necessary execution.

**Lineage and fault tolerance:** If a partition is lost, Spark can often recompute it from the source data and recorded dependencies rather than storing a complete backup of every intermediate RDD.

# 6\. Core PySpark exercises

The lecture explicitly includes three exercises based on _El Quijote_. Learn the solution patterns and be able to explain each step.

## 6.1 Exercise 1 — Count characters

**Task:** Count the characters in the book, assuming four partitions.

```
lines = sc.textFile("data/quijote.txt", 4)

character_counts = lines.map(lambda line: len(line))

total_characters = character_counts.reduce(
    lambda a, b: a + b
)

print(total_characters)
```

A concise alternative:

```
total_characters = lines.map(len).sum()
```

Explanation:

1. `textFile()` creates an RDD of lines.
2. `map(len)` computes the character count of each line.
3. `reduce()` or `.sum()` combines the counts.
4. The final result is a single total.

**Important assumption:** `len(line)` counts characters in the line string returned by `textFile()`. Line terminators are generally not included. If the task counts newline separators too, account for them explicitly.

## 6.2 Exercise 2 — Count occurrences of each word

This is the classic word-count pipeline.

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

1. **Input:** One element per line.
2. **`flatMap(split)`****:** Convert lines into individual tokens.
3. **`map(word → (word, 1))`****:** Give every occurrence a count of one.
4. **`reduceByKey(+)`****:** Add counts belonging to the same word.

The lecture includes example frequencies such as `el → 8170` and `Mancha → 44`. These are examples from the lecture, not guaranteed results for differently processed text.

**Tokenisation caveat:** `split()` separates on whitespace. Consequently, punctuation and capitalisation are retained: `Mancha`, `Mancha,`, and `mancha` can be counted separately.

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

This changes the counting rules, so the results may differ from the lecture's example.

## 6.3 Exercise 3 — Find the three most frequent words

Given the `word_counts` RDD from Exercise 2:

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)

print(top_three)
```

Explanation:

- `pair[0]` is the word.
- `pair[1]` is its count.
- Negating the count makes higher frequencies sort first under ascending ordering.
- `pair[0]` provides alphabetical tie-breaking.

Illustrative output structure:

```
[
    ("word_a", 12000),
    ("word_b", 9500),
    ("word_c", 8100)
]
```

These values are illustrative, not actual frequencies from _El Quijote_.

**Memorise the three patterns:**

- Character count: `map(len)` → `reduce()` or `.sum()`.
- Word count: `flatMap(split)` → `map((word, 1))` → `reduceByKey(+)`.
- Most frequent words: word-count RDD → `takeOrdered()` with a custom key.

# 7\. PySpark DataFrames and the structured API

## 7.1 Why use a higher-level API?

RDDs are flexible low-level collections, but complex structured operations can require considerable manual code.

Spark provides higher-level structured APIs, particularly DataFrames, for working with rows, columns, schemas, filtering, sorting, joins, and aggregations.

A **DataFrame** is a distributed table consisting of rows and named columns with associated data types.

- Data is divided into partitions for parallel processing.
- DataFrames provide a declarative, SQL-like interface.
- Spark can optimise structured operations using Spark SQL's query-planning and execution machinery.

The lecture introduces Datasets and DataFrames as structured abstractions. In PySpark, the DataFrame API is the principal structured interface; Scala and Java also support typed Datasets.

## 7.2 RDD versus DataFrame

| Feature | RDD | DataFrame |
| --- | --- | --- |
| Abstraction | Distributed collection | Structured table |
| Schema | No required tabular schema | Named columns and schema |
| Style | Functional, element-level processing | Declarative, SQL-like operations |
| Optimisation | Less automatic relational optimisation | Spark SQL optimisation |
| Development | More low-level control | Usually simpler for structured analytics |
| Typical use | Custom processing | Filtering, joins, aggregation, analytics |

**Default rule:** Prefer DataFrames for structured data. Use RDDs when low-level control or custom element-level processing is needed.

A DataFrame should not be treated as merely an RDD with a different name: structured operations are planned and executed through Spark SQL's optimisation framework.

## 7.3 Read JSON and inspect a schema

```
df = spark.read.format("json").load(
    "/data/genomics/json/summary.json"
)

df.printSchema()
df.show()
```

- `spark.read.format("json")` selects the JSON data source.
- `load()` reads the input into a DataFrame.
- `printSchema()` displays column names, types, and nested structure.
- `show()` displays a limited number of rows by default.

# 8\. DataFrame selection and expressions

The lecture uses a sample DataFrame `c` with columns including `customer`, `product`, `quantity`, and `price`.

## 8.1 `select()` — choose columns

```
c.select("customer").show()

c.select("customer", "product").show()
```

`select()` returns a new DataFrame containing the requested columns or expressions.

## 8.2 `col()` and calculated columns

`col()` creates a reference to a DataFrame column, allowing it to be used in expressions.

```
from pyspark.sql.functions import col

c.select(
    "customer",
    "product",
    (col("quantity") * col("price")).alias("total")
).show()
```

The expression calculates quantity multiplied by price for every row. `alias("total")` names the resulting column.

For the four rows shown in the lecture:

| Customer | Product | Quantity | Price | Total |
| --- | --- | --- | --- | --- |
| 200 | 5 | 8 | 33 | 264 |
| 200 | 8 | 5 | 111 | 555 |
| 100 | 8 | 5 | 45 | 225 |
| 100 | 8 | 5 | 59 | 295 |

The total is calculated as `quantity × price`.

## 8.3 `selectExpr()` — SQL expressions

`selectExpr()` allows column selection and calculations using SQL expression strings.

```
c.selectExpr(
    "customer",
    "product",
    "quantity * price AS total"
).show()
```

To retain all original columns and add a calculated column:

```
c.selectExpr(
    "*",
    "quantity * price AS total"
).show()
```

Remember:

- `select()` accepts column names and Spark column expressions.
- `selectExpr()` accepts SQL expression strings.
- Both return a new DataFrame and leave the original unchanged.
- `"*"` means all original columns in a SQL expression.

# 9\. DataFrame filtering and sorting

## 9.1 Filtering with `where()` or `filter()`

`where()` returns a new DataFrame containing only rows that satisfy the specified condition.

```
c.where(col("quantity") > 3).show()
```

Equivalent:

```
c.filter(col("quantity") > 3).show()
```

Combine conditions using Spark column operators:

```
c.where(
    (col("quantity") > 0)
    & (col("price") > 0)
).show()
```

- `&` means logical AND.
- `|` means logical OR.
- Parenthesise individual comparisons.
- Do not use Python's `and` or `or` to combine Spark column expressions.

The lecture also introduces `expr()` for constructing expressions from SQL-like strings.

## 9.2 Sorting with `orderBy()`

Ascending order is the default.

```
c.orderBy("price").show()
```

Descending order:

```
c.orderBy(col("price").desc()).show()
```

Multiple sort keys:

```
c.orderBy(
    col("customer").asc(),
    col("price").desc()
).show()
```

A distributed global sort may require a shuffle and can be expensive.

# 10\. DataFrame aggregation with `groupBy()` and `agg()`

`groupBy()` specifies which rows belong to each group. `agg()` specifies the aggregate calculations performed for each group.

## 10.1 Total quantity per customer

```
from pyspark.sql.functions import sum

c.groupBy("customer").agg(
    sum("quantity").alias("total_quantity")
).show()
```

## 10.2 Total sales value per customer

```
c.groupBy("customer").agg(
    sum(col("quantity") * col("price")).alias("total_sales")
).show()
```

Other aggregate functions include:

- `sum()` — sum values.
- `avg()` — calculate an average.
- `max()` — find the maximum.
- `min()` — find the minimum.
- `count()` — count rows or non-null values, depending on the expression.

**Exam distinction:** `groupBy()` defines the grouping columns; `agg()` defines the calculations.

## 10.3 DataFrame patterns to memorise

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

**End of Part 1.** Part 2 completes this document with Spark performance, Spark Streaming, the final memory sheet, exam-style questions, and a self-test checklist.

# T6 — Apache Spark: Exam Compression

Part 2 of 2 · Performance, streaming, memory sheet, and exam preparation

Append the following directly after Part 1 in `T6-Spark.md`.

Writing

# 11\. Spark performance guidelines

The lecture explicitly identifies five performance guidelines. Learn each rule, the reason behind it, and an example.

## 11.1 Prefer `reduceByKey()` over `groupByKey()` for aggregation

Consider this RDD:

```
rdd = sc.parallelize([
    ("A", 1),
    ("A", 1),
    ("B", 1),
    ("A", 1)
])
```

Using `reduceByKey()`:

```
counts = rdd.reduceByKey(lambda a, b: a + b)

print(sorted(counts.collect()))
# [('A', 3), ('B', 1)]
```

`reduceByKey()` aggregates values associated with the same key. Spark can combine values locally before the shuffle, reducing the amount of data transferred.

By contrast:

```
groups = rdd.groupByKey()
```

`groupByKey()` gathers all values for each key into an iterable. This can require more network transfer and memory when many values share a key.

**Rule:** Use `reduceByKey()` for sums, counts, and similar aggregations. Use `groupByKey()` when the task genuinely requires all values associated with each key.

## 11.2 Minimise shuffles

Shuffles redistribute data between partitions and can involve network transfer, serialization, disk I/O, and memory overhead.

Common shuffle-intensive operations include:

- `distinct()`
- `groupByKey()`
- `reduceByKey()`
- Global sorting
- Many joins

Strategies for reducing overhead:

- Filter unnecessary records early.
- Avoid redundant wide transformations.
- Prefer appropriate aggregation operations.
- Choose suitable partitioning and join strategies.
- Avoid moving large datasets to the driver unnecessarily.

Not every transformation causes a shuffle. `map()` and `filter()`, for example, are normally narrow transformations.

## 11.3 Cache datasets that will be reused

Caching can avoid repeated computation when a costly intermediate dataset is used by multiple actions.

```
filtered = lines.filter(
    lambda line: "Spark" in line
).cache()

print(filtered.count())
print(filtered.take(10))
```

The first action computes the required data. Caching allows computed partitions to be reused where available.

Caching is useful when the cost of recomputation outweighs the storage overhead.

However:

- Caching consumes memory and possibly disk.
- Not every dataset needs caching.
- Cached partitions may need to be recomputed if lost.
- Caching does not mean that every partition is immediately computed when `.cache()` is called.

## 11.4 Avoid `collect()` on large datasets

```
result = rdd.collect()
```

`collect()` transfers every RDD element to the driver. A large result can exhaust driver memory.

Prefer an operation suited to the task:

```
rdd.take(10)
```

Use `take()` for a small sample, `reduce()` for a single aggregate result, or distributed output methods when the entire result must be stored.

**Exam trap:** `collect()` is an action, not a transformation, and it is not a safe default for inspecting large datasets.

## 11.5 Choose an appropriate number of partitions

Partitions are units of distributed processing.

- **Too few partitions:** May limit parallelism and produce oversized tasks.
- **Too many tiny partitions:** Can increase scheduling overhead and create many small output files.
- **Appropriate partitioning:** Depends on data size, available resources, task costs, and the operation being performed.

The lecture's exercises assume four worker nodes. This does not mean every RDD must have four partitions. A worker can process multiple partitions, and the number of partitions can differ from the number of physical machines.

## 11.6 Performance rules — one-line revision

| Rule | Reason |
| --- | --- |
| Prefer `reduceByKey()` for aggregation | Local combining can reduce shuffle traffic |
| Minimise shuffles | Reduce communication and intermediate-data costs |
| Cache reused datasets | Avoid repeated computation |
| Avoid large `collect()` operations | Prevent driver memory exhaustion |
| Choose suitable partition counts | Balance parallelism and scheduling overhead |

# 12\. Spark Streaming fundamentals

## 12.1 What is Spark Streaming?

Spark Streaming enables processing of incoming data streams, such as social media messages, click events, and application logs.

The lecture emphasises:

- Continuous analysis of incoming events.
- An API familiar to Spark developers.
- Integration with the wider Spark and big-data ecosystem.
- Processing streams in small time-based batches.

The classic Spark Streaming API uses **micro-batching**: incoming data is divided into batches, and each batch is processed using Spark operations.

This is not the same as guaranteeing that every event is processed immediately upon arrival.

## 12.2 DStreams

A **DStream (Discretized Stream)** is the main abstraction of the original Spark Streaming API.

A DStream represents a time-ordered sequence of RDDs. Each RDD contains the data received during a particular batch interval.

Conceptually:

```
Continuous incoming data
          |
          v
   Batch interval 1  ---> RDD 1
   Batch interval 2  ---> RDD 2
   Batch interval 3  ---> RDD 3
          |
          v
Apply Spark operations to each batch
          |
          v
       Results
```

The diagram is conceptual. The batch intervals shown do not imply a particular production configuration.

Key facts:

- A DStream represents a continuous stream as a sequence of RDDs.
- Each batch interval produces an RDD containing the data for that interval.
- Each RDD can be processed using Spark-style transformations and actions.
- Spark Streaming coordinates the creation and processing of the RDDs.

## 12.3 StreamingContext and batch interval

In the classic DStream API, `StreamingContext` is the entry point for streaming functionality.

It requires a SparkContext and a batch interval.

Conceptual setup:

1. Create a SparkContext.
2. Create a StreamingContext with the required interval.
3. Define the input stream.
4. Apply transformations and output operations.
5. Start the streaming computation.
6. Keep the application running as needed.

The batch interval must suit the application's latency requirements and available resources.

- A shorter interval can reduce the wait before a batch is processed.
- Very short intervals can increase scheduling overhead.
- If processing cannot keep up with incoming data, the application may fall behind.

## 12.4 Example: count words received from a TCP socket

**Lecture task:** Count words received from a server listening on port `9999`.

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
4. `flatMap()` extracts individual words from incoming lines.
5. `map()` converts each word into a `(word, 1)` pair.
6. `reduceByKey()` sums word counts within each batch.
7. `pprint()` displays the results.
8. `start()` starts streaming execution.
9. `awaitTermination()` keeps the application waiting for termination.

This is the classic DStream API demonstrated by the lecture. The socket server must be running and accessible, and the environment must support the relevant streaming API.

For production systems, consider source reliability, recovery, checkpointing, and processing guarantees.

## 12.5 DStreams versus Structured Streaming

The lecture identifies Structured Streaming as the recommended modern approach for new structured streaming applications.

| Feature | Classic Spark Streaming | Structured Streaming |
| --- | --- | --- |
| Fundamental abstraction | DStreams, a sequence of RDDs | Structured DataFrames/Datasets |
| Programming style | RDD-oriented | SQL-like and DataFrame-oriented |
| Query optimisation | Less structured-query optimisation | Uses Spark SQL's optimisation framework |
| Batch integration | Separate classic streaming abstraction | Similar programming model to batch DataFrames |
| Status in the lecture | Legacy API | Recommended modern API |

Structured Streaming supports incremental processing and commonly uses micro-batch execution. Continuous processing is available only for supported configurations and operations; it is not a universal guarantee of every query.

**Exam takeaway:** Explain the DStream model because it is taught explicitly, and distinguish it from Structured Streaming.

# 13\. NoSQL recap — minimum exam knowledge

Although Spark is the main focus, the opening slides recap NoSQL concepts.

Memorise:

- NoSQL systems provide different trade-offs in flexibility, scalability, performance, and availability.
- The main models introduced are document, key-value, column-family, and graph.
- MongoDB is an example of a document database.
- Redis is an in-memory key-value data store with additional data structures and caching capabilities.
- ACID refers to atomicity, consistency, isolation, and durability.
- BASE refers to basically available, soft state, and eventual consistency.
- CAP concerns the trade-off between consistency and availability during a network partition.

Do not spend more revision time on this recap than on the chapter's core Spark objectives unless your lecturer indicates otherwise.

# 14\. Closed-book memory sheet

This section is designed for quick revision immediately before the exam.

## 14.1 Essential definitions

| Term | Definition to reproduce |
| --- | --- |
| Apache Spark | Distributed processing engine for large-scale data |
| RDD | Immutable, partitioned collection that Spark processes in parallel |
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
| Executor | Process that runs tasks and may cache intermediate data |
| Partition | Unit of distributed data processing |
| DStream | Time-ordered sequence of RDDs in classic Spark Streaming |
| StreamingContext | Entry point for the classic DStream streaming API |
| `reduceByKey()` | Aggregates values with matching keys |
| `collect()` | Retrieves all RDD elements at the driver |

## 14.2 Essential RDD code

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

# Top three word frequencies
top_three = counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

## 14.3 Essential DataFrame code

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

# Calculate a new column
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

## 14.4 Five performance rules

1. Prefer `reduceByKey()` over `groupByKey()` when aggregating values.
2. Minimise unnecessary shuffles.
3. Cache expensive datasets that are reused.
4. Avoid `collect()` on large datasets.
5. Choose an appropriate number of partitions.

## 14.5 Five distinctions you must know

1. Transformation versus action.
2. Narrow versus wide transformation.
3. RDD versus DataFrame.
4. Driver versus executor.
5. DStream versus Structured Streaming.

# 15\. Exam-style questions and model answers

These are practice questions derived from the supplied lecture. They are not predictions of the actual examination.

## Question 1 — Why is distributed processing needed?

**Model answer:** A single machine has finite CPU, memory, and storage resources. Spark distributes data into partitions and executes tasks across multiple machines, enabling horizontal scaling and processing of datasets too large for one machine.

## Question 2 — Explain the roles of the driver and executors.

**Model answer:** The driver runs the application, builds execution plans, schedules tasks, and coordinates execution. Executors run tasks on worker resources and can cache intermediate data. The driver coordinates distributed computation rather than performing all data processing itself.

## Question 3 — What is an RDD?

**Model answer:** An RDD is an immutable, distributed collection of elements processed in parallel. It supports transformations and actions, and its lineage can help Spark recompute lost partitions.

## Question 4 — Explain lazy evaluation.

**Model answer:** Transformations normally record computations without executing them immediately. When an action requests a result, Spark executes the computations needed to produce it.

## Question 5 — Distinguish transformations from actions.

**Model answer:** Transformations define new RDDs or computations, such as `map()` and `filter()`. Actions request results or output, such as `count()`, `collect()`, and `saveAsTextFile()`. Actions trigger the required execution.

## Question 6 — What is the difference between narrow and wide transformations?

**Model answer:** Narrow transformations normally process data locally within a limited dependency between partitions. Wide transformations depend on data from multiple partitions and typically require a shuffle. Shuffles introduce communication and processing overhead.

## Question 7 — Why is `reduceByKey()` preferred to `groupByKey()` for counting words?

**Model answer:** `reduceByKey()` can combine values locally before the shuffle, reducing data transfer. `groupByKey()` collects all values for each key and is less efficient when only an aggregate is needed.

## Question 8 — What happens when `collect()` is called?

**Model answer:** Spark computes the required RDD result and transfers all its elements to the driver as a local collection. A sufficiently large result can exhaust driver memory.

## Question 9 — Write a word-count solution.

**Model answer:**

```
word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

`flatMap()` splits lines into tokens, `map()` assigns each token a count of one, and `reduceByKey()` adds counts for matching words.

## Question 10 — How would you find the three most frequent words?

**Model answer:**

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

The negative count puts larger frequencies first, and the word provides alphabetical tie-breaking.

## Question 11 — Why prefer DataFrames for structured data?

**Model answer:** DataFrames provide named columns, schemas, and declarative operations. Spark SQL can optimise structured operations, making filtering, sorting, joining, and aggregation easier to express and often more efficient.

## Question 12 — How do `select()` and `selectExpr()` differ?

**Model answer:** `select()` accepts column names and Spark column expressions. `selectExpr()` accepts SQL expression strings. Both return new DataFrames and can select or calculate columns.

## Question 13 — How do `groupBy()` and `agg()` work together?

**Model answer:** `groupBy()` defines the grouping columns, while `agg()` defines aggregate functions such as `sum()`, `avg()`, `min()`, `max()`, and `count()`.

## Question 14 — What is a DStream?

**Model answer:** A DStream is the abstraction used by classic Spark Streaming to represent a stream as a time-ordered sequence of RDDs. Each RDD represents the data received during a batch interval.

## Question 15 — How does Structured Streaming differ from DStreams?

**Model answer:** Classic Spark Streaming uses DStreams and RDDs. Structured Streaming uses structured DataFrames/Datasets and the Spark SQL execution framework, providing a more declarative interface and query optimisation.

# 16\. Common exam mistakes

Check that you can avoid these errors:

- Saying that transformations execute immediately.
- Saying that transformations modify an existing RDD.
- Confusing the number of partitions with the number of worker machines.
- Claiming that every transformation causes a shuffle.
- Using `groupByKey()` for a simple sum when `reduceByKey()` is more appropriate.
- Calling `collect()` on a large distributed dataset without considering driver memory.
- Confusing the driver with an executor.
- Assuming `union()` removes duplicates or sorts data.
- Assuming `split()` automatically removes punctuation or normalises capitalisation.
- Forgetting the negative sign in the `takeOrdered()` top-frequency pattern.
- Using Python's `and` or `or` to combine Spark column expressions.
- Confusing `groupBy()` with `agg()`.
- Describing DStreams as one RDD containing the entire continuous stream.
- Claiming that Structured Streaming always operates in continuous-processing mode.
- Treating the lecture's “up to 100 times faster” statement as a guarantee for every workload.

# 17\. Final revision checklist

Before moving on to T7, make sure you can do the following without consulting the notes.

- [ ]Explain why Spark is useful for large datasets.
- [ ]Explain vertical versus horizontal scaling.
- [ ]Identify the roles of the driver, SparkContext, cluster manager, workers, and executors.
- [ ]Define RDD and explain immutability, lineage, and fault tolerance.
- [ ]Create RDDs using `textFile()` and `parallelize()`.
- [ ]Classify common transformations and actions.
- [ ]Explain lazy evaluation and DAGs.
- [ ]Distinguish narrow and wide transformations and explain shuffles.
- [ ]Write the character-count exercise.
- [ ]Write the word-count exercise.
- [ ]Find the three most frequent words using `takeOrdered()`.
- [ ]Read a JSON DataFrame and inspect its schema.
- [ ]Use `select()`, `selectExpr()`, `where()`, and `orderBy()`.
- [ ]Aggregate values using `groupBy()` and `agg()`.
- [ ]Explain all five performance guidelines.
- [ ]Explain DStreams, StreamingContext, and batch intervals.
- [ ]Compare classic Spark Streaming with Structured Streaming.
- [ ]Recall the introductory NoSQL, ACID, BASE, and CAP concepts.

---

## Chapter completion

**T6 covers:** distributed computing, Spark architecture, RDDs, transformations, actions, PySpark exercises, DataFrames, performance, and streaming.

**Suggested GitHub filename:** `T6-Spark.md`

Keep the original lecture material alongside the compression whenever possible. The compression is a revision aid; the lecture remains the source of truth for the course's emphasis and terminology.

**Next step:** Proceed to T7 using the same compression format. Once all five chapters are complete, use the five compressions and, preferably, the original lecture notes to construct a practice examination with balanced chapter coverage, code exercises, conceptual questions, worked solutions, and a marking rubric.

## How to use this for the five-chapter workflow

- Save both parts in order in `T6-Spark.md`.
- Keep a copy of the original T6 lecture notes in the repository, too.
- For T7–T10, use the same headings and revision structure where relevant, adapting to each chapter's content.
- When all five are ready, submit the five compressions together. If possible, include the original lecture notes as well, so the practice exam can reflect the lecturer's actual material rather than just the summaries.

One important point: the practice exam will be more representative if we use the original lecture notes as the primary source and the compressions as revision guides. That also lets us identify any examinable detail accidentally omitted during compression.
