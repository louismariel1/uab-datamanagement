# UAB 106566 — Data Management
Exam Compression T6: Apache Spark
Academic year 2026/2027 · Bachelor's Degree in Artificial Intelligence · Universitat Autònoma de Barcelona

This compression is based on the lecture material you supplied, including the RDD exercises, DataFrame examples, performance guidelines and Spark Streaming slides. It is designed for closed-book exam preparation: learn the definitions, understand the distinctions, memorise the core code patterns, and practise explaining why a solution works.

The chapter's main emphasis is Apache Spark, particularly RDDs, transformations and actions, PySpark DataFrames, distributed execution, performance and streaming.

1. Chapter map: what you must know
1. Distributed computing and Spark architecture

Why Spark is needed, cluster execution, driver, SparkContext, workers and executors.

2. RDDs, transformations and actions

Immutability, lazy evaluation, DAGs, narrow versus wide transformations, MapReduce-style processing.

3. PySpark programming

Creating RDDs, using map/filter/flatMap, performing actions and solving word-count problems.

4. DataFrames and structured operations

select, selectExpr, col, where, orderBy, groupBy and agg; RDD versus DataFrame.

5. Spark performance

Shuffles, reduceByKey versus groupByKey, caching, partitions and avoiding collect on large datasets.

6. Spark Streaming

DStreams, RDDs per time interval, StreamingContext, micro-batches and Structured Streaming.

2. Distributed computing and why Spark exists
2.1 The problem with processing large datasets on one machine
Pandas is a Python library that provides convenient data structures, particularly DataFrames, for data analysis.

It is excellent for small and medium-sized datasets that fit the available resources of one machine. However, processing data at terabyte scale can exceed the machine's memory, storage or computational capacity.

Vertical scaling means increasing the resources of one machine. Horizontal scaling means distributing work across multiple machines.

Apache Spark enables distributed processing by dividing data into partitions and executing tasks across a cluster.

Pandas on one machine

Apache Spark on a cluster

Primarily single-machine processing

Distributed processing across multiple workers

Limited by one machine's resources

Can scale by adding cluster resources

Convenient for local analysis

Suitable for large-scale distributed workloads

No distributed shuffle between worker nodes

May require network shuffles between partitions

2.2 Spark versus Hadoop MapReduce
The lecture contrasts Spark with traditional Hadoop MapReduce.

Hadoop MapReduce: traditionally writes intermediate results to disk between processing stages.

Spark: can keep intermediate datasets in memory and cache them for reuse, reducing disk I/O when the workload benefits from reuse.

Spark can therefore be much faster for iterative algorithms and repeated computations over the same data.

However, the lecture's statement that Spark can be up to 100 times faster should be treated as a potential benchmark result, not a universal guarantee. Actual performance depends on the workload, data size, memory, storage and configuration. Spark can also spill intermediate data to disk.

2.3 Benefits of Spark
Memorise these five advantages:

Speed: in-memory computation and reduced disk I/O when applicable.

Scalability: distribute work across additional worker nodes.

Caching: reuse intermediate datasets rather than recomputing them.

High-level APIs: program using Python, Scala and Java.

Broad processing ecosystem: supports SQL, machine learning, graph processing and streaming.

2.4 Spark ecosystem
Component

Main purpose

Spark Core

Execution engine, distributed computation and fundamental abstractions

Spark SQL

Structured data processing using SQL and DataFrames

Spark Streaming

Stream processing using the DStream model

MLlib

Distributed machine-learning functionality

GraphX

Graph processing

Exam question to anticipate: Which component would you use for structured analytical queries, machine learning or graph computations? Explain the choice.

3. Spark architecture and execution model
3.1 Master–worker model
The lecture presents Spark as a master–worker distributed processing system.

Driver program

Coordinates the application and builds execution plans

SparkContext is the classic RDD API's entry point

Tasks are scheduled and distributed to executors

Worker 1

Executor tasks

Worker 2

Executor tasks

Worker N

Executor tasks

Data is divided into partitions that can be processed in parallel.

Know the distinction between these terms:

Driver: runs the main application, constructs execution plans, schedules tasks and coordinates computation.

SparkContext: the classic entry point for creating RDDs and interacting with Spark Core. Modern applications commonly use SparkSession, which provides access to DataFrame and SQL functionality and can expose the underlying SparkContext.

Cluster manager: allocates resources across applications, for example standalone Spark, YARN or Kubernetes.

Worker: a cluster machine that provides resources for computation.

Executor: a process that runs tasks and can cache data in memory or store intermediate data on disk.

The driver's role is coordination, not performing all the distributed data processing itself.

3.2 How a Spark program executes
A simplified lifecycle is:

Create the Spark application and its entry point.

Read data or create an RDD/DataFrame.

Define transformations.

Build a logical execution plan or dependency graph.

Call an action.

Spark schedules the required tasks across partitions and executors.

Results are returned to the driver or written to storage.

The important exam point is that transformations are lazy, so computation normally begins when an action requires a result.

4. RDDs: the fundamental distributed data abstraction
4.1 Definition and properties
An RDD (Resilient Distributed Dataset) is an immutable, distributed collection of elements that Spark can process in parallel.

Its defining properties are:

Resilient: Spark can recover lost partitions using lineage information, provided the necessary source data and dependencies remain available.

Distributed: data is partitioned across the cluster.

Dataset: the collection can contain elements such as numbers, strings, tuples or other objects.

Immutable: transformations create new RDDs rather than modifying existing ones.

Lazy: transformations are recorded for later execution.

Parallel: tasks can process different partitions concurrently.

Example:

python
numbers = sc.parallelize([1, 2, 3, 4, 5])

doubled = numbers.map(lambda x: x * 2)
numbers remains unchanged. doubled is a new RDD representing the transformed elements.

4.2 Creating RDDs in PySpark
The lecture covers two main methods.

Method 1: Read a text file

python
lines = sc.textFile("data/book.txt")
Each RDD element represents a line of text. The file may reside in a supported local or distributed filesystem, such as HDFS.

Method 2: Parallelise a Python collection

python
numbers = sc.parallelize([1, 2, 3, 4, 5], 4)
The second argument requests four partitions. It does not necessarily mean four physical worker machines; partitions are units of parallel processing.

A typical application can obtain the SparkContext through a SparkSession:

python
from pyspark.sql import SparkSession

spark = (
    SparkSession.builder
    .appName("DataManagement")
    .getOrCreate()
)

sc = spark.sparkContext
4.3 Transformations versus actions
This is one of the highest-priority concepts in the chapter.

Transformation

Action

Returns a new RDD

Returns a result or writes output

Normally lazy

Triggers the required computation

Adds to the dependency graph

Executes the required computation

Example: map()

Example: count()

Example:

python
numbers = sc.parallelize([1, 2, 3, 4, 5])

squares = numbers.map(lambda x: x * x)
# No result has been collected yet.

print(squares.collect())
# [1, 4, 9, 16, 25]
The transformation defines the calculation. collect() triggers execution and returns the resulting elements to the driver.

Exam trap: map() does not execute the complete computation immediately, and it does not modify the original RDD.

4.4 Essential RDD transformations
map() — one output per input

python
rdd = sc.parallelize([1, 2, 3, 4, 5])
result = rdd.map(lambda x: x * 3)
print(result.collect())
# [3, 6, 9, 12, 15]
Each input element produces one output element.

filter() — retain matching elements

python
rdd = sc.parallelize([1, 2, 3, 4, 5])
result = rdd.filter(lambda x: x % 2 == 0)
print(result.collect())
# [2, 4]
Elements that fail the condition are removed.

flatMap() — zero, one or many outputs per input

python
rdd = sc.parallelize([1, 2, 3])
result = rdd.flatMap(lambda x: [x, x * 10])
print(result.collect())
# [1, 10, 2, 20, 3, 30]
Unlike map(), flatMap() flattens the output collections into one RDD.

union() — combine two RDDs

python
a = sc.parallelize([2, 4, 6])
b = sc.parallelize([1, 3, 5])

result = a.union(b)
print(result.collect())
# [2, 4, 6, 1, 3, 5]
union() does not automatically remove duplicates or globally sort the results.

distinct() — remove duplicates

python
rdd = sc.parallelize([1, 2, 2, 3, 3])
result = rdd.distinct()
print(sorted(result.collect()))
# [1, 2, 3]
This commonly requires a shuffle to eliminate duplicates across partitions.

Other transformations explicitly mentioned in the lecture include sample(), groupByKey(), reduceByKey(), join() and sortByKey().

4.5 Narrow versus wide transformations
A narrow transformation allows each output partition to be computed from a limited set of input partitions, usually without redistributing data across the cluster.

A wide transformation requires data from multiple input partitions to be brought together, typically through a shuffle.

Operation

Typical classification

Reason

map()

Narrow

Each element is transformed locally

flatMap()

Narrow

Each element produces local output elements

filter()

Narrow

Rows are retained or removed locally

sample()

Narrow

Sampling is typically performed within partitions

union()

Usually narrow

Existing partitions can be combined without a data shuffle

distinct()

Wide

Duplicates must be identified across partitions

reduceByKey()

Wide overall

Values with matching keys must be aggregated; local combining can reduce shuffle traffic

groupByKey()

Wide

Values must be collected by key across partitions

join()

Often wide

Matching keys may require redistribution; some join strategies avoid a full shuffle

sortByKey()

Wide overall

Global ordering generally requires redistribution and coordination

Why do shuffles matter?

A shuffle can require network transfer, serialization, disk I/O and additional memory. It often creates a performance bottleneck.

A useful qualification for the exam is that classification describes the usual dependency and execution behaviour. Some operations and physical plans have implementation-specific variations.

4.6 Essential RDD actions
Action

Purpose

Example

count()

Count elements

rdd.count()

first()

Return the first element

rdd.first()

take(n)

Return up to the first 
n
n elements

rdd.take(3)

takeOrdered(n)

Return the 
n
n smallest elements by default

rdd.takeOrdered(3)

collect()

Return all elements to the driver

rdd.collect()

reduce()

Combine elements using a function

rdd.reduce(lambda a, b: a + b)

foreach()

Execute a function for each element

rdd.foreach(func)

saveAsTextFile()

Write elements to output files

rdd.saveAsTextFile("output")

Important details:

takeOrdered(3) returns the three smallest elements according to the default ordering, unless a custom ordering is provided.

collect() can exhaust driver memory when the dataset is large.

saveAsTextFile() writes distributed output, typically as multiple part files, rather than collecting all records into one Python list.

foreach() executes work on executors; it is not a way to build a normal Python list in the driver.

4.7 Lazy evaluation and lineage
Spark records transformations and their dependencies in a directed acyclic graph (DAG). When an action is called, Spark executes the stages and tasks needed to produce the result.

For example:

python
rdd = sc.parallelize([1, 2, 3, 4, 5])

filtered = rdd.filter(lambda x: x > 2)
squared = filtered.map(lambda x: x * x)

print(squared.collect())
# [9, 16, 25]
Before collect(), the transformations define the computation. The action triggers the execution required to produce the output.

Lineage and fault tolerance: if a partition is lost, Spark can often recompute it from its source data and transformation dependencies instead of requiring a complete backup of every intermediate RDD.

5. Core PySpark exercise patterns
These are especially important because the supplied lecture contains three exercises based on El Quijote: counting characters, counting word frequencies and finding the three most frequent words.

5.1 Exercise 1 — Count characters in a book
Task: Read the text file and count all characters across its lines. Assume four partitions.

python
lines = sc.textFile("data/quijote.txt", 4)

character_counts = lines.map(lambda line: len(line))

total_characters = character_counts.reduce(
    lambda a, b: a + b
)

print(total_characters)
Explanation:

textFile() creates an RDD of lines.

map(len) produces one character count per line.

reduce() adds all line counts to obtain the total.

The equivalent concise solution is:

python
total_characters = lines.map(len).sum()
The .sum() action is available on numeric RDDs.

Important assumption: This counts characters in the line strings returned by textFile(). It generally excludes newline separators removed during line reading. If the intended total includes line breaks, add the appropriate count of separators.

5.2 Exercise 2 — Count occurrences of every word
The classic solution uses a word-count pipeline.

python
lines = sc.textFile("data/quijote.txt", 4)

word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

word_counts.take(10)
The processing stages are:

Input lines

One RDD element per line

flatMap(split)

Turn lines into individual words

map(word → (word, 1))

Represent every occurrence as a key-value pair

reduceByKey(+)

Sum the counts for each word

Output

Pairs such as ("Mancha", 44)

The lecture gives example frequencies such as el → 8170 and Mancha → 44. These are examples from the provided material, not results guaranteed for a differently cleaned or tokenised version of the book.

Tokenisation caveat: split() separates on whitespace only. Thus, Mancha, Mancha, and Mancha. are different tokens. Punctuation and capitalisation must be normalised if the task requires them to be treated as equivalent.

A normalised version might be:

python
import re

word_counts = (
    lines
    .flatMap(
        lambda line: re.findall(r"\b\w+\b", line.lower())
    )
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
This version lowercases words and extracts alphanumeric word tokens. Its counts will differ from the lecture's basic whitespace-based example.

5.3 Exercise 3 — Find the three most frequent words
Building on the previous exercise:

python
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)

print(top_three)
This returns three pairs with the largest counts first, using alphabetical order to break ties.

For example, the output structure is:

python
[
    ("word_a", 12000),
    ("word_b", 9500),
    ("word_c", 8100)
]
The values above are illustrative, not actual book frequencies.

Why the negative count? takeOrdered() normally selects the smallest items. Using -pair[1] makes the largest frequencies rank first.

Alternative:

python
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
Do not confuse the three tasks:

Character count: map(len) followed by reduce() or .sum().

Word frequency: flatMap() → map((word, 1)) → reduceByKey(+).

Top three words: apply takeOrdered(3, key=...) to the word-frequency RDD.

6. PySpark DataFrames: the preferred API for structured data
6.1 What is a DataFrame?
A Spark DataFrame is a distributed table with named columns and a schema describing the data types.

Its rows are distributed across partitions, which Spark can process in parallel on different executors.

Compared with RDDs, DataFrames provide a higher-level, more declarative API. Spark can optimise structured operations using its query optimisation and execution engine.

6.2 RDD versus DataFrame
Feature

RDD

DataFrame

Abstraction

Low-level distributed collection

Structured table of rows and columns

Data schema

No required tabular schema

Named columns and defined data types

Programming style

Functional transformations

Declarative, SQL-like operations

Optimisation

Less automatic relational optimisation

Catalyst query optimisation

Ease of development

More manual for structured tasks

Usually simpler for structured data

Best use

Custom low-level processing

Filtering, joins, aggregation and analytics

Default rule: Prefer DataFrames for structured data; use RDDs when low-level control or custom element-level processing is required.

One nuance: a DataFrame is not simply an RDD with a different name. Modern Spark SQL plans structured operations through its optimised execution engine; RDDs remain a lower-level API.

6.3 Creating a DataFrame from JSON
The lecture demonstrates:

python
df = spark.read.format("json").load(
    "/data/genomics/json/summary.json"
)

df.printSchema()
spark.read.format("json") selects the JSON data source.

load() reads the data into a DataFrame.

printSchema() displays column names, types and nested structure.

To inspect the rows:

python
df.show()
show() is an action that displays a limited number of rows by default.

6.4 select() — choose columns and calculate expressions
Given a DataFrame c containing customer, product, quantity and price:

python
from pyspark.sql.functions import col

c.select("customer").show()
Select several columns:

python
c.select(
    "customer",
    "product"
).show()
Create a calculated column:

python
c.select(
    "customer",
    "product",
    (col("quantity") * col("price")).alias("total")
).show()
For the lecture's example rows, the calculation is:

Customer

Product

Quantity

Price

Total

200

5

8

33

264

200

8

5

111

555

100

8

5

45

225

100

8

5

59

295

col("quantity") refers to a DataFrame column. Multiplying two column expressions calculates a result for each row.

alias("total") names the resulting expression.

6.5 selectExpr() — use SQL expressions
selectExpr() allows column selection and calculations using SQL-like expression strings.

python
c.selectExpr(
    "customer",
    "product",
    "quantity * price AS total"
).show()
The expression is equivalent in purpose to the previous select() example.

Remember:

select() accepts column names and Spark column expressions.

selectExpr() accepts SQL expression strings.

Both return a new DataFrame; neither modifies the original DataFrame.

To retain all original columns while adding a calculated column, use:

python
c.selectExpr(
    "*",
    "quantity * price AS total"
).show()
6.6 Filtering rows: where() and filter()
The lecture introduces where() for retaining rows that satisfy a condition.

python
c.where(col("quantity") > 3).show()
Equivalent form:

python
c.filter(col("quantity") > 3).show()
Multiple conditions:

python
c.where(
    (col("quantity") > 0)
    & (col("price") > 0)
).show()
In PySpark column expressions, use & for logical AND and | for logical OR, and place comparisons in parentheses. Do not use Python's and or or to combine Spark column expressions.

6.7 Sorting rows: orderBy()
By default, orderBy() sorts in ascending order.

python
c.orderBy("price").show()
For descending order:

python
c.orderBy(
    col("price").desc()
).show()
To sort by multiple columns:

python
c.orderBy(
    col("customer").asc(),
    col("price").desc()
).show()
Unlike sorting a small Python list locally, a distributed global sort may require a shuffle and can be expensive.

6.8 Aggregation: groupBy() and agg()
Use groupBy() to group rows according to one or more columns, and agg() to calculate aggregate values.

Example: total quantity per customer.

python
from pyspark.sql.functions import sum

c.groupBy("customer").agg(
    sum("quantity").alias("total_quantity")
).show()
Other useful aggregate functions include avg(), max(), min() and count().

Example: total sales value per customer:

python
c.groupBy("customer").agg(
    sum(col("quantity") * col("price")).alias("total_sales")
).show()
Exam distinction: groupBy() specifies the groups; agg() specifies the computations performed for each group.

6.9 DataFrame code patterns to memorise
Requirement

PySpark pattern

Read JSON

spark.read.format("json").load(path)

Inspect schema

df.printSchema()

Select columns

df.select("a", "b")

Calculate a column in a selection

df.select((col("a") * col("b")).alias("total"))

Use SQL expressions

df.selectExpr("a", "b * c AS total")

Filter rows

df.where(col("quantity") > 0)

Sort ascending

df.orderBy("price")

Sort descending

df.orderBy(col("price").desc())

Group and sum

df.groupBy("customer").agg(sum("quantity"))

7. Spark performance: essential design rules
The lecture explicitly highlights five performance guidelines. These are likely candidates for conceptual questions and real-world scenarios.

Rule 1 — Prefer reduceByKey() over groupByKey() for aggregation
Suppose the RDD contains:

python
[("A", 1), ("A", 1), ("B", 1), ("A", 1)]
Using reduceByKey():

python
counts = rdd.reduceByKey(lambda a, b: a + b)
produces key-value aggregates equivalent to:

python
[("A", 3), ("B", 1)]
By contrast, groupByKey() groups all values for each key into an iterable, requiring more data to be transferred and retained when many values share the same key.

Why reduceByKey() is generally preferable for aggregation: Spark can combine values locally before shuffling them across the network, reducing the volume of intermediate data.

Use groupByKey() when you actually need all the values associated with each key, not simply a sum, count or similar aggregate.

Rule 2 — Minimise shuffles
Shuffles redistribute data across partitions. They can incur network, serialization and disk costs.

Examples that commonly require shuffles include distinct(), groupByKey(), reduceByKey() and global sorting.

Ways to reduce shuffle overhead include filtering unnecessary records early, avoiding redundant wide transformations, using suitable aggregation operations and selecting appropriate join or partitioning strategies.

Do not assume every transformation causes a shuffle.

Rule 3 — Cache datasets that will be reused
If a costly RDD is reused by several actions, caching may avoid repeated recomputation.

python
filtered = lines.filter(
    lambda line: "Spark" in line
).cache()

print(filtered.count())
print(filtered.take(10))
Caching is useful when the data is reused and the saved recomputation cost outweighs the storage and memory overhead.

It is not automatically beneficial for every DataFrame or RDD. Cached data may consume resources that other tasks need, and a cache can be recomputed if partitions are lost.

Rule 4 — Avoid collect() on large datasets
python
result = rdd.collect()
collect() transfers all RDD elements to the driver. If the result is too large, the driver can run out of memory.

Prefer:

python
rdd.take(10)
for a small sample, or use distributed aggregation and write results to external storage.

For example, reduce() can return one aggregate value instead of every input element.

Rule 5 — Choose an appropriate number of partitions
Too few partitions may limit parallelism and create oversized tasks. Too many tiny partitions may add scheduling overhead and create many small output files.

The appropriate number depends on the dataset size, available resources, operation costs and desired task granularity.

The lecture's example assumes four worker nodes. It is important not to confuse four workers with exactly four partitions: a cluster can execute multiple partitions per worker, and partitions are not the same thing as physical machines.

8. Spark Streaming: fundamentals
8.1 What is Spark Streaming?
Spark Streaming enables continuous processing of incoming data, such as social media messages, click events or application logs.

It uses the Spark execution engine and allows streaming workloads to share concepts and APIs with batch processing.

The lecture emphasises three benefits:

Continuous analysis: process new events as they arrive.

Familiar programming model: use Spark-style transformations and processing logic.

Ecosystem integration: connect to data sources and systems such as Kafka and Hadoop-related storage.

A key distinction is that Spark Streaming, as presented in the lecture, processes streaming data in micro-batches, rather than treating every event as a completely independent computation.

8.2 DStreams
A DStream (Discretized Stream) is the abstraction used by the original Spark Streaming API.

A DStream represents a continuous sequence of RDDs, with each RDD corresponding to data received during a particular time interval.

Continuous input stream

Events arriving over time

Split incoming data by batch interval

Batch 1

RDD 1

0–1 seconds

Batch 2

RDD 2

1–2 seconds

Batch 3

RDD 3

2–3 seconds

Process each batch

Apply Spark operations and produce results

The time intervals in this diagram are illustrative.

8.3 StreamingContext and batch interval
In the classic DStream API, the StreamingContext is the entry point for configuring and running a streaming application.

It uses a SparkContext and a specified batch interval.

Conceptually:

Create a SparkContext.

Create a StreamingContext with the desired batch interval.

Define the input stream.

Apply transformations and actions to process each batch.

Start the streaming computation.

The batch interval must reflect the application's latency requirements and the resources available.

A shorter interval may reduce the delay before processing each batch, but can increase scheduling overhead and make it harder to keep up with incoming data.

8.4 Example: count words received over a TCP socket
The lecture asks you to count words received from a server listening on port 9999.

A classic PySpark DStream example is:

python
from pyspark import SparkContext
from pyspark.streaming import StreamingContext

sc = SparkContext(
    appName="StreamingWordCount"
)

ssc = StreamingContext(sc, 1)

lines = ssc.socketTextStream(
    "localhost",
    9999
)

word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

word_counts.pprint()

ssc.start()
ssc.awaitTermination()
This example uses a one-second batch interval. The socket server must be running and accessible, and the classic streaming API must be available in the Spark environment.

Important: This is the older DStream API used to explain the lecture's concepts. A raw TCP socket example is useful for learning, but a production application requires appropriate source reliability, checkpointing and recovery considerations.

8.5 DStreams versus Structured Streaming
The lecture contrasts the original Spark Streaming API with the modern Structured Streaming approach.

Feature

DStream API

Structured Streaming

Fundamental abstraction

Sequence of RDDs

Structured streaming DataFrames/Datasets

Programming style

Lower-level streaming operations

Similar to batch DataFrame and SQL operations

Query optimisation

Less structured-query optimisation

Uses Spark SQL's optimisation framework

Development

RDD-oriented code

Generally simpler for structured analytics

Recommended approach for new structured applications

Legacy API

Generally preferred

Structured Streaming supports incremental processing and, depending on the query and configuration, can use micro-batch execution or continuous processing modes where supported. It should not be assumed that every Structured Streaming query has identical latency or execution guarantees.

Exam takeaway: Know the DStream model because it is explicitly taught, but recognise Structured Streaming as the preferred modern API for most new structured streaming applications.

9. NoSQL recap: what you need from the beginning of T6
Although Apache Spark is the main subject, the lecture begins with a short recap of NoSQL databases.

Remember these concepts:

NoSQL systems commonly prioritise flexibility, scalability, performance and availability.

Database design involves trade-offs, including consistency and availability considerations under distributed failures.

Common models include document, key-value, column-family and graph databases.

MongoDB is a document database; Redis is an in-memory key-value store with additional data structures.

Redis can support caching, persistence and distributed deployment.

No database is universally best: the choice depends on the application's requirements.

The recap refers to ACID, BASE and CAP. For this chapter, understand their broad roles:

ACID: atomicity, consistency, isolation and durability; properties associated with reliable transactions.

BASE: basically available, soft state and eventual consistency; a conceptual approach associated with some highly distributed systems.

CAP theorem: during a network partition, a distributed system cannot guarantee both consistency and availability in the strict CAP sense simultaneously.

These are trade-offs rather than simple labels that determine every property of a database. The supplied lecture does not develop these topics in detail, so they should not displace Spark revision time.

10. Closed-book memory sheet
This is the compressed list to memorise and reproduce without consulting the lecture.

Essential definitions
Term

Definition to remember

Apache Spark

Distributed processing engine for large-scale data

RDD

Immutable, partitioned collection processed in parallel

Transformation

Creates a new RDD/DataFrame or defines a new computation

Action

Triggers required computation and returns a result or writes output

Lazy evaluation

Defers computation until an action requires a result

DAG

Directed acyclic graph representing computation dependencies

Narrow transformation

Output partition depends on a limited set of input partitions; normally no shuffle

Wide transformation

Output partition depends on multiple input partitions; normally requires a shuffle

Shuffle

Redistribution of data across partitions, often across the network

DataFrame

Distributed table with named columns and a schema

DStream

Time-ordered sequence of RDDs in classic Spark Streaming

reduceByKey()

Aggregate values by key, with local combining where possible

collect()

Transfer all RDD elements to the driver

Essential code patterns
python
# Create RDDs
rdd = sc.parallelize([1, 2, 3, 4, 5], 4)
lines = sc.textFile("data/book.txt")

# Transformations
rdd.map(lambda x: x * 2)
rdd.filter(lambda x: x % 2 == 0)
rdd.flatMap(lambda x: [x, x * 10])
rdd.distinct()

# Actions
rdd.count()
rdd.first()
rdd.take(3)
rdd.collect()
rdd.reduce(lambda a, b: a + b)

# Word count
counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

# DataFrame operations
df.select("customer", "product")
df.where(col("quantity") > 0)
df.orderBy(col("price").desc())
df.groupBy("customer").agg(sum("quantity"))
df.selectExpr("*", "quantity * price AS total")
Five conceptual questions you must answer correctly
Which operations trigger a shuffle? Common examples include distinct(), reduceByKey(), groupByKey() and global sorting. Joins often shuffle too, depending on the execution strategy.

Why is reduceByKey() preferred over groupByKey() for summation? It allows local combining before the shuffle, reducing the amount of data transferred.

What happens when collect() executes? Spark computes the required result and transfers every element of the RDD to the driver, which can exhaust driver memory.

What is the difference between transformations and actions? Transformations define new computations; actions trigger execution and return or write results.

Why use DataFrames instead of RDDs for structured data? DataFrames provide a highe
