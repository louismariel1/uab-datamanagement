I’ll structure the practice exams as a GitHub-ready Markdown document, with three independent 120-minute exams, each worth 100 marks, followed by comprehensive model solutions, marking rubrics, code explanations, and common exam traps. All questions will be based on your supplied T5/T6 Apache Spark compression, with a balanced mix of conceptual knowledge, multiple-choice questions, PySpark exercises, DataFrame queries, performance analysis, streaming, and introductory NoSQL.

Writing

# T5 — Apache Spark: Three 120-Minute Practice Exams

**Course:** UAB 106566 — Data Management\
**Academic year:** 2026/2027\
**Degree:** Bachelor's Degree in Artificial Intelligence\
**University:** Universitat Autònoma de Barcelona

**Format:** GitHub Markdown (`.md`)\
**Number of exams:** 3\
**Duration:** 120 minutes per exam\
**Total per exam:** 100 marks\
**Scope:** Apache Spark, RDDs, PySpark, DataFrames, execution plans, performance, Spark Streaming, NoSQL, ACID, BASE and CAP.

---

## Table of contents

- Instructions and assessment framework
- Practice Exam 1 — Fundamentals and RDDs
- Solutions to Exam 1
- Practice Exam 2 — DataFrames and Performance
- Solutions to Exam 2
- Practice Exam 3 — Integrated Spark Applications
- Solutions to Exam 3
- Consolidated marking rubric
- Final revision checklist

---

# Instructions and assessment framework

## General instructions

1. Attempt each exam under closed-book conditions before reading its solutions.
2. Each exam lasts exactly 120 minutes and is worth 100 marks.
3. Answer all questions. Where a question asks for code, provide valid or clearly explained PySpark.
4. Unless stated otherwise, assume a working PySpark environment with a correctly configured `SparkSession` and `SparkContext`.
5. Assume that input files exist and that their contents are readable.
6. Where output ordering is not guaranteed, distinguish between the expected contents and the expected order.
7. Explain the reasoning behind performance recommendations rather than merely naming an API method.
8. In code questions, equivalent correct solutions receive credit.

## Suggested time allocation

| Section | Topic | Marks | Time |
| --- | --- | --- | --- |
| A | Multiple-choice questions | 10 | 12 min |
| B | Conceptual short answers | 20 | 22 min |
| C | RDD programming and interpretation | 25 | 30 min |
| D | DataFrames and query interpretation | 20 | 24 min |
| E | Performance, streaming and NoSQL | 25 | 32 min |
| **Total** |  | **100** | **120 min** |

The sections may be rearranged within the exam, but the time allocation provides a useful pacing target.

## Marking principles

- **Conceptual answers:** Correct definition, accurate explanation and appropriate terminology.
- **Code questions:** Correct transformation sequence, valid API usage and correct output or result.
- **Performance questions:** Identify the bottleneck, explain its cause and recommend a justified improvement.
- **Debugging questions:** Identify the defect, explain why it occurs and provide a correction.
- **Partial credit:** A correct method with a minor syntax error may receive partial marks. Code that uses the wrong execution model or produces an incorrect result receives less credit.
- **No universal performance claims:** Recommendations must account for data size, partitioning, memory and workload.

---

# Practice Exam 1 — Fundamentals and RDDs

**Duration:** 120 minutes\
**Maximum score:** 100 marks

## Section A — Multiple-choice questions (10 marks)

**Instructions:** Select exactly one answer per question. Each question is worth 1 mark. There is no negative marking.

### Question 1.1 — Lazy evaluation

When is an RDD transformation such as `map()` normally executed?

A. Immediately when the Python statement is evaluated.\
B. When a subsequent action requires its result.\
C. Only when the Spark application terminates.\
D. Only when the RDD is explicitly cached.

### Question 1.2 — Application architecture

Which component coordinates an application's execution and schedules tasks?

A. Worker\
B. Partition\
C. Driver\
D. Executor

### Question 1.3 — Transformation versus action

Which operation is an action?

A. `filter()`\
B. `flatMap()`\
C. `distinct()`\
D. `count()`

### Question 1.4 — Distributed processing

What is horizontal scaling?

A. Increasing the RAM of one machine.\
B. Increasing the CPU speed of one machine.\
C. Adding machines or distributing computation across more nodes.\
D. Replacing a DataFrame with an RDD.

### Question 1.5 — Word counting

Which operation is most appropriate for aggregating word counts by key?

A. `groupByKey()` in every case\
B. `reduceByKey(lambda a, b: a + b)`\
C. `union()`\
D. `collect()`

### Question 1.6 — Driver memory

What is the main risk of calling `collect()` on an enormous RDD?

A. It automatically deletes the input data.\
B. It forces all executors to become drivers.\
C. It may exhaust the driver's memory.\
D. It always removes duplicate records.

### Question 1.7 — Shuffle

Which operation typically requires a shuffle?

A. `map()`\
B. `filter()`\
C. `flatMap()`\
D. `distinct()`

### Question 1.8 — DataFrames

Which statement about Spark DataFrames is correct?

A. They must fit entirely in the driver's memory.\
B. They provide named columns and a schema and support structured query optimisation.\
C. They cannot perform aggregations.\
D. They are always faster than every possible RDD implementation.

### Question 1.9 — Classic Spark Streaming

A DStream represents:

A. One permanent RDD containing every event ever received.\
B. A time-ordered sequence of RDDs associated with batch intervals.\
C. A database table stored only in driver memory.\
D. A single executor's task queue.

### Question 1.10 — CAP theorem

According to CAP, what trade-off arises when a network partition occurs?

A. A system cannot use both RAM and disk.\
B. A distributed system cannot guarantee both strict consistency and availability during the partition.\
C. A database must abandon durability permanently.\
D. Every distributed database must become eventually consistent in every situation.

---

## Section B — Conceptual short answers (20 marks)

### Question 1.11 — Spark architecture (5 marks)

Explain the roles of the following components:

1. Driver
2. Cluster manager
3. Executor
4. Partition
5. `SparkSession`

Give one sentence for each component.

### Question 1.12 — RDD properties and lineage (5 marks)

Define an RDD and explain:

- Immutability.
- Lazy evaluation.
- Lineage.
- Fault tolerance.

Explain how lineage helps Spark recover from the loss of a partition.

### Question 1.13 — Narrow and wide transformations (5 marks)

Classify the following as typically narrow or wide:

- `map()`
- `filter()`
- `distinct()`
- `reduceByKey()`
- `sortByKey()`

Explain why wide transformations can be expensive.

### Question 1.14 — Spark versus single-machine processing (5 marks)

A dataset is too large to fit comfortably in the RAM of one computer.

Explain why Spark may be suitable, and distinguish horizontal scaling from vertical scaling. Give one limitation or cost of distributed processing.

---

## Section C — RDD programming and interpretation (25 marks)

### Question 1.15 — Trace transformations and actions (7 marks)

Consider the following code:

```
rdd = sc.parallelize([1, 2, 3, 4, 5, 6], 3)

filtered = rdd.filter(lambda x: x % 2 == 0)
result = filtered.map(lambda x: x * x)

print(result.take(2))
print(result.count())
```

Answer the following:

1. What is the content of `filtered`? (2 marks)
2. What can `result.take(2)` return? (2 marks)
3. What is the value of `result.count()`? (1 mark)
4. Do the transformation assignments execute all their computations immediately? Explain. (2 marks)

### Question 1.16 — Character counting (5 marks)

Write PySpark code to calculate the total number of characters in `data/book.txt`.

Requirements:

- Read the file into an RDD of lines.
- Use four requested partitions.
- Count the characters in each line.
- Produce one total without collecting every line at the driver.

Explain the computation in two or three sentences.

### Question 1.17 — Word counting (7 marks)

Write a word-count program for `data/book.txt`.

The result must be an RDD of `(word, count)` pairs.

Requirements:

- Split each line on whitespace.
- Preserve the original token case and punctuation.
- Count all occurrences of each resulting token.

Explain the role of each transformation.

### Question 1.18 — Top three words (6 marks)

Using the RDD produced in Question 1.17:

1. Write code that returns the three most frequent words. (3 marks)
2. Break ties alphabetically by word. (1 mark)
3. Explain why the negative sign in the ordering key is needed. (2 marks)

---

## Section D — DataFrames and query interpretation (20 marks)

Use the following dataset throughout this section.

| customer | product | quantity | price |
| --- | --- | --- | --- |
| 200 | 5 | 8 | 33 |
| 200 | 8 | 5 | 111 |
| 100 | 8 | 5 | 45 |
| 100 | 8 | 5 | 59 |

Assume these rows are loaded into a DataFrame named `df` with the corresponding column names.

### Question 1.19 — Calculated column (5 marks)

Write PySpark code that selects `customer`, `product` and a new column called `total`, calculated as:

$$
\text{total} = \text{quantity} \times \text{price}
$$

State the four resulting totals.

### Question 1.20 — Filtering and sorting (5 marks)

Write code to select rows where `price > 50`, then sort them by descending price.

State the expected rows in order.

### Question 1.21 — Aggregation (6 marks)

Write code to calculate the total sales value per customer, where sales value is `quantity × price`.

Calculate the expected total for each customer.

### Question 1.22 — `select()` versus `selectExpr()` (4 marks)

Explain the difference between `select()` and `selectExpr()`. Give one valid example of each for calculating `quantity * price` as `total`.

---

## Section E — Performance, streaming and NoSQL (25 marks)

### Question 1.23 — Performance diagnosis (7 marks)

A program counts events using this pipeline:

```
events = sc.textFile("data/events.txt")

pairs = events.flatMap(
    lambda line: line.split()
).map(
    lambda word: (word, 1)
)

grouped = pairs.groupByKey()

counts = grouped.mapValues(
    lambda values: sum(values)
)

print(counts.collect())
```

Identify **three** performance or scalability concerns. For each, propose an improvement and explain why it helps.

### Question 1.24 — Caching (5 marks)

A costly filtered RDD is used by four subsequent actions.

Explain:

1. Why caching may improve performance. (2 marks)
2. Why calling `.cache()` does not immediately compute every partition. (1 mark)
3. Two costs or limitations of caching. (2 marks)

### Question 1.25 — Classic Spark Streaming (5 marks)

Explain how a DStream processes incoming data. In your answer, describe the role of `StreamingContext`, RDDs and the batch interval.

### Question 1.26 — NoSQL and consistency (8 marks)

Answer the following:

1. Name four NoSQL data models and give one example of a system or application associated with each. (4 marks)
2. Expand ACID. (2 marks)
3. Explain the difference between BASE and ACID at a conceptual level. (1 mark)
4. State the trade-off described by CAP during a network partition. (1 mark)

---

# Solutions to Exam 1

## Section A — Multiple-choice answer key (10 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| 1.1 | B | Transformations are normally lazy; an action triggers the required computation. |
| 1.2 | C | The driver coordinates application execution and schedules tasks. |
| 1.3 | D | `count()` is an action that returns the number of elements. |
| 1.4 | C | Horizontal scaling distributes work across more machines or nodes. |
| 1.5 | B | `reduceByKey()` aggregates values and can combine them locally before shuffling. |
| 1.6 | C | `collect()` transfers the full result to the driver. |
| 1.7 | D | `distinct()` typically requires coordination across partitions. |
| 1.8 | B | DataFrames are structured, distributed tables with named columns and a schema. |
| 1.9 | B | A DStream is a sequence of RDDs associated with time-based batches. |
| 1.10 | B | CAP concerns the inability to guarantee both strict consistency and availability during a partition. |

**Marking:** 1 mark per correct answer; 0 for an incorrect or missing answer.

## Section B — Model answers (20 marks)

### Question 1.11 — Spark architecture (5 marks)

- **Driver:** Runs the application, constructs execution plans, schedules tasks and coordinates execution.
- **Cluster manager:** Allocates cluster resources to applications.
- **Executor:** A process that executes tasks and may cache intermediate data on worker resources.
- **Partition:** A logical subdivision of distributed data that can be processed as a task unit.
- **SparkSession:** The modern entry point commonly used for DataFrames and Spark SQL.

**Marking:** 1 mark per correct role.

**Common trap:** A worker is a machine providing resources; an executor is a process running on a worker. They are not interchangeable terms.

### Question 1.12 — RDD properties and lineage (5 marks)

An RDD is an immutable, distributed collection of elements that Spark can process in parallel.

- **Immutability:** Transformations create new RDDs rather than modifying existing ones.
- **Lazy evaluation:** Transformations normally describe computations without immediately executing them.
- **Lineage:** Spark records dependencies that describe how an RDD was derived from earlier data.
- **Fault tolerance:** If a partition is lost, Spark can often recompute it from source data and its dependencies.

Lineage helps recovery because Spark can rerun the necessary transformations for a missing partition instead of requiring a complete copy of every intermediate RDD.

**Marking:**

- Definition: 1 mark.
- Immutability: 1 mark.
- Lazy evaluation: 1 mark.
- Lineage: 1 mark.
- Recovery explanation: 1 mark.

**Common trap:** Lineage does not guarantee recovery if the source data and necessary dependencies are no longer available.

### Question 1.13 — Narrow and wide transformations (5 marks)

| Operation | Typical classification |
| --- | --- |
| `map()` | Narrow |
| `filter()` | Narrow |
| `distinct()` | Wide |
| `reduceByKey()` | Wide overall |
| `sortByKey()` | Wide overall |

Wide transformations may require a shuffle: data is redistributed between partitions, often over the network. This can require serialization, memory, disk I/O and additional execution stages.

**Marking:**

- 0.5 marks for each correct classification: 2.5 marks.
- Explanation of shuffle and its costs: 2.5 marks.

**Common trap:** `reduceByKey()` can perform local combining, but the operation may still require a shuffle to bring matching keys together.

### Question 1.14 — Distributed processing (5 marks)

Spark divides data into partitions and distributes computation across a cluster. This can allow a workload to use the combined resources of several machines when a single computer lacks sufficient memory, storage or processing capacity.

- **Vertical scaling:** Increasing resources such as CPU or RAM on one machine.
- **Horizontal scaling:** Adding machines or distributing processing across nodes.
- **Limitation:** Distributed processing introduces network communication, coordination, serialization and potentially expensive shuffles.

**Marking:**

- Reason Spark is suitable: 2 marks.
- Correct scaling distinction: 2 marks.
- Limitation: 1 mark.

**Common trap:** Spark does not automatically make every workload faster. Small tasks may run faster on one machine because they avoid distributed overhead.

## Section C — Model answers (25 marks)

### Question 1.15 — Trace transformations and actions (7 marks)

Given:

```
rdd = sc.parallelize([1, 2, 3, 4, 5, 6], 3)

filtered = rdd.filter(lambda x: x % 2 == 0)
result = filtered.map(lambda x: x * x)
```

**1\. Content of****`filtered`****(2 marks)**

The even elements are:

```
[2, 4, 6]
```

**2\. Result of****`take(2)`****(2 marks)**

The squared elements are `4`, `16` and `36`.

Therefore, `take(2)` returns two elements, normally:

```
[4, 16]
```

**3\. Result of****`count()`****(1 mark)**

```
3
```

There are three elements in the resulting RDD.

**4\. Execution behaviour (2 marks)**

The assignments define transformations. They do not normally execute the entire computation immediately. `take(2)` and `count()` trigger the computations required for their respective results.

The second action may reuse cached data if caching has been configured and the relevant data is available. Without caching, Spark may recompute upstream transformations.

**Common trap:** Lazy evaluation does not mean the result is never computed. It means execution is deferred until required.

### Question 1.16 — Character counting (5 marks)

```
lines = sc.textFile("data/book.txt", 4)

total_characters = lines.map(len).sum()

print(total_characters)
```

Equivalent solution:

```
total_characters = lines.map(
    lambda line: len(line)
).reduce(
    lambda a, b: a + b
)
```

Explanation:

1. `textFile()` creates an RDD whose elements are lines.
2. `map(len)` calculates the number of characters in each line.
3. `sum()` or `reduce()` combines the line counts into one total.

**Marking:**

- Read the file with four requested partitions: 1 mark.
- Apply `map(len)`: 2 marks.
- Aggregate to one total: 1 mark.
- Correct explanation: 1 mark.

**Important qualification:** This counts characters in the line strings returned by `textFile()`. Line terminators are generally excluded. If the problem requires counting newline separators, they must be accounted for separately.

### Question 1.17 — Word counting (7 marks)

```
lines = sc.textFile("data/book.txt", 4)

word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

Explanation:

- `flatMap()` splits each line into tokens and flattens the token collections.
- `map()` changes each token into a key-value pair with an initial count of one.
- `reduceByKey()` adds values belonging to the same key.

The output contains pairs such as:

```
[("spark", 4), ("data", 7)]
```

These numbers are illustrative only; actual counts depend on the file contents.

**Marking:**

- Read the file: 1 mark.
- Correct `flatMap()` tokenisation: 2 marks.
- Correct `(word, 1)` mapping: 2 marks.
- Correct `reduceByKey()` aggregation: 2 marks.

**Common trap:** `split()` does not remove punctuation or normalise case. For example, `Spark`, `spark` and `spark,` are potentially different keys.

### Question 1.18 — Top three words (6 marks)

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)

print(top_three)
```

- `pair[0]` is the word.
- `pair[1]` is the count.
- The negative count orders larger frequencies first.
- The word provides alphabetical tie-breaking.

**Marking:**

- Correct `takeOrdered(3, ...)`: 3 marks.
- Correct alphabetical tie-breaker: 1 mark.
- Explanation of the negative count: 2 marks.

**Common trap:** `takeOrdered(3)` alone returns the three smallest elements according to the default ordering, not the three most frequent words.

## Section D — Model answers (20 marks)

### Question 1.19 — Calculated column (5 marks)

```
from pyspark.sql.functions import col

result = df.select(
    "customer",
    "product",
    (col("quantity") * col("price")).alias("total")
)

result.show()
```

Expected totals:

| customer | product | total |
| --- | --- | --- |
| 200 | 5 | 264 |
| 200 | 8 | 555 |
| 100 | 8 | 225 |
| 100 | 8 | 295 |

**Marking:**

- Correct selection: 1 mark.
- Correct multiplication: 2 marks.
- Correct alias: 1 mark.
- Correct calculated values: 1 mark.

### Question 1.20 — Filtering and sorting (5 marks)

```
result = (
    df
    .where(col("price") > 50)
    .orderBy(col("price").desc())
)

result.show()
```

Expected rows:

| customer | product | quantity | price |
| --- | --- | --- | --- |
| 200 | 8 | 5 | 111 |
| 100 | 8 | 5 | 59 |

The row with price 33 and the row with price 45 are excluded.

**Marking:**

- Correct filter: 2 marks.
- Correct descending sort: 2 marks.
- Correct result: 1 mark.

### Question 1.21 — Aggregation (6 marks)

```
from pyspark.sql.functions import sum

sales_by_customer = df.groupBy("customer").agg(
    sum(col("quantity") * col("price")).alias("total_sales")
)

sales_by_customer.show()
```

Customer 200:

$$
8 \times 33 + 5 \times 111 = 264 + 555 = 819
$$

Customer 100:

$$
5 \times 45 + 5 \times 59 = 225 + 295 = 520
$$

Expected result:

| customer | total\_sales |
| --- | --- |
| 100 | 520 |
| 200 | 819 |

The order of the grouped output is not guaranteed unless explicitly sorted.

**Marking:**

- Correct grouping: 1 mark.
- Correct expression: 2 marks.
- Correct aggregation and alias: 1 mark.
- Customer 100 total: 1 mark.
- Customer 200 total: 1 mark.

### Question 1.22 — `select()` versus `selectExpr()` (4 marks)

Using Spark column expressions:

```
df.select(
    "customer",
    (col("quantity") * col("price")).alias("total")
)
```

Using SQL expression strings:

```
df.selectExpr(
    "customer",
    "quantity * price AS total"
)
```

`select()` accepts column names and Spark column expressions. `selectExpr()` accepts SQL expression strings. Both return a new DataFrame.

**Marking:**

- Difference explained: 2 marks.
- Correct `select()` example: 1 mark.
- Correct `selectExpr()` example: 1 mark.

## Section E — Model answers (25 marks)

### Question 1.23 — Performance diagnosis (7 marks)

**Concern 1:****`groupByKey()`****gathers all values for each key.**

Improvement:

```
counts = pairs.reduceByKey(lambda a, b: a + b)
```

`reduceByKey()` can combine values locally before the shuffle, reducing the amount of data transferred when only counts are required.

**Concern 2:****`collect()`****transfers every result to the driver.**

Improvement depends on the objective:

```
counts.take(10)
```

for inspection, or:

```
counts.saveAsTextFile("output/word-counts")
```

for distributed output.

If the complete result is genuinely small, `collect()` may be acceptable. For a large result, transferring everything to the driver risks memory exhaustion.

**Concern 3: The program may process more data than necessary.**

Improvement: filter unnecessary records early, where the task permits it, and avoid redundant transformations or shuffles. Filtering early can reduce downstream work and network traffic.

**Marking:**

- First valid concern and improvement: up to 2 marks.
- Second valid concern and improvement: up to 2 marks.
- Third valid concern and improvement: up to 2 marks.
- Clear explanation of why the changes help: 1 mark.

Equivalent answers discussing appropriate partitioning or other justified improvements can receive credit.

**Common trap:** `flatMap()` and `map()` are normally narrow transformations and are not, by themselves, the main shuffle problem in this example.

### Question 1.24 — Caching (5 marks)

1. **Why cache?** If a costly filtered RDD is reused by several actions, caching can avoid recomputing its upstream transformations when the cached partitions are available. (2 marks)
2. **Why not immediate?** `.cache()` marks the RDD for caching; execution is normally triggered by an action. (1 mark)
3. **Two limitations:** (2 marks)
   - Cached data consumes memory and potentially disk.
   - Cache capacity is finite, so partitions may be evicted or recomputed.
   - Caching a dataset used only once may add overhead without meaningful benefit.

Award up to 1 mark for each distinct, correct limitation.

### Question 1.25 — Classic Spark Streaming (5 marks)

Classic Spark Streaming groups incoming data into time-based micro-batches.

- `StreamingContext` configures the streaming application and its batch interval.
- Each batch is represented as an RDD within a DStream.
- Transformations process the data in each batch.
- Output operations expose or store results.
- The batch interval controls how frequently batches are formed; shorter intervals may reduce waiting time but can increase scheduling overhead.

**Marking:**

- DStream definition: 1 mark.
- RDD relationship: 1 mark.
- `StreamingContext` role: 1 mark.
- Batch interval: 1 mark.
- Latency or scheduling trade-off: 1 mark.

### Question 1.26 — NoSQL and consistency (8 marks)

**1\. Four models (4 marks)**

| Model | Example |
| --- | --- |
| Document | MongoDB |
| Key-value | Redis |
| Column-family | Apache Cassandra |
| Graph | Neo4j |

Other suitable examples receive credit.

**2\. ACID (2 marks)**

- Atomicity
- Consistency
- Isolation
- Durability

Award 0.5 marks for each correct term.

**3\. BASE versus ACID (1 mark)**

ACID describes transactional guarantees, while BASE is a conceptual approach associated with systems that favour availability and eventual consistency in some distributed workloads.

**4\. CAP (1 mark)**

During a network partition, a distributed system cannot guarantee both strict consistency and availability simultaneously.

**Common trap:** CAP does not mean that a database must always choose availability over consistency. The trade-off is specifically about guarantees during a network partition.

---

# Practice Exam 2 — DataFrames and Performance

**Duration:** 120 minutes\
**Maximum score:** 100 marks

## Section A — Multiple-choice questions (10 marks)

Choose one answer per question. Each question is worth 1 mark.

### Question 2.1

Which expression calculates a new DataFrame column?

A. `df.collect("quantity" * "price")`\
B. `df.select((col("quantity") * col("price")).alias("total"))`\
C. `df.map("quantity" * "price")`\
D. `df.count().alias("total")`

### Question 2.2

Which method filters rows in a DataFrame?

A. `where()`\
B. `reduceByKey()`\
C. `parallelize()`\
D. `StreamingContext()`

### Question 2.3

What is the correct way to combine two Spark column conditions with logical AND?

A. `col("a") > 0 and col("b") > 0`\
B. `col("a") > 0 && col("b") > 0`\
C. `(col("a") > 0) & (col("b") > 0)`\
D. `(col("a") > 0) AND (col("b") > 0)` in ordinary Python code

### Question 2.4

What does `groupBy("customer")` do?

A. Immediately returns one Python dictionary.\
B. Defines the grouping columns for subsequent aggregation.\
C. Sorts the DataFrame by customer.\
D. Removes duplicate customers without aggregation.

### Question 2.5

Which action is suitable for inspecting a small number of RDD elements?

A. `collect()` on the entire dataset\
B. `take(10)`\
C. `groupByKey()`\
D. `distinct()`

### Question 2.6

Why might caching improve performance?

A. It permanently guarantees that every partition remains in memory.\
B. It can avoid repeated computation of a reused dataset.\
C. It removes all shuffles from the execution plan.\
D. It forces transformations to execute immediately.

### Question 2.7

Which statement about `union()` is correct?

A. It automatically sorts all elements.\
B. It automatically removes duplicates.\
C. It combines RDD contents without inherently deduplicating them.\
D. It is an action.

### Question 2.8

What is a major advantage of DataFrames for structured workloads?

A. They cannot spill to disk.\
B. They eliminate the need for partitioning.\
C. They support structured query planning and optimisation.\
D. They always execute on the driver.

### Question 2.9

Which statement about `reduceByKey()` is correct?

A. It returns a Python integer in all cases.\
B. It aggregates values associated with matching keys.\
C. It can only be used on DataFrames.\
D. It always avoids all network communication.

### Question 2.10

Which statement about classic and Structured Streaming is correct?

A. Both use DStreams as their only abstraction.\
B. Structured Streaming uses structured DataFrames/Datasets and Spark SQL capabilities.\
C. Structured Streaming always runs in continuous-processing mode.\
D. Classic Spark Streaming cannot process data in batches.

---

## Section B — Conceptual short answers (20 marks)

### Question 2.11 — DataFrames versus RDDs (5 marks)

Compare RDDs and DataFrames in terms of:

- Data structure.
- Schema.
- Programming style.
- Optimisation.
- Suitable use case.

### Question 2.12 — Lazy evaluation and DAGs (5 marks)

Consider:

```
a = sc.textFile("data/logs.txt")
b = a.filter(lambda line: "ERROR" in line)
c = b.map(lambda line: line.upper())
```

Explain what happens before an action is called, what happens when an action is called, and how the computation can be represented as a DAG.

### Question 2.13 — Performance guidelines (5 marks)

Explain the rationale behind any three of the following recommendations:

- Prefer `reduceByKey()` to `groupByKey()` for aggregation.
- Cache reused datasets.
- Avoid unnecessary `collect()`.
- Minimise shuffles.
- Choose appropriate partition counts.

### Question 2.14 — Sorting and aggregation costs (5 marks)

Explain why a global distributed sort can be expensive. Also explain why a `groupBy()` followed by aggregation may require data redistribution.

---

## Section C — RDD programming and interpretation (25 marks)

### Question 2.15 — Code interpretation (6 marks)

Consider:

```
rdd = sc.parallelize([3, 1, 3, 2, 2, 5], 3)

unique = rdd.distinct()

print(unique.count())
print(unique.takeOrdered(3))
```

Answer:

1. What does `unique.count()` return? (2 marks)
2. What does `unique.takeOrdered(3)` return? (2 marks)
3. Which operation typically involves a shuffle? Why? (2 marks)

### Question 2.16 — Improved word count (7 marks)

Write a word-count program with the following rules:

- Read `data/book.txt`.
- Convert text to lowercase.
- Extract word-like tokens using `re.findall(r"\b\w+\b", line.lower())`.
- Count token occurrences with `reduceByKey()`.

Explain why the result can differ from a program that uses `line.split()`.

### Question 2.17 — Aggregation choice (6 marks)

Given:

```
records = sc.parallelize([
    ("A", 1),
    ("A", 1),
    ("B", 1),
    ("A", 1),
    ("B", 1)
])
```

1. Write code to count records per key using `reduceByKey()`. (3 marks)
2. State the expected result, ignoring output order. (1 mark)
3. Explain why `reduceByKey()` is generally preferable to `groupByKey()` for this task. (2 marks)

### Question 2.18 — Debugging `takeOrdered()` (6 marks)

A student writes:

```
word_counts = sc.parallelize([
    ("spark", 20),
    ("data", 50),
    ("python", 30),
    ("sql", 40)
])

top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: pair[1]
)
```

1. Explain the error in the student's reasoning. (2 marks)
2. Correct the code to return the three highest counts, breaking ties alphabetically. (3 marks)
3. State the expected result. (1 mark)

---

## Section D — DataFrames and query interpretation (20 marks)

Use the following DataFrame:

| customer | product | quantity | price |
| --- | --- | --- | --- |
| 1 | 10 | 2 | 5 |
| 1 | 11 | 3 | 7 |
| 2 | 10 | 4 | 5 |
| 2 | 12 | 1 | 20 |
| 3 | 11 | 5 | 7 |

### Question 2.19 — Filtering and calculated columns (6 marks)

Write a DataFrame query that:

1. Keeps rows where `quantity >= 3` and `price < 10`.
2. Selects `customer`, `quantity`, `price`.
3. Adds `total = quantity * price`.
4. Sorts by `total` in descending order.

State the expected rows.

### Question 2.20 — Grouped aggregation (7 marks)

For each customer, calculate:

- Total quantity.
- Total sales value.
- Average price.

Write PySpark code and calculate the expected values.

### Question 2.21 — JSON reading and schema inspection (3 marks)

Write code to read `data/orders.json` into a DataFrame, print its schema and display five rows.

Explain what `printSchema()` reveals.

### Question 2.22 — DataFrame boolean expressions (4 marks)

A student writes:

```
df.where(
    col("quantity") > 0 and col("price") > 10
)
```

Explain why this is incorrect and provide the correct expression.

---

## Section E — Performance, streaming and NoSQL (25 marks)

### Question 2.23 — Performance case study (8 marks)

A Spark application performs the following operations:

1. Reads a large text dataset.
2. Filters for records matching a particular event type.
3. Runs an expensive transformation.
4. Uses the resulting dataset in five actions.
5. Calls `collect()` on the complete final result.

Propose four improvements or design decisions. Explain the benefit and any relevant trade-off for each.

### Question 2.24 — Partitions and parallelism (5 marks)

A cluster has four worker machines. An RDD has two partitions.

1. Explain why the number of workers and partitions need not match. (2 marks)
2. Describe one potential disadvantage of having too few partitions. (1 mark)
3. Describe one potential disadvantage of having too many tiny partitions. (1 mark)
4. State one factor that should influence partition selection. (1 mark)

### Question 2.25 — Streaming design (6 marks)

A system receives website click events continuously.

Explain how classic Spark Streaming could process the events, and compare it with Structured Streaming. Include one potential issue caused by choosing an excessively short batch interval.

### Question 2.26 — NoSQL selection (6 marks)

Choose a suitable NoSQL model for each scenario and justify your choices:

1. A service stores user sessions indexed by session ID.
2. A product catalogue contains records with different optional attributes.
3. A social network analyses relationships between users.
4. Explain why the same database model need not be ideal for all three workloads. (3 marks total for the three selections and their justifications; 1 mark for the final explanation.)

Then explain the conceptual distinction between ACID and BASE and name one CAP trade-off. (2 marks)

---

# Solutions to Exam 2

## Section A — Multiple-choice answer key (10 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| 2.1 | B | Spark column expressions calculate a new column. |
| 2.2 | A | `where()` filters DataFrame rows. |
| 2.3 | C | Spark column conditions are combined using `&` with parenthesised comparisons. |
| 2.4 | B | `groupBy()` defines the grouping operation. |
| 2.5 | B | `take(10)` returns up to ten elements to the driver. |
| 2.6 | B | Caching can avoid recomputation when data is reused. |
| 2.7 | C | `union()` does not inherently remove duplicates. |
| 2.8 | C | DataFrames support structured query optimisation. |
| 2.9 | B | `reduceByKey()` combines values for matching keys. |
| 2.10 | B | Structured Streaming uses DataFrames/Datasets and Spark SQL. |

## Section B — Model answers (20 marks)

### Question 2.11 — DataFrames versus RDDs (5 marks)

| Aspect | RDD | DataFrame |
| --- | --- | --- |
| Data structure | Distributed collection of elements | Distributed table |
| Schema | No required tabular schema | Named columns and associated types |
| Programming style | Functional, element-oriented | Declarative, SQL-like |
| Optimisation | Less automatic relational optimisation | Spark SQL query optimisation |
| Suitable use | Custom element-level processing | Structured filtering, joins and aggregation |

**Marking:** 1 mark per correctly explained comparison.

**Common trap:** RDDs are not necessarily unstructured in the ordinary sense; their elements can be tuples or objects. The distinction is that RDDs do not require a relational schema.

### Question 2.12 — Lazy evaluation and DAGs (5 marks)

Before an action, the assignments define the transformations and their dependencies. Spark does not normally execute the entire data pipeline immediately.

When an action requests a result, Spark schedules the required work, divides execution into stages and tasks, and runs the tasks across partitions.

Conceptual DAG:

```
Input text RDD
      |
    filter()
      |
     map()
      |
    action
```

The DAG is a directed acyclic graph because the operations form dependency relationships without directed cycles. A shuffle dependency, if introduced by a wide transformation, commonly separates stages.

**Marking:**

- Lazy transformations: 2 marks.
- Action and execution: 1 mark.
- Correct DAG representation: 1 mark.
- Stage or task explanation: 1 mark.

### Question 2.13 — Performance guidelines (5 marks)

Award credit for any three recommendations with correct reasoning.

Examples:

- **Prefer****`reduceByKey()`****:** Local combining can reduce shuffle traffic when only an aggregate is needed.
- **Cache reused datasets:** Avoids recomputation when the same expensive intermediate data is needed repeatedly, at the cost of storage.
- **Avoid large****`collect()`****:** Prevents transferring an enormous result to the driver and potentially exhausting its memory.
- **Minimise shuffles:** Reduces network transfer, serialization, disk I/O and execution overhead.
- **Choose suitable partitions:** Balances parallelism against scheduling overhead and task size.

**Marking:** Up to 1.5 marks per recommendation, with 0.5 marks reserved for a coherent explanation across the three responses.

### Question 2.14 — Sorting and aggregation costs (5 marks)

A global distributed sort requires data to be ordered across partition boundaries, which commonly involves repartitioning, communication and shuffle work. The operation can consume memory and disk resources and may introduce additional execution stages.

A grouped aggregation must combine records belonging to the same key. If matching keys are distributed across different partitions, Spark commonly redistributes data so that their values can be aggregated.

**Marking:**

- Sorting explanation: 2 marks.
- Grouped aggregation explanation: 2 marks.
- Explicit mention of shuffle or communication overhead: 1 mark.

**Common trap:** Not every aggregation requires the same amount of network transfer. Partial aggregation, partitioning and execution strategies affect the actual cost.

## Section C — Model answers (25 marks)

### Question 2.15 — Code interpretation (6 marks)

```
rdd = sc.parallelize([3, 1, 3, 2, 2, 5], 3)

unique = rdd.distinct()
```

**1\.****`unique.count()`****(2 marks)**

There are four unique values: `1`, `2`, `3` and `5`.

Answer:

```
4
```

**2\.****`unique.takeOrdered(3)`****(2 marks)**

The three smallest values are:

```
[1, 2, 3]
```

**3\. Shuffle (2 marks)**

`distinct()` typically requires a shuffle because Spark must identify duplicate values that may exist in different partitions. `count()` is an action, but it does not itself necessarily require a shuffle of all values.

### Question 2.16 — Improved word count (7 marks)

```
import re

lines = sc.textFile("data/book.txt")

word_counts = (
    lines
    .flatMap(
        lambda line: re.findall(
            r"\b\w+\b",
            line.lower()
        )
    )
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

Explanation:

- `lower()` normalises case.
- `re.findall()` extracts word-like tokens rather than simply splitting on whitespace.
- `flatMap()` emits each token as an individual RDD element.
- `map()` assigns each token a count of one.
- `reduceByKey()` adds the counts.

The result can differ from a `split()` implementation because `split()` uses whitespace boundaries and preserves punctuation attached to tokens. This regular-expression approach also normalises case. The precise tokenisation depends on the regular expression and character definitions.

**Marking:**

- Imports and input: 1 mark.
- Correct lowercase and token extraction: 2 marks.
- Correct `flatMap()`: 1 mark.
- Correct pair mapping: 1 mark.
- Correct aggregation: 1 mark.
- Explanation of differing results: 1 mark.

### Question 2.17 — Aggregation choice (6 marks)

```
records = sc.parallelize([
    ("A", 1),
    ("A", 1),
    ("B", 1),
    ("A", 1),
    ("B", 1)
])

counts = records.reduceByKey(
    lambda a, b: a + b
)

print(sorted(counts.collect()))
```

Expected result:

```
[("A", 3), ("B", 2)]
```

`reduceByKey()` is generally preferable because the task needs a single aggregate per key, not every original value. Local combining can reduce the amount of data shuffled across the cluster.

**Marking:**

- Correct code: 3 marks.
- Correct counts: 1 mark.
- Performance explanation: 2 marks.

### Question 2.18 — Debugging `takeOrdered()` (6 marks)

The student's key orders values by ascending count. `takeOrdered(3)` therefore selects the three smallest counts rather than the three largest.

Correct solution:

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

Expected result:

```
[
    ("data", 50),
    ("sql", 40),
    ("python", 30)
]
```

The negative count makes larger frequencies sort first. The word is the secondary ordering key.

**Marking:**

- Identify ascending-order error: 2 marks.
- Correct descending count and alphabetical tie-breaker: 3 marks.
- Correct output: 1 mark.

## Section D — Model answers (20 marks)

### Question 2.19 — Filtering and calculated columns (6 marks)

```
from pyspark.sql.functions import col

result = (
    df
    .where(
        (col("quantity") >= 3)
        & (col("price") < 10)
    )
    .select(
        "customer",
        "quantity",
        "price",
        (col("quantity") * col("price")).alias("total")
    )
    .orderBy(col("total").desc())
)

result.show()
```

Rows satisfying both conditions:

| customer | quantity | price | total |
| --- | --- | --- | --- |
| 3 | 5 | 7 | 35 |
| 2 | 4 | 5 | 20 |
| 1 | 3 | 7 | 21 |

After sorting by descending total, the correct order is:

| customer | quantity | price | total |
| --- | --- | --- | --- |
| 3 | 5 | 7 | 35 |
| 1 | 3 | 7 | 21 |
| 2 | 4 | 5 | 20 |

**Marking:**

- Correct filter and parentheses: 2 marks.
- Correct column selection and calculation: 2 marks.
- Correct descending sort: 1 mark.
- Correct result: 1 mark.

### Question 2.20 — Grouped aggregation (7 marks)

```
from pyspark.sql.functions import sum, avg, col

summary = df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity"),
    sum(col("quantity") * col("price")).alias("total_sales"),
    avg("price").alias("average_price")
)

summary.show()
```

Calculations:

**Customer 1**

- Total quantity: $2 + 3 = 5$
- Total sales: $2 \times 5 + 3 \times 7 = 31$
- Average price: $(5 + 7)/2 = 6$

**Customer 2**

- Total quantity: $4 + 1 = 5$
- Total sales: $4 \times 5 + 1 \times 20 = 40$
- Average price: $(5 + 20)/2 = 12.5$

**Customer 3**

- Total quantity: $5$
- Total sales: $5 \times 7 = 35$
- Average price: $7$

Expected result:

| customer | total\_quantity | total\_sales | average\_price |
| --- | --- | --- | --- |
| 1 | 5 | 31 | 6 |
| 2 | 5 | 40 | 12.5 |
| 3 | 5 | 35 | 7 |

**Marking:**

- Correct grouping: 1 mark.
- Total quantity: 2 marks.
- Total sales: 2 marks.
- Average price: 1 mark.
- Correct aliases and presentation: 1 mark.

**Common trap:** `avg("price")` calculates the average of the row-level prices. It does not calculate a quantity-weighted average.

### Question 2.21 — JSON reading (3 marks)

```
df = spark.read.format("json").load("data/orders.json")

df.printSchema()
df.show(5)
```

`printSchema()` reveals the DataFrame's column names, data types and nested structure.

**Marking:**

- Correct JSON read: 1 mark.
- Schema inspection: 1 mark.
- Display five rows: 1 mark.

### Question 2.22 — Boolean expressions (4 marks)

Python's `and` attempts to evaluate the truth value of each operand. Spark column expressions represent distributed computations and cannot be combined using ordinary Python boolean evaluation.

Correct expression:

```
df.where(
    (col("quantity") > 0)
    & (col("price") > 10)
)
```

Use `&` for logical AND and parenthesise each comparison.

**Marking:**

- Explanation of the error: 2 marks.
- Correct use of `&`: 1 mark.
- Correct parentheses: 1 mark.

## Section E — Model answers (25 marks)

### Question 2.23 — Performance case study (8 marks)

Any four justified improvements can receive credit.

**1\. Filter early.**

Apply the event-type filter before the expensive transformation when logically valid. This reduces the volume of records processed downstream.

**2\. Cache the expensive intermediate dataset.**

If all five actions use the same result, caching may avoid recomputing the costly upstream transformation. The trade-off is memory or disk consumption and possible cache eviction.

**3\. Avoid unnecessary full collection.**

If the purpose is inspection, use `take(n)`. If the purpose is distributed storage, use `saveAsTextFile()` or an appropriate distributed output sink. Collect the full result only if it is known to be small enough for the driver.

**4\. Minimise unnecessary wide transformations.**

Review the pipeline for redundant `distinct()`, grouping, joins or sorting. Avoiding unnecessary shuffles can reduce network and intermediate-data overhead.

Other acceptable recommendations include choosing appropriate partitions and preferring DataFrame operations for structured processing.

**Marking:** 2 marks per improvement: 1 for identifying the decision and 1 for explaining its benefit or trade-off.

### Question 2.24 — Partitions and parallelism (5 marks)

1. Partitions are logical units of data processing; workers are machines providing resources. A worker can process multiple partitions, so their counts need not match. (2 marks)
2. Too few partitions can limit parallelism and create oversized tasks. (1 mark)
3. Too many tiny partitions can increase scheduling overhead and produce many small output files. (1 mark)
4. Suitable factors include dataset size, available resources, task cost, skew and the operation being performed. (1 mark)

**Common trap:** Four workers do not imply that exactly four partitions are optimal.

### Question 2.25 — Streaming design (6 marks)

Classic Spark Streaming can process click events by collecting incoming data into micro-batches. A DStream represents a sequence of RDDs, and each batch can be processed using transformations such as `flatMap()`, `map()` and `reduceByKey()`.

`StreamingContext` configures the classic streaming application, including its batch interval.

Structured Streaming instead uses structured DataFrames/Datasets and Spark SQL's execution framework. It supports a more declarative interface and integrates with structured batch-processing concepts.

An excessively short batch interval can increase scheduling overhead and make it harder for the application to keep up with incoming data. Actual latency and throughput depend on the workload, resources, source and sink.

**Marking:**

- Classic micro-batch explanation: 2 marks.
- DStream/`StreamingContext` explanation: 1 mark.
- Structured Streaming comparison: 2 marks.
- Batch-interval trade-off: 1 mark.

### Question 2.26 — NoSQL selection (6 marks)

**1\. User sessions — key-value model**

A key-value store such as Redis is suitable when sessions are accessed primarily by session ID. Redis also supports additional data structures.

**2\. Product catalogue — document model**

A document database such as MongoDB is suitable for products whose attributes vary between records.

**3\. Social network relationships — graph model**

A graph database such as Neo4j represents users as nodes and relationships as edges, making relationship-oriented queries natural.

**4\. Why no universal model?**

Different workloads have different access patterns, relationship structures, consistency requirements and scaling needs. A model suitable for session lookup may not be the best for graph traversal.

**5\. ACID, BASE and CAP**

ACID describes transactional properties: Atomicity, Consistency, Isolation and Durability. BASE is a conceptual approach associated with Basically Available, Soft state and Eventual consistency. CAP describes the inability to guarantee both strict consistency and availability during a network partition.

**Marking:**

- Three suitable models and justifications: 3 marks.
- Explanation of workload-dependent choice: 1 mark.
- ACID/BASE distinction: 1 mark.
- CAP trade-off: 1 mark.

---

# Practice Exam 3 — Integrated Spark Applications

**Duration:** 120 minutes\
**Maximum score:** 100 marks

This exam combines conceptual understanding, code tracing, debugging, distributed execution, structured analytics, streaming and introductory NoSQL.

## Section A — Multiple-choice questions (10 marks)

Choose one answer per question. Each question is worth 1 mark.

### Question 3.1

Which statement best describes Spark lineage?

A. A permanent backup of every intermediate dataset.\
B. A record of dependencies that can help recompute lost partitions.\
C. A table of all values collected by the driver.\
D. A guarantee that no data can ever be lost.

### Question 3.2

What does `flatMap()` do?

A. Always produces exactly one output element per input.\
B. Produces zero or more outputs per input and flattens the results.\
C. Sorts every output partition.\
D. Aggregates matching keys.

### Question 3.3

What is the purpose of `alias("total")` in a DataFrame expression?

A. Cache the column permanently.\
B. Rename the calculated column in the resulting DataFrame.\
C. Sort the DataFrame by total.\
D. Convert the DataFrame into an RDD.

### Question 3.4

Which operation returns the number of elements in an RDD?

A. `map()`\
B. `filter()`\
C. `count()`\
D. `distinct()`

### Question 3.5

Which operation normally removes duplicate RDD elements?

A. `union()`\
B. `distinct()`\
C. `first()`\
D. `saveAsTextFile()`

### Question 3.6

Which is the best default choice for a large structured aggregation?

A. Convert the whole dataset into a Python list.\
B. Use a DataFrame with an appropriate `groupBy()` and aggregation.\
C. Call `collect()` before aggregating locally.\
D. Cache every intermediate dataset regardless of reuse.

### Question 3.7

What does `saveAsTextFile()` generally do?

A. Returns all rows as a Python list.\
B. Writes distributed text output, usually into multiple part files.\
C. Sorts the data globally.\
D. Converts every partition into one driver-side string.

### Question 3.8

Which is true about Spark's performance relative to Hadoop MapReduce?

A. Spark is guaranteed to be 100 times faster for every workload.\
B. Spark can reduce intermediate disk I/O in suitable workloads, but actual performance depends on configuration and workload.\
C. Spark never uses disk.\
D. Hadoop MapReduce cannot process large datasets.

### Question 3.9

Which component executes Spark tasks?

A. Executor\
B. Schema\
C. `SparkSession` alone\
D. DStream

### Question 3.10

Which statement about Redis is correct?

A. It is exclusively a graph database.\
B. It is an in-memory key-value data store that supports additional data structures.\
C. It is Spark's cluster manager.\
D. It is the name of a Spark transformation.

---

## Section B — Conceptual short answers (20 marks)

### Question 3.11 — Spark execution lifecycle (5 marks)

Describe the sequence from defining transformations to obtaining a result. Include:

- The DAG.
- An action.
- Stages and tasks.
- Executors.
- The driver.

### Question 3.12 — Fault tolerance and caching (5 marks)

Explain the difference between lineage-based recovery and caching. Why does caching not eliminate the need for lineage?

### Question 3.13 — Word-count design (5 marks)

Explain the purpose of each operation in this pattern:

```
counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

Identify one limitation of this implementation and explain how you could address it.

### Question 3.14 — Choosing an API (5 marks)

A team must implement:

1. A structured sales report.
2. A custom algorithm that processes arbitrary Python objects.
3. A classic Spark Streaming application that must be understood for an existing legacy system.

Recommend an appropriate API or abstraction for each, with a short justification.

---

## Section C — RDD programming and debugging (25 marks)

### Question 3.15 — Debug a word-count pipeline (7 marks)

A student writes:

```
lines = sc.textFile("data/book.txt")

words = lines.map(
    lambda line: line.split()
)

counts = words.map(
    lambda word: (word, 1)
).reduceByKey(
    lambda a, b: a + b
)
```

The student expects one count per individual word.

1. Identify the problem. (2 marks)
2. Correct the code. (3 marks)
3. Explain why the corrected version works. (2 marks)

### Question 3.16 — Character count with line separators (5 marks)

Write code to count:

- Every character in every line returned by `textFile()`.
- One additional character for each line, as a simplified model of counting one newline separator per line.

State the formula used and explain its assumption about line endings.

### Question 3.17 — Combining RDDs and actions (6 marks)

Given:

```
a = sc.parallelize([1, 2, 2])
b = sc.parallelize([2, 3])

combined = a.union(b)
unique = combined.distinct()
```

Answer:

1. What is the element count of `combined`? (1 mark)
2. What is the element count of `unique`? (1 mark)
3. Explain why `union()` does not produce the same result as `distinct()`. (2 marks)
4. Which operation typically requires a shuffle, and why? (2 marks)

### Question 3.18 — Correct aggregation and top-k (7 marks)

Given a large RDD of `(category, value)` pairs, the objective is to calculate the total value for each category and then find the three categories with the largest totals.

Write a suitable PySpark pipeline. Explain why your aggregation choice is appropriate and how you would implement deterministic alphabetical tie-breaking.

---

## Section D — DataFrames and practical analysis (20 marks)

Use the following table:

| customer | product | quantity | price |
| --- | --- | --- | --- |
| 10 | 1 | 2 | 100 |
| 10 | 2 | 1 | 50 |
| 20 | 1 | 3 | 100 |
| 20 | 3 | 2 | 25 |
| 30 | 2 | 4 | 50 |
| 30 | 3 | 1 | 25 |

### Question 3.19 — Sales analysis (8 marks)

Write a DataFrame query that:

1. Adds `total = quantity * price`.
2. Keeps rows with `total >= 100`.
3. Groups by customer.
4. Calculates total sales and total quantity.
5. Sorts customers by total sales descending.

Calculate the expected result.

### Question 3.20 — Multiple aggregations (6 marks)

For each customer, calculate:

- Minimum price.
- Maximum price.
- Average price.
- Number of rows.

Write the PySpark query and calculate the expected results.

### Question 3.21 — Debug a DataFrame query (6 marks)

A student writes:

```
result = df.selectExpr(
    "customer",
    "quantity * price AS total"
).where(
    col("total") > 100 and col("customer") == 10
).orderBy(
    col("total").desc()
)
```

Identify two problems or risks and correct the code.

---

## Section E — Performance, streaming and NoSQL (25 marks)

### Question 3.22 — Diagnose a Spark workload (8 marks)

An application has these characteristics:

- It reads a large dataset.
- It applies several transformations.
- It runs six actions on the same expensive intermediate RDD.
- It collects millions of results to the driver.
- It uses `groupByKey()` for calculating sums.

Identify four improvements. Explain the reason for each, and state one trade-off where relevant.

### Question 3.23 — Streaming code interpretation (7 marks)

Consider this classic Spark Streaming program:

```
from pyspark import SparkContext
from pyspark.streaming import StreamingContext

sc = SparkContext(appName="ClickCount")
ssc = StreamingContext(sc, 2)

lines = ssc.socketTextStream("localhost", 9999)

counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

counts.pprint()

ssc.start()
ssc.awaitTermination()
```

Answer:

1. What is the batch interval? (1 mark)
2. What is the role of `socketTextStream()`? (1 mark)
3. Explain the transformations used for counting words. (2 marks)
4. What does `pprint()` do here? (1 mark)
5. Identify two operational considerations or limitations of this program. (2 marks)

### Question 3.24 — NoSQL and CAP (5 marks)

A distributed service must continue responding to requests during network partitions. Some reads may temporarily return stale data.

Explain how this requirement relates to:

- Availability.
- Consistency.
- Eventual consistency.
- The CAP theorem.

Avoid claiming that every NoSQL database uses the same consistency model.

### Question 3.25 — Integrated design question (5 marks)

A company processes a large stream of purchase events and needs to calculate sales totals per customer, inspect the largest totals, and retain results for later analysis.

Propose a high-level Spark design. Include:

- A suitable streaming API.
- A suitable structured or distributed aggregation.
- An approach to handling large outputs.
- One performance consideration.
- One consistency or reliability consideration.

---

# Solutions to Exam 3

## Section A — Multiple-choice answer key (10 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| 3.1 | B | Lineage records dependencies that can help recompute lost partitions. |
| 3.2 | B | `flatMap()` flattens zero or more outputs per input element. |
| 3.3 | B | `alias()` names the calculated output column. |
| 3.4 | C | `count()` returns the number of RDD elements. |
| 3.5 | B | `distinct()` removes duplicate elements. |
| 3.6 | B | DataFrames are well suited to structured aggregation. |
| 3.7 | B | Distributed text output is generally written into multiple part files. |
| 3.8 | B | Performance depends on workload, configuration and data movement. |
| 3.9 | A | Executors execute tasks. |
| 3.10 | B | Redis is an in-memory key-value store with additional data structures. |

## Section B — Model answers (20 marks)

### Question 3.11 — Spark execution lifecycle (5 marks)

A typical execution lifecycle is:

1. The application defines transformations, creating dependencies represented in a DAG.
2. An action requests a result or output.
3. Spark plans the required execution and divides work into stages, with shuffle dependencies commonly separating stages.
4. Tasks process partitions on executors.
5. The driver coordinates scheduling and execution and may receive the result or coordinate distributed output.

**Marking:** 1 mark for each correctly described stage or role.

**Common trap:** The driver coordinates distributed execution; it does not necessarily perform all data processing itself.

### Question 3.12 — Fault tolerance and caching (5 marks)

**Lineage-based recovery** uses recorded dependencies to recompute lost partitions when the source data and required dependencies remain available.

**Caching** stores computed partitions for possible reuse, reducing the cost of repeated computation.

Caching does not eliminate the need for lineage because cached partitions can be evicted or lost. Spark may need to recompute them.

**Marking:**

- Lineage: 2 marks.
- Caching: 2 marks.
- Why lineage remains useful: 1 mark.

### Question 3.13 — Word-count design (5 marks)

- `flatMap()` converts each line into individual tokens.
- `map()` creates `(word, 1)` pairs.
- `reduceByKey()` adds values for identical keys.

One limitation is that whitespace splitting does not normalise case or punctuation. For example, `Spark`, `spark` and `spark,` may be treated as different tokens.

An improvement is to lowercase each line and use a suitable tokenisation rule, such as `re.findall()`.

**Marking:**

- Correct explanation of each transformation: 3 marks.
- Limitation: 1 mark.
- Suitable correction: 1 mark.

### Question 3.14 — Choosing an API (5 marks)

1. **Structured sales report:** Use a DataFrame with `groupBy()` and aggregation functions. Named columns, schemas and Spark SQL optimisation suit structured reporting.
2. **Custom algorithm on arbitrary Python objects:** An RDD may be appropriate when the algorithm requires low-level element processing that is awkward to express through DataFrame operations.
3. **Legacy classic streaming system:** Understand and use the DStream API with `StreamingContext` when maintaining an existing application that uses classic Spark Streaming.

**Marking:** Up to 1.5 marks for each recommendation and justification, plus 0.5 marks for recognising that API choice depends on the workload.

## Section C — Model answers (25 marks)

### Question 3.15 — Debug a word-count pipeline (7 marks)

**Problem**

The first `map()` returns a list of words for each line:

```
["spark", "processes", "data"]
```

The next `map()` wraps that entire list in a tuple rather than creating one pair per word. The keys are therefore lists, which are not suitable as ordinary hash keys for `reduceByKey()`.

**Corrected code**

```
lines = sc.textFile("data/book.txt")

counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

`flatMap()` emits individual tokens rather than lists. `map()` creates one key-value pair per token, and `reduceByKey()` adds the values for each word.

**Marking:**

- Identify nested-list issue: 2 marks.
- Correct `flatMap()`: 2 marks.
- Correct pair mapping and reduction: 1 mark.
- Explain the corrected data flow: 2 marks.

### Question 3.16 — Character count with line separators (5 marks)

```
lines = sc.textFile("data/book.txt", 4)

total_characters = lines.map(
    lambda line: len(line) + 1
).sum()

print(total_characters)
```

The formula is:

$$
\text{Total} = \sum_{\text{line}}(\operatorname{len}(\text{line}) + 1)
$$

The additional one represents a simplified newline separator for every line.

**Assumption:** This method assumes that every returned line should contribute one newline character. It may not match a file's actual byte representation or final-line termination, especially when the file uses different newline conventions. If exact file-byte counting is required, the file representation must be considered explicitly.

**Marking:**

- Correct input: 1 mark.
- Correct per-line calculation: 2 marks.
- Correct aggregation: 1 mark.
- Explanation of newline assumption: 1 mark.

### Question 3.17 — Combining RDDs and actions (6 marks)

```
a = sc.parallelize([1, 2, 2])
b = sc.parallelize([2, 3])

combined = a.union(b)
unique = combined.distinct()
```

1. `combined` has five elements. (1 mark)
2. `unique` has three elements: `1`, `2` and `3`. (1 mark)
3. `union()` combines the elements from both RDDs and retains duplicates. `distinct()` removes duplicate values. (2 marks)
4. `distinct()` typically requires a shuffle because duplicates can occur in different partitions and must be identified across the dataset. (2 marks)

**Common trap:** Do not assume that `union()` performs set union in the mathematical sense. Spark RDD `union()` combines collections; it does not inherently deduplicate them.

### Question 3.18 — Correct aggregation and top-k (7 marks)

A suitable solution is:

```
totals = pairs.reduceByKey(
    lambda a, b: a + b
)

top_three = totals.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

`reduceByKey()` is appropriate because the objective is to calculate one sum per category. Local aggregation can reduce the data shuffled before matching keys are combined.

The `takeOrdered()` key sorts by descending total through the negative value and then by category alphabetically. The secondary key ensures deterministic tie-breaking for categories with equal totals.

**Marking:**

- Correct `reduceByKey()` aggregation: 2 marks.
- Correct `takeOrdered()` usage: 2 marks.
- Descending-order explanation: 1 mark.
- Alphabetical tie-breaking: 1 mark.
- Performance justification: 1 mark.

## Section D — Model answers (20 marks)

### Question 3.19 — Sales analysis (8 marks)

```
from pyspark.sql.functions import col, sum

with_total = df.withColumn(
    "total",
    col("quantity") * col("price")
)

filtered = with_total.where(
    col("total") >= 100
)

summary = filtered.groupBy("customer").agg(
    sum("total").alias("total_sales"),
    sum("quantity").alias("total_quantity")
).orderBy(
    col("total_sales").desc()
)

summary.show()
```

Rows that meet `total >= 100`:

| customer | quantity | price | total |
| --- | --- | --- | --- |
| 10 | 2 | 100 | 200 |
| 20 | 3 | 100 | 300 |
| 30 | 4 | 50 | 200 |

Grouped result:

| customer | total\_sales | total\_quantity |
| --- | --- | --- |
| 20 | 300 | 3 |
| 10 | 200 | 2 |
| 30 | 200 | 4 |

Customers 10 and 30 have equal sales totals. Their relative order is not guaranteed without a secondary sort key.

For deterministic ordering:

```
summary = summary.orderBy(
    col("total_sales").desc(),
    col("customer").asc()
)
```

**Marking:**

- Calculated total: 1 mark.
- Correct filter: 1 mark.
- Correct grouping: 1 mark.
- Total sales aggregation: 2 marks.
- Total quantity aggregation: 1 mark.
- Correct numerical result: 1 mark.
- Correct descending ordering: 1 mark.

### Question 3.20 — Multiple aggregations (6 marks)

```
from pyspark.sql.functions import min, max, avg, count

summary = df.groupBy("customer").agg(
    min("price").alias("minimum_price"),
    max("price").alias("maximum_price"),
    avg("price").alias("average_price"),
    count("*").alias("row_count")
)

summary.show()
```

Calculations:

- Customer 10: prices 100 and 50.
- Customer 20: prices 100 and 25.
- Customer 30: prices 50 and 25.

Expected result:

| customer | minimum\_price | maximum\_price | average\_price | row\_count |
| --- | --- | --- | --- | --- |
| 10 | 50 | 100 | 75 | 2 |
| 20 | 25 | 100 | 62.5 | 2 |
| 30 | 25 | 50 | 37.5 | 2 |

**Marking:**

- Correct grouping: 1 mark.
- Correct minimum: 1 mark.
- Correct maximum: 1 mark.
- Correct average: 1 mark.
- Correct count: 1 mark.
- Correct numerical results: 1 mark.

### Question 3.21 — Debug a DataFrame query (6 marks)

**Problem 1: Incorrect boolean operator**

The expression uses Python's `and` with Spark column expressions. Use `&` with parenthesised conditions.

**Problem 2: Column-expression and query correctness**

`selectExpr()` creates the `total` alias, so filtering on that resulting column is valid in the subsequent `where()` operation. The code's main execution defect is the boolean operator; a corrected expression should also make the intended filtering explicit.

Correct code:

```
from pyspark.sql.functions import col

result = (
    df
    .selectExpr(
        "customer",
        "quantity * price AS total"
    )
    .where(
        (col("total") > 100)
        & (col("customer") == 10)
    )
    .orderBy(col("total").desc())
)
```

An alternative is to filter before selecting the calculated column, but the expression must still use valid Spark column operations.

**Marking:**

- Identify the `and` error: 2 marks.
- Correct `&` usage and parentheses: 2 marks.
- Preserve the intended filtering: 1 mark.
- Correct descending sort: 1 mark.

**Common trap:** Do not claim that `col("total")` is automatically invalid after `selectExpr()`. The alias is part of the resulting DataFrame schema.

## Section E — Model answers (25 marks)

### Question 3.22 — Diagnose a Spark workload (8 marks)

**1\. Cache the expensive intermediate RDD when reuse justifies it.**

The six actions may otherwise recompute expensive transformations. Caching can reduce repeated work, but it consumes memory and possibly disk.

**2\. Avoid full****`collect()`****of millions of results.**

Use `take(n)` for inspection or write the distributed result to external storage. Full collection can exhaust driver memory.

**3\. Replace****`groupByKey()`****with****`reduceByKey()`****for sums.**

`reduceByKey()` can combine values locally before the shuffle, reducing network transfer and memory pressure.

**4\. Minimise unnecessary shuffles and choose suitable partitions.**

Review the transformations for redundant grouping, sorting or deduplication. Appropriate partition counts can improve parallelism without excessive scheduling overhead.

**Marking:** 2 marks per improvement, comprising 1 mark for the recommendation and 1 mark for a correct justification or trade-off.

Other well-justified improvements can receive credit.

### Question 3.23 — Streaming code interpretation (7 marks)

**1\. Batch interval (1 mark)**

The interval is two seconds:

```
ssc = StreamingContext(sc, 2)
```

**2\.****`socketTextStream()`****(1 mark)**

It creates an input stream from a TCP socket, here using `localhost` and port `9999`.

**3\. Word-count transformations (2 marks)**

- `flatMap()` splits lines into individual tokens.
- `map()` produces `(word, 1)` pairs.
- `reduceByKey()` sums the values for matching words within the relevant streaming computation.

**4\.****`pprint()`****(1 mark)**

It prints a limited representation of the DStream's RDD contents for inspection.

**5\. Operational considerations (2 marks)**

Any two of the following:

- A socket server must be running and accessible.
- The classic streaming API and a compatible environment must be available.
- Production deployments require suitable fault-tolerance and recovery configuration.
- The batch interval must be balanced against scheduling overhead and incoming data volume.
- The application needs to remain running to process incoming batches.

### Question 3.24 — NoSQL and CAP (5 marks)

**Availability:** The service aims to continue responding to requests, even during network partitions.

**Consistency:** Strict consistency would require reads and writes to obey the system's defined consistency guarantees. Some distributed systems may instead permit temporarily stale reads.

**Eventual consistency:** Under suitable conditions, replicas can converge if updates stop and communication resumes. It does not mean that every read is immediately current.

**CAP:** During a network partition, a distributed system cannot guarantee both strict consistency and availability simultaneously. A system designed to remain responsive may relax consistency for some operations, depending on its design and guarantees.

**Marking:**

- Availability: 1 mark.
- Consistency: 1 mark.
- Eventual consistency: 1 mark.
- CAP trade-off: 2 marks.

**Common trap:** Not every NoSQL database uses the same consistency model. NoSQL describes a diverse family of systems.

### Question 3.25 — Integrated design question (5 marks)

One suitable design is:

1. **Streaming API:** Use Structured Streaming for a new structured purchase-event pipeline, subject to the source and sink requirements.
2. **Aggregation:** Represent events as a DataFrame and group by customer to calculate sales totals.
3. **Large outputs:** Persist results through a suitable distributed sink rather than collecting the entire dataset to the driver.
4. **Performance:** Filter irrelevant events early, avoid unnecessary shuffles, and consider caching only for reused intermediate results.
5. **Reliability and consistency:** Configure an appropriate checkpointing and recovery strategy and choose sink semantics suitable for the required delivery and consistency guarantees.

**Marking:** 1 mark per requirement.

Equivalent designs using classic DStreams may receive credit if the question's assumptions are stated and the approach is justified.

---

# Consolidated marking rubric

Use this rubric to evaluate all three exams consistently.

## Performance bands

| Score | Suggested interpretation | What the result indicates |
| --- | --- | --- |
| 90–100 | Excellent | Strong conceptual understanding, accurate code, justified design choices and few misconceptions. |
| 80–89 | Very good | Good command of Spark and its APIs, with only minor gaps. |
| 70–79 | Good | Understands the main execution model and can solve most standard exercises. |
| 60–69 | Satisfactory | Can handle basic questions but has weaknesses in performance analysis or integrated tasks. |
| 50–59 | Borderline | Partial understanding; important API distinctions or implementation details remain unclear. |
| Below 50 | Needs revision | Significant gaps in core concepts, code construction or execution reasoning. |

These bands are study guidelines, not official UAB grade thresholds.

## Detailed marking principles

### Multiple-choice questions

- Award the stated mark for the single correct answer.
- Do not award marks for multiple selected answers when the question requests exactly one.

### Short-answer questions

- Award full credit for correct definitions supported by accurate explanations.
- Award partial credit for a correct core idea with incomplete terminology.
- Do not award full marks for unsupported keywords alone.

### PySpark code

- Reward valid API usage and the correct sequence of transformations.
- Accept equivalent implementations where they satisfy the requirements.
- Give partial credit for a correct algorithm with minor syntax mistakes.
- Penalise using an action where a transformation is required, or vice versa.
- Penalise incorrect assumptions about ordering, tokenisation or duplicates when these change the result.

### Performance analysis

Full-credit answers should generally identify:

1. The relevant bottleneck or risk.
2. A suitable improvement.
3. Why the improvement works.
4. A relevant trade-off, where applicable.

### Numerical questions

- Award credit for the correct calculation method even if an arithmetic error affects the final result.
- Check that the result matches the stated filtering, grouping and sorting rules.
- Do not assume grouped DataFrame results have a particular order unless an explicit sort is specified.

---

# Final revision checklist

Before sitting the actual examination, make sure you can do the following without consulting notes.

## Architecture and execution

- [ ]Explain the roles of the driver, cluster manager, worker, executor and partition.
- [ ]Explain the difference between `SparkContext` and `SparkSession`.
- [ ]Describe the relationship between transformations, actions, DAGs, stages and tasks.
- [ ]Explain lazy evaluation and lineage.
- [ ]Distinguish fault tolerance from caching.

## RDDs and PySpark

- [ ]Create RDDs using `parallelize()` and `textFile()`.
- [ ]Explain immutability and distributed processing.
- [ ]Distinguish transformations from actions.
- [ ]Classify narrow and wide transformations.
- [ ]Explain when shuffles occur and why they are expensive.
- [ ]Implement character counting.
- [ ]Implement word counting.
- [ ]Implement top-k frequency queries with deterministic tie-breaking.
- [ ]Explain why `union()` does not remove duplicates.
- [ ]Explain the differences between `reduceByKey()` and `groupByKey()`.
- [ ]Explain the risks of large `collect()` operations.

## DataFrames

- [ ]Read JSON and inspect schemas.
- [ ]Use `select()` and `selectExpr()`.
- [ ]Calculate new columns with `col()` and `alias()`.
- [ ]Filter with `where()` and `filter()`.
- [ ]Combine conditions using `&` and `|`.
- [ ]Sort using `orderBy()`.
- [ ]Group and aggregate using `groupBy()` and `agg()`.
- [ ]Calculate sums, averages, minimums, maximums and counts.

## Performance

- [ ]Explain why caching can improve repeated workloads.
- [ ]Identify cases where caching is not worthwhile.
- [ ]Minimise unnecessary shuffles.
- [ ]Select appropriate partition counts.
- [ ]Prefer DataFrames for suitable structured workloads.
- [ ]Explain why Spark's performance advantage depends on the workload.

## Streaming

- [ ]Define a DStream.
- [ ]Explain the roles of `StreamingContext` and batch intervals.
- [ ]Trace a classic streaming word-count pipeline.
- [ ]Compare DStreams with Structured Streaming.
- [ ]Explain latency, throughput and scheduling trade-offs.

## NoSQL and distributed consistency

- [ ]Identify document, key-value, column-family and graph data models.
- [ ]Expand ACID and BASE.
- [ ]Explain the CAP trade-off during a network partition.
- [ ]Match a database model to a workload and its access patterns.

---

# Recommended GitHub repository structure

Save the document as `T5-Three-Spark-Practice-Exams.md`.

A possible repository layout is:

```
data-management-revision/
├── README.md
├── T5-Spark-Exam-Compression.md
├── T5-Three-Spark-Practice-Exams.md
├── exams/
│   ├── T5-Practice-Exam-1.md
│   ├── T5-Practice-Exam-2.md
│   └── T5-Practice-Exam-3.md
└── solutions/
    ├── T5-Practice-Exam-1-Solutions.md
    ├── T5-Practice-Exam-2-Solutions.md
    └── T5-Practice-Exam-3-Solutions.md
```

To create the main file locally:

```
mkdir -p data-management-revision
cd data-management-revision

touch T5-Three-Spark-Practice-Exams.md

git init
git add T5-Three-Spark-Practice-Exams.md
git commit -m "Add three Spark practice exams and solutions"
```

Paste the contents of this document into the Markdown file before committing. The commands initialise a local Git repository; publishing to GitHub additionally requires creating or selecting a remote repository and configuring the remote.

---

## Final study strategy

1. **Exam 1:** Focus on core definitions, execution behaviour, RDD transformations and standard exercises.
2. **Exam 2:** Focus on DataFrame syntax, numerical aggregation, query interpretation and performance reasoning.
3. **Exam 3:** Focus on debugging, integrated problem-solving, streaming and distributed-system trade-offs.

For the most realistic practice, complete each exam in 120 minutes, mark it using the solutions, and maintain an error log recording the question, the misconception, the correct reasoning and the revision needed.

**Scope note:** These exams are original practice material based on the supplied T5/T6 compression. They are not official UAB examinations and cannot guarantee coverage of details present only in the original lecture slides.
