# T5 — Spark Exam Compression (Part 2)

## 6. PySpark: RDD creation, transformations and actions

### Creating RDDs

A `SparkContext` (`sc`) is the entry point for working with RDDs.

python
```
# Create an RDD from a Python collection
rdd = sc.parallelize([1, 2, 3, 4, 5])

# Create an RDD from a text file
lines = sc.textFile("data.txt")
```

- `parallelize(collection)`: distributes a local Python collection across Spark partitions.
- `textFile(path)`: reads a text file; each RDD element represents a line.
- Data can be read from supported storage systems, including distributed storage.

### Transformations

Transformations create new RDDs and are evaluated lazily.

python
```
rdd = sc.parallelize([1, 2, 3, 4, 5])

doubled = rdd.map(lambda x: x * 2)
evens = rdd.filter(lambda x: x % 2 == 0)
expanded = rdd.flatMap(lambda x: [x, x * 10])
combined = evens.union(doubled)
```

| Transformation | Meaning | Example result |
| --- | --- | --- |
| `map()` | One output per input element | `[1,2,3]` → `[2,4,6]` |
| `flatMap()` | Zero, one or many outputs per input | `[1,2]` → `[1,10,2,20]` |
| `filter()` | Keep elements satisfying a condition | `[1,2,3,4]` → `[2,4]` |
| `union()` | Combine two RDDs | `[1,2]` + `[3,4]` → `[1,2,3,4]` |
| `distinct()` | Remove duplicates; generally requires shuffle | `[1,1,2]` → `[1,2]` |
| `reduceByKey()` | Aggregate values sharing a key | Sum values per key |
| `groupByKey()` | Group all values for each key | Values collected per key |
| `join()` | Match records with the same key | Join matching key-value pairs |
| `sortByKey()` | Sort key-value records by key | Ordered key-value pairs |

Narrow vs. wide transformations

- Narrow: Each output partition depends on one input partition; generally no shuffle. Examples: `map()`, `filter()`, `flatMap()`.
- Wide: Output partitions depend on data from multiple input partitions; a shuffle is typically required. Examples: `distinct()`, `reduceByKey()`, `groupByKey()`, `join()`, `sortByKey()`.

`union()` is generally narrow when corresponding partitions can be processed independently.

### Actions

Actions trigger execution of the required lazy transformations.

| Action | Purpose |
| --- | --- |
| `count()` | Return the number of elements |
| `collect()` | Return all elements to the driver |
| `first()` | Return the first element |
| `take(n)` | Return up to the first `n` elements |
| `takeOrdered(n)` | Return the smallest `n` elements by default |
| `reduce(func)` | Combine elements using a function |
| `foreach(func)` | Apply an operation to each element |
| `saveAsTextFile(path)` | Write results to storage |

Example:

python
```
rdd = sc.parallelize([1, 2, 3, 4, 5])

mapped = rdd.map(lambda x: x * 2)  # Lazy
print(mapped.collect())            # Action triggers execution
# [2, 4, 6, 8, 10]
```

Important exam distinction:

- A transformation returns a new RDD.
- An action triggers computation and returns a result or writes output.
- An action does not create a new RDD as its result.
- `collect()` can exhaust driver memory on large datasets; avoid using it indiscriminately.

## 7. PySpark DataFrames

### What is a DataFrame?

A DataFrame is a distributed, structured table with named columns and defined data types. Its data is divided into partitions for parallel processing across executors.

Compared with RDDs, DataFrames provide a higher-level, more declarative API and allow Spark's Catalyst optimizer to optimize query execution.

### Loading data and inspecting schema

python
```
df = spark.read.format("json").load("/data/summary.json")

df.printSchema()
df.show()
```

- `printSchema()`: displays column names, data types and nested structure.
- `show()`: displays a sample of rows.
- `spark.read`: loads data into a DataFrame.

### Selecting columns

python
```
df.select("customer").show()

df.select("customer", "product").show()
```

Use `col()` to refer to a column in expressions:

python
```
from pyspark.sql.functions import col

df.select(
    "customer",
    "product",
    (col("quantity") * col("price")).alias("total")
).show()
```

The calculated `total` column contains quantity multiplied by price.

### `selectExpr()`

`selectExpr()` allows SQL-style expressions as strings.

python
```
df.selectExpr(
    "customer",
    "product",
    "quantity * price AS total"
).show()
```

This is conceptually equivalent to using `col()` and `.alias()` in `select()`.

### Filtering rows

`where()` and `filter()` select rows satisfying a condition.

python
```
df.where(col("quantity") > 5).show()

df.filter(col("price") > 50).show()
```

SQL-style expressions can also be used:

python
```
from pyspark.sql.functions import expr

df.where(expr("quantity > 5")).show()
```

These operations return a new DataFrame and are lazily evaluated.

### Sorting rows

`orderBy()` sorts rows by column values. Ascending order is the default.

python
```
df.orderBy("price").show()                  # Ascending
df.orderBy(col("price").desc()).show()      # Descending
```

### Aggregations with `groupBy()` and `agg()`

`groupBy()` groups rows by one or more columns. `agg()` applies aggregate functions to the resulting groups.

python
```
from pyspark.sql.functions import sum, avg, max, min

df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity"),
    avg("price").alias("average_price"),
    max("price").alias("maximum_price"),
    min("price").alias("minimum_price")
).show()
```

To calculate the total quantity per customer:

python
```
df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity")
).show()
```

Remember: `groupBy()` defines the groups; `agg()` defines the calculations applied to each group.

## 8. RDD vs. DataFrame

| Feature | RDD | DataFrame |
| --- | --- | --- |
| Abstraction | Low-level distributed collection | High-level structured table |
| Data access | Individual objects | Named columns and rows |
| Programming style | Python functions | SQL-like expressions and APIs |
| Optimization | Less automatic query optimization | Catalyst query optimization |
| Development | More manual control | Generally easier |
| Best use | Low-level control or specialized operations | Structured data processing |

Exam recommendation: Prefer DataFrames for most structured data processing. Use RDDs when low-level control is necessary.

## 9. Spark Streaming

### Purpose

Spark Streaming processes continuously arriving data, such as social media events or data received from a network socket.

Benefits include:

- Near-real-time data analysis.
- An API familiar to Spark developers.
- Integration with other big-data technologies, such as Kafka and Hadoop ecosystem tools.
- The ability to combine streaming and batch-processing workflows.

### DStreams

A DStream (Discretized Stream) is the fundamental abstraction in the original Spark Streaming API.

- A DStream represents a continuous sequence of RDDs ordered over time.
- Each RDD contains the data associated with a particular batch interval.
- Spark processes incoming data in micro-batches rather than processing each event individually as it arrives.
- The RDDs can be processed using familiar Spark operations.

### StreamingContext

`StreamingContext` is the entry point for the original Spark Streaming API.

It requires a `SparkContext` and a batch interval that determines how frequently incoming data is grouped into batches.

The batch interval should be selected based on application latency requirements and available computing resources.

### DStreams vs. Structured Streaming

| Feature | DStreams | Structured Streaming |
| --- | --- | --- |
| Data abstraction | RDDs | DataFrames / Datasets |
| API level | Lower-level | Higher-level, declarative |
| Optimization | Less SQL-style optimization | Uses Spark SQL optimization |
| Development | More manual | Generally easier to maintain |
| Typical recommendation | Legacy applications | Preferred for new applications |

Structured Streaming is generally the recommended approach for new Spark streaming applications.

## 10. MapReduce and Spark execution model

The lecture presents Spark through the MapReduce programming paradigm.

- Map: Transforms each input element into one or more intermediate outputs.
- Reduce: Aggregates intermediate outputs to produce a final result.

In Spark, transformations resemble the map stage, while aggregation actions such as `reduce()` can resemble the reduce stage. However, Spark is not limited to a strict two-stage MapReduce execution model.

### Spark program lifecycle

1. Create or load an RDD/DataFrame.
2. Define transformations.
3. Spark records the transformation lineage and builds an execution plan.
4. Call an action.
5. Spark executes the necessary computation, potentially across multiple stages and partitions.
6. Return results to the driver or write them to storage.

The execution plan is represented through a DAG (Directed Acyclic Graph).

## 11. Performance guidelines

These are particularly important for conceptual questions and code optimization.

- Prefer `reduceByKey()` over `groupByKey()` when aggregating values by key. `reduceByKey()` can combine values locally before shuffling, reducing network traffic.
- Minimize shuffles. Data redistribution across the cluster is expensive.
- Cache reused datasets. Caching can avoid recomputing expensive transformations when the same dataset is used repeatedly.
- Avoid `collect()` on large datasets. It transfers all results to the driver and may cause memory exhaustion.
- Choose an appropriate number of partitions. Too few can limit parallelism; too many can increase scheduling overhead.
- Use DataFrames by default for structured data because Spark can optimize execution.

Caching is not automatically beneficial for every dataset: it consumes memory or storage resources, so it should be used when reuse justifies the cost.

## 12. PySpark exercise patterns

### Exercise A: Count all characters in a book

Assume the text file contains one line per RDD element.

python
```
lines = sc.textFile("quijote.txt")

total_characters = lines.map(len).sum()

print(total_characters)
```

Conceptual steps:

1. Read the file into an RDD of lines.
2. Map each line to its character count.
3. Sum all line counts.
4. Return the total.

This counts characters within each line. Newline characters separating lines are not necessarily included, depending on how the file is read.

### Exercise B: Count occurrences of each word

python
```
lines = sc.textFile("quijote.txt")

word_counts = (
    lines.flatMap(lambda line: line.split())
         .map(lambda word: (word, 1))
         .reduceByKey(lambda a, b: a + b)
)

word_counts.collect()
```

Conceptual steps:

1. `flatMap()` splits lines into words.
2. `map()` creates `(word, 1)` pairs.
3. `reduceByKey()` sums the counts for each word.

The exact result depends on tokenization and normalization. For example, `El`, `el`, and `el,` may be counted separately unless the text is normalized.

### Exercise C: Find the three most frequent words

python
```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: -pair[1]
)

print(top_three)
```

This returns the three highest-frequency `(word, count)` pairs. If tied counts occur, the ordering among ties may need additional criteria.

### Exercise D: Why four worker nodes do not mean four RDD elements

An RDD is divided into partitions, and partitions are scheduled across available executors. The number of workers and the number of partitions are not necessarily equal. Four workers may process more than four partitions, depending on the configuration.

## 13. Common exam questions and model answers

Q1. Why is Spark preferred over single-machine Pandas for very large datasets?

Spark distributes data and computation across a cluster, allowing horizontal scaling beyond the memory and processing limits of one machine.

Q2. What is the difference between a transformation and an action?

A transformation lazily defines a new RDD or DataFrame. An action triggers execution and returns a result or writes output.

Q3. What is the difference between narrow and wide transformations?

Narrow transformations generally process data within individual partitions without shuffling. Wide transformations require data from multiple partitions and typically involve a shuffle.

Q4. Why is `reduceByKey()` often better than `groupByKey()`?

`reduceByKey()` can aggregate values locally before the shuffle. `groupByKey()` transfers all values for a key to the grouping stage, potentially requiring more network traffic and memory.

Q5. Why can `collect()` be dangerous?

It transfers the entire result to the driver, which may exceed the driver's available memory.

Q6. What is an RDD?

A Resilient Distributed Dataset is an immutable distributed collection processed in parallel. Its lineage helps Spark recompute lost partitions after failures.

Q7. What is lazy evaluation?

Spark delays executing transformations until an action requires their results. This allows Spark to plan and optimize computation.

Q8. What is the role of the driver and executors?

The driver coordinates the Spark application and schedules work. Executors run tasks on worker nodes and can cache intermediate data.

Q9. What is the difference between `map()` and `flatMap()`?

`map()` produces one output element per input element. `flatMap()` can produce zero, one, or many output elements per input and flattens the resulting sequences.

Q10. What is a DStream?

A DStream is the original Spark Streaming abstraction representing a continuous sequence of RDDs, one for each batch interval.

Q11. Why are DataFrames generally preferred over RDDs?

DataFrames provide a structured, declarative API and enable Catalyst optimization, generally making structured data operations easier and more efficient.

Q12. What does `groupBy().agg()` do?

It groups records by specified column values and computes summary statistics, such as sums, averages, minimums, or maximums, for each group.

## 14. Last-minute revision checklist

Before the exam, make sure you can explain or implement each of the following:

- [ ]Why distributed computing is necessary for large datasets.
- [ ]Spark's driver, executors, workers and SparkContext.
- [ ]The purpose of Spark Core, Spark SQL, Streaming, MLlib and GraphX.
- [ ]RDD immutability, distribution and fault tolerance.
- [ ]Transformations versus actions and lazy evaluation.
- [ ]Narrow versus wide transformations and shuffling.
- [ ]`map()`, `flatMap()`, `filter()`, `union()` and `reduceByKey()`.
- [ ]`count()`, `collect()`, `takeOrdered()`, `reduce()` and `saveAsTextFile()`.
- [ ]Creating RDDs with `parallelize()` and `textFile()`.
- [ ]DataFrame selection, filtering, sorting and aggregation.
- [ ]The difference between RDDs and DataFrames.
- [ ]DStreams, batch intervals and Structured Streaming.
- [ ]Word-count and character-count problems.
- [ ]Why `reduceByKey()` is preferable to `groupByKey()` for aggregation.

## GitHub workflow for all five chapters

Save this as `T6-Spark-Exam-Compression.md`. For the next four chapters, use the same format so the final revision material remains consistent.

Once all five chapter compressions are ready, submit them together. I can then generate a practice exam grounded in the actual lecture notes, with a mixture of conceptual questions, code interpretation, debugging, short-answer questions and practical exercises, plus a separate answer key and explanations.

One important distinction: this compression preserves the material supplied here and adds clarifications where necessary. It cannot guarantee that every examinable detail from the original slides is present, especially details contained only in the extracted figures.
