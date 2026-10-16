The comprehensive solutions have not been generated yet in the conversation. The material you provided contains revision notes and 17 exam-style questions with model answers, but it does not contain three complete, 120-minute practice exams with full marking schemes and worked solutions.

I’ll use your T5 Apache Spark compression as the source and structure the three exams as follows:

- Practice Exam 1 — Foundations: Spark architecture, RDDs, lazy evaluation, transformations, actions and code interpretation.
- Practice Exam 2 — Applied PySpark: Word and character counts, shuffles, DataFrame operations, aggregations, debugging and performance.
- Practice Exam 3 — Comprehensive: Integrated practical problems, streaming, NoSQL, ACID/BASE/CAP, conceptual traps and optimisation.

Each exam will be designed for 120 minutes and 100 marks, with a student question paper followed by comprehensive solutions, explanations, common pitfalls and a point-by-point marking rubric. The format will be GitHub-compatible Markdown, ready to save as `.md` files.

## Suggested GitHub repository structure

T5-Spark-Practice-Exams/

`Practice-Exam-1-Solutions.md`

Foundations, architecture, RDDs and execution model

`Practice-Exam-2-Solutions.md`

PySpark programming, DataFrames and optimisation

`Practice-Exam-3-Solutions.md`

Integrated problems, streaming, NoSQL and exam traps

The documents below include both the questions and the solutions, so you can attempt each exam under timed conditions before consulting its answer key.

# Practice Exam 1 — Foundations and Execution Model

Writing

# T5 — Practice Exam 1: Spark Foundations and Execution Model

**Course:** UAB 106566 — Data Management\
**Academic year:** 2026/2027\
**Degree:** Bachelor's Degree in Artificial Intelligence\
**Duration:** 120 minutes\
**Total:** 100 marks

## Instructions

- Answer all questions.
- Explain your reasoning where requested.
- Assume standard PySpark RDD semantics unless stated otherwise.
- Code snippets are illustrative and assume an appropriately configured Spark session/context.
- `collect()` output ordering is not guaranteed after operations involving shuffles unless an explicit ordering is applied.
- Suggested time allocations total 120 minutes.

## Part A — Multiple-choice questions (20 marks, 20 minutes)

**Select exactly one answer per question. Each question is worth 2 marks. No negative marking.**

### Q1. Which statement best describes an RDD?

A. A mutable Python list stored entirely on the driver.\
B. An immutable distributed collection that Spark can process in parallel.\
C. A relational database table that always has a fixed schema.\
D. A file format used exclusively for streaming.

### Q2. Which operation is an action?

A. `map()`\
B. `filter()`\
C. `flatMap()`\
D. `count()`

### Q3. What normally happens when the following code executes?

```
rdd = sc.parallelize([1, 2, 3, 4])
result = rdd.map(lambda x: x * 2)
```

A. Spark immediately computes all doubled values and stores them on the driver.\
B. Spark records the transformation; execution is normally deferred until an action requires the result.\
C. The original RDD is modified in place.\
D. Spark automatically writes the result to disk.

### Q4. Which operation is typically narrow?

A. `distinct()`\
B. `groupByKey()`\
C. `filter()`\
D. `sortByKey()`

### Q5. Which statement about `collect()` is correct?

A. It returns only the first ten elements.\
B. It removes duplicates before returning data.\
C. It retrieves all elements into a collection on the driver.\
D. It always writes the result to distributed storage.

### Q6. What is the primary role of the Spark driver?

A. Store every input record in local memory.\
B. Coordinate the application, plan execution and schedule tasks.\
C. Replace every executor after each action.\
D. Guarantee that every operation runs on a single machine.

### Q7. What is lineage used for?

A. Automatically converting RDDs into SQL tables.\
B. Tracking computation dependencies so lost partitions can often be recomputed.\
C. Ensuring that all data is permanently cached in memory.\
D. Sorting records alphabetically.

### Q8. Which statement about `union()` is correct?

A. It automatically removes duplicates.\
B. It automatically sorts all elements.\
C. It combines the elements of two RDDs without inherently removing duplicates.\
D. It is an action that returns a Python list.

### Q9. Which operation is generally preferable for summing values by key?

A. `groupByKey()` followed by unnecessary materialisation of every value.\
B. `reduceByKey()` with an addition function.\
C. `collect()` followed by a local loop on the driver.\
D. `distinct()`.

### Q10. Which statement about partitions is correct?

A. Each worker must have exactly one partition.\
B. The number of partitions must equal the number of executors.\
C. A worker can process multiple partitions, and partition counts need not equal worker counts.\
D. Partitions are used only for streaming.

## Part B — Short-answer and conceptual questions (20 marks, 25 minutes)

### Q11. Spark architecture (6 marks)

Explain the roles of the following components:

1. Driver.
2. Cluster manager.
3. Executor.
4. Partition.

Explain why the driver should not be confused with an executor.

### Q12. Lazy evaluation and DAGs (6 marks)

Consider:

```
rdd = sc.parallelize([1, 2, 3, 4, 5])
a = rdd.filter(lambda x: x % 2 == 1)
b = a.map(lambda x: x * x)
c = b.map(lambda x: x + 1)
```

1. How many transformations are explicitly defined? (2 marks)
2. Has the code necessarily computed all values in `c` yet? Explain. (2 marks)
3. What changes when `c.collect()` is called? (2 marks)

### Q13. Narrow transformations and shuffles (8 marks)

For each operation, classify it as typically narrow or wide and explain why:

- `map()`
- `filter()`
- `distinct()`
- `reduceByKey()`

Define a shuffle and give two reasons it may be expensive.

## Part C — RDD code interpretation (20 marks, 25 minutes)

### Q14. Trace the computation (8 marks)

Given:

```
rdd = sc.parallelize([1, 2, 2, 3, 4])
a = rdd.filter(lambda x: x > 1)
b = a.map(lambda x: x * 2)

print(b.collect())
print(a.count())
print(rdd.count())
```

Answer:

1. What values does `b.collect()` return? (3 marks)
2. What does `a.count()` return? (2 marks)
3. What does `rdd.count()` return? (1 mark)
4. Has `rdd` been modified? Explain. (2 marks)

### Q15. Identify and correct misconceptions (6 marks)

A student makes these claims:

1. “Calling `map()` immediately executes the whole computation.”
2. “`union()` removes duplicates.”
3. “Four partitions require four physical machines.”

Correct each claim in one or two sentences. (2 marks each)

### Q16. Actions and result sizes (6 marks)

For an RDD containing millions of records, compare `take(5)`, `count()` and `collect()`.

For each operation, state what it returns and identify the main concern, if any, when processing a large dataset.

## Part D — Practical programming (25 marks, 30 minutes)

### Q17. Character count (8 marks)

Write PySpark code that reads `data/quijote.txt` using four requested input partitions and calculates the total number of characters across all lines.

Requirements:

- Use an RDD.
- Use an appropriate aggregation.
- Do not collect all lines to the driver.
- Explain the role of each principal operation.

State one limitation of this counting method.

### Q18. Word count (10 marks)

Given:

```
lines = sc.textFile("data/quijote.txt")
```

Write code to count the occurrences of each whitespace-separated token.

Your solution must use:

1. `flatMap()`.
2. `map()`.
3. `reduceByKey()`.

Explain why `flatMap()` is appropriate and why `reduceByKey()` is preferable to `groupByKey()` for this task.

### Q19. Top three words (7 marks)

Assume `word_counts` contains `(word, count)` pairs.

Write code to retrieve the three most frequent words, breaking ties alphabetically by word. Explain the sorting key.

## Part E — Performance and reliability (15 marks, 20 minutes)

### Q20. Diagnose an inefficient program (8 marks)

A student writes:

```
pairs = lines.flatMap(
    lambda line: line.split()
).map(
    lambda word: (word, 1)
)

all_pairs = pairs.collect()

counts = {}
for word, value in all_pairs:
    counts[word] = counts.get(word, 0) + value
```

Identify two scalability or performance problems and rewrite the solution using an appropriate distributed aggregation. Explain the improvement.

### Q21. Caching and fault tolerance (7 marks)

A filtered RDD is used in three subsequent actions. The filtering operation is expensive.

1. How could caching help? (2 marks)
2. Does calling `.cache()` necessarily compute all partitions immediately? (2 marks)
3. How can lineage help after a partition is lost? (2 marks)
4. Give one reason not to cache a dataset. (1 mark)

---

# SOLUTIONS AND MARKING SCHEME

## Part A — Multiple-choice answers (20 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| Q1 | B | An RDD is immutable, distributed and parallelisable. |
| Q2 | D | `count()` is an action. The other operations are transformations. |
| Q3 | B | `map()` defines a lazy transformation; an action normally triggers the required computation. |
| Q4 | C | `filter()` can retain or discard records within existing partitions. |
| Q5 | C | `collect()` brings every element to the driver. |
| Q6 | B | The driver coordinates execution and task scheduling. |
| Q7 | B | Lineage records dependencies used for recomputation when possible. |
| Q8 | C | `union()` combines elements; it does not automatically deduplicate or globally sort them. |
| Q9 | B | `reduceByKey()` performs keyed aggregation and can combine values locally before shuffling. |
| Q10 | C | Partitions are processing units, not physical machines. |

**Marking:** 2 marks per correct answer; 0 for an incorrect or missing answer.

**Common traps:**

- Confusing a transformation with an action.
- Assuming laziness means Spark never executes the computation.
- Treating partitions, workers and executors as interchangeable.
- Assuming `collect()` is safe merely because it is convenient.

## Part B — Model answers (20 marks)

### Q11. Spark architecture (6 marks)

- **Driver (2 marks):** Runs the application, constructs execution plans, coordinates execution and schedules tasks.
- **Cluster manager (1.5 marks):** Allocates resources for the application. Examples include Spark Standalone, YARN and Kubernetes.
- **Executor (1.5 marks):** A process that executes tasks and may store cached data on worker resources.
- **Partition (1 mark):** A logical subdivision of distributed data that can be processed as a task's input.

**Why the driver is different:** The driver coordinates the distributed computation; executors perform the distributed tasks. The driver does not normally process every record itself.

**Partial credit:** Award half the component marks when the general role is correct but important details are missing.

**Exam trap:** A worker is a machine; an executor is a process. They are not synonymous.

### Q12. Lazy evaluation and DAGs (6 marks)

1. **Three transformations (2 marks):** `filter()`, the first `map()` and the second `map()`.
2. **Not necessarily computed (2 marks):** These transformations normally define a dependency graph without immediately evaluating every element.
3. **Effect of****`collect()`****(2 marks):** It requests all resulting elements at the driver and triggers the computation needed to produce them.

The conceptual graph is:

`rdd → filter → map → map → collect`

The output, if collected, is:

```
[2, 10, 26]
```

The intermediate filtered values are `[1, 3, 5]`, the squared values are `[1, 9, 25]`, and the final values are `[2, 10, 26]`.

**Exam trap:** Lazy evaluation is not the same as permanent non-execution. The action requests the necessary computation.

### Q13. Narrow transformations and shuffles (8 marks)

- **`map()`****— narrow (1.5 marks):** Each output element is produced from an input element without normally redistributing records between partitions.
- **`filter()`****— narrow (1.5 marks):** Each partition can retain or discard its own records.
- **`distinct()`****— wide (1.5 marks):** Identifying duplicates across partitions typically requires redistribution.
- **`reduceByKey()`****— wide overall (1.5 marks):** Values for the same key may originate in different partitions and need to be combined. Local combining can reduce, but not necessarily eliminate, shuffle traffic.
- **Shuffle definition (1 mark):** Redistribution of data between partitions, often across the network.
- **Two costs (1 mark total):** Examples include network communication, serialization, disk spill, memory pressure and extra stage overhead.

**Common misconception:** A wide transformation does not mean every record is sent across the network. It means the dependency can require redistribution across partitions.

## Part C — Model answers (20 marks)

### Q14. Trace the computation (8 marks)

The original RDD is:

```
[1, 2, 2, 3, 4]
```

After `filter(x > 1)`:

```
[2, 2, 3, 4]
```

After doubling:

```
[4, 4, 6, 8]
```

1. `b.collect()` returns `[4, 4, 6, 8]` (3 marks).
2. `a.count()` returns `4` (2 marks).
3. `rdd.count()` returns `5` (1 mark).
4. The original RDD is unchanged because transformations produce new RDDs rather than mutating existing ones (2 marks).

The actions may cause Spark to recompute shared transformations if the intermediate results have not been cached.

**Exam trap:** Repeated actions do not imply that all previously computed intermediate RDDs are automatically cached.

### Q15. Misconceptions (6 marks)

1. **Incorrect:** `map()` normally defines a lazy transformation; execution is triggered when an action requires its result. (2 marks)
2. **Incorrect:** `union()` combines the elements of both RDDs and preserves duplicates unless another operation removes them. (2 marks)
3. **Incorrect:** Four partitions do not require four physical machines. Multiple partitions can be processed on one worker, and workers can process multiple tasks. (2 marks)

### Q16. Actions and result sizes (6 marks)

- **`take(5)`****(2 marks):** Returns up to five elements to the driver. Useful for inspection because it limits the requested result size.
- **`count()`****(2 marks):** Returns a single count, but Spark may still need to process the entire RDD to calculate it.
- **`collect()`****(2 marks):** Returns all elements to the driver. It can exhaust driver memory on a very large dataset.

**Exam trap:** A small result does not always mean little computation. `count()` returns one number but may require processing every partition.

## Part D — Model answers (25 marks)

### Q17. Character count (8 marks)

```
lines = sc.textFile("data/quijote.txt", 4)

total_characters = lines.map(len).sum()

print(total_characters)
```

Marking:

- Read the text file as an RDD and request four partitions: 2 marks.
- Use `map(len)` to calculate the character count for each line: 2 marks.
- Use `.sum()` to aggregate the counts: 2 marks.
- Explain distributed aggregation without collecting all lines: 1 mark.
- State a valid limitation: 1 mark.

**Explanation:** `textFile()` produces an RDD whose elements normally represent lines. `map(len)` converts each line into a number. `sum()` aggregates these numbers into one result.

**Limitation:** Line terminators are generally not included in the line strings. This calculates the total number of characters in the returned line strings, not necessarily the number of bytes in the source file or the count including newline separators.

A valid alternative is:

```
total_characters = lines.map(
    lambda line: len(line)
).reduce(lambda a, b: a + b)
```

### Q18. Word count (10 marks)

```
lines = sc.textFile("data/quijote.txt")

word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

Marking:

- Read the input RDD: 1 mark.
- Use `flatMap()` to emit individual tokens: 3 marks.
- Map each token to `(word, 1)`: 2 marks.
- Use `reduceByKey()` to sum values for each key: 2 marks.
- Explain `flatMap()` and `reduceByKey()` choices: 2 marks.

**Why****`flatMap()`****?** A line contains multiple words. `flatMap()` converts each line into multiple output elements and flattens them into one RDD.

**Why****`reduceByKey()`****?** It combines counts associated with the same word and can perform local aggregation before data is shuffled. `groupByKey()` would gather values for each word even though the task only needs a sum.

**Important limitation:** `split()` separates on whitespace. It does not automatically remove punctuation or normalise capitalisation. For example, `"Spark"`, `"spark"` and `"Spark,"` can be different keys.

### Q19. Top three words (7 marks)

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)

print(top_three)
```

Marking:

- Use `takeOrdered(3)`: 2 marks.
- Use the negative count to rank higher frequencies first: 2 marks.
- Use the word as the alphabetical tie-breaker: 2 marks.
- Explain that each pair is `(word, count)`: 1 mark.

The key `(-pair[1], pair[0])` orders pairs by descending count and then ascending word.

**Exam trap:** `takeOrdered(3)` uses ascending order by default. Negating the count reverses the frequency ranking.

## Part E — Model answers (15 marks)

### Q20. Diagnose an inefficient program (8 marks)

Two problems:

1. **Driver memory pressure:** `collect()` transfers every word pair to the driver, potentially exhausting its memory. (2 marks)
2. **Loss of distributed aggregation:** Counting occurs in a local Python dictionary after the complete intermediate dataset has been transferred. (2 marks)

Improved solution:

```
word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

Marking:

- Correct distributed pipeline: 2 marks.
- Explanation of keyed aggregation: 1 mark.
- Explanation of reduced driver transfer and possible local combining: 1 mark.

The result remains distributed. If the complete result is small enough and explicitly required locally, it can be collected afterwards; otherwise, use distributed output or a limited sample.

### Q21. Caching and fault tolerance (7 marks)

1. **Benefit (2 marks):** Caching may avoid repeatedly recomputing an expensive filtered RDD when subsequent actions reuse it.
2. **Lazy cache (2 marks):** `.cache()` marks the RDD for caching. It does not necessarily compute all partitions immediately; required partitions are populated as computation reaches them.
3. **Lineage (2 marks):** Spark can recompute lost partitions from source data and recorded dependencies when those sources and dependencies remain available.
4. **Reason not to cache (1 mark):** It consumes memory or storage resources and may offer little benefit if the dataset is used only once.

**Final score interpretation**

- **85–100:** Strong command of Spark foundations.
- **70–84:** Good understanding; review shuffles, execution and performance details.
- **50–69:** Partial understanding; practise code tracing and transformations versus actions.
- **Below 50:** Revisit the RDD model, lazy evaluation and core PySpark patterns.

These bands are revision guidance, not official university grade boundaries.

# Practice Exam 2 — PySpark, DataFrames and Performance

Writing

# T5 — Practice Exam 2: Applied PySpark and Optimisation

**Course:** UAB 106566 — Data Management\
**Duration:** 120 minutes\
**Total:** 100 marks

## Instructions

Answer all questions. Show intermediate calculations where appropriate. Assume the following transaction DataFrame unless a question states otherwise.

| customer | product | quantity | price |
| --- | --- | --- | --- |
| 200 | 5 | 8 | 33 |
| 200 | 8 | 5 | 111 |
| 100 | 8 | 5 | 45 |
| 100 | 8 | 5 | 59 |
| 300 | 2 | 2 | 10 |

Each row represents a transaction line. `price` is the price per unit.

Suggested time allocations total 120 minutes.

## Part A — Multiple choice (15 marks, 15 minutes)

Each question is worth 3 marks. Select one answer.

### Q1. Which is generally the best abstraction for filtering, joining and aggregating structured tabular data in Spark?

A. A Python list on the driver.\
B. A DataFrame.\
C. A DStream for every workload.\
D. A manually maintained dictionary.

### Q2. What does this expression calculate?

```
df.select(
    (col("quantity") * col("price")).alias("total")
)
```

A. The sum of all prices.\
B. The number of rows.\
C. A per-row quantity-times-price column named `total`.\
D. The maximum transaction value.

### Q3. Which expression correctly combines two Spark column conditions?

A. `col("quantity") > 0 and col("price") > 0`\
B. `(col("quantity") > 0) & (col("price") > 0)`\
C. `col("quantity") > 0 && col("price") > 0`\
D. `col("quantity") > 0, col("price") > 0`

### Q4. Why can `reduceByKey()` be more efficient than `groupByKey()` for word count?

A. It never transfers data across the network.\
B. It automatically removes punctuation.\
C. It can combine values locally before shuffling.\
D. It stores all results on the driver.

### Q5. What is a sensible reason to cache an RDD?

A. Every RDD must be cached before use.\
B. The RDD is expensive to compute and reused by multiple actions.\
C. The dataset is guaranteed to fit in memory.\
D. Caching automatically removes all shuffles.

## Part B — DataFrame calculations (25 marks, 30 minutes)

### Q6. Calculate transaction totals (8 marks)

Using the table above:

1. Write PySpark code to create a DataFrame column named `total` equal to `quantity * price`, retaining all original columns.
2. Calculate the `total` value for every row.
3. State the total sales value across all five rows.

### Q7. Filtering and sorting (7 marks)

Write PySpark code to select rows satisfying both conditions:

- `quantity > 4`
- `price > 50`

Then sort the selected rows by `price` descending and `customer` ascending.

State which rows satisfy the filter.

### Q8. Aggregation by customer (10 marks)

Write PySpark code that calculates, for each customer:

- Total quantity.
- Total sales value, calculated as the sum of `quantity * price`.
- Average unit price.

Calculate the expected result for each customer using the supplied table.

## Part C — RDD exercises and interpretation (25 marks, 30 minutes)

### Q9. Word count with normalisation (8 marks)

Write a word-count pipeline that:

- Converts words to lowercase.
- Removes punctuation using a regular expression.
- Counts the resulting tokens by key.

Use `flatMap()`, `map()` and `reduceByKey()`.

Explain why this result can differ from a simple whitespace-based word count.

### Q10. Aggregation interpretation (7 marks)

Given:

```
data = sc.parallelize([
    ("A", 2),
    ("A", 3),
    ("B", 4),
    ("A", 5),
    ("B", 1)
])

result = data.reduceByKey(lambda a, b: a + b)
```

1. What key-value pairs are in the result? (3 marks)
2. Is `reduceByKey()` a transformation or action? (1 mark)
3. Does `result` have to be computed immediately? (1 mark)
4. Explain why `groupByKey()` is generally less suitable here. (2 marks)

### Q11. Debug the top-word query (5 marks)

A student writes:

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: pair[1]
)
```

The student says this returns the three most frequent words.

Explain the mistake and provide corrected code that breaks frequency ties alphabetically.

### Q12. Explain a shuffle (5 marks)

Explain why `distinct()`, `reduceByKey()` and global sorting commonly involve shuffles, and why `map()` normally does not.

## Part D — Performance and debugging (20 marks, 25 minutes)

### Q13. Optimise repeated work (7 marks)

Consider:

```
filtered = lines.filter(
    lambda line: "Spark" in line
)

print(filtered.count())
print(filtered.take(10))
print(filtered.count())
```

1. Why might Spark repeat work? (2 marks)
2. How could caching help? Write the modified code. (3 marks)
3. Give one reason caching could be counterproductive. (2 marks)

### Q14. Diagnose an unsafe collection (7 marks)

A dataset contains several hundred million key-value pairs.

```
pairs = rdd.collect()
answer = {}

for key, value in pairs:
    answer[key] = answer.get(key, 0) + value
```

Explain two problems and rewrite the computation in Spark. Explain what happens to the result.

### Q15. Partitioning and scalability (6 marks)

A job processes a large dataset using only two partitions on a cluster with many available executors.

1. Identify one likely performance problem. (2 marks)
2. Explain the risk of creating thousands of tiny partitions unnecessarily. (2 marks)
3. Explain why the number of partitions need not equal the number of workers. (2 marks)

## Part E — Integrated practical problem (15 marks, 20 minutes)

### Q16. Transaction analysis (15 marks)

Write a complete PySpark solution that:

1. Reads a JSON file into a DataFrame. (2 marks)
2. Adds a calculated `total` column. (2 marks)
3. Filters transactions where `quantity > 0` and `price > 0`. (3 marks)
4. Groups by customer and calculates total sales. (3 marks)
5. Sorts customers by total sales descending. (2 marks)
6. Explains why a DataFrame is appropriate and why collecting the full input to the driver is unnecessary. (3 marks)

---

# SOLUTIONS AND MARKING SCHEME

## Part A — Answers (15 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| Q1 | B | DataFrames provide schemas and structured query optimisation. |
| Q2 | C | Multiplication is evaluated for each row; `alias()` names the expression. |
| Q3 | B | Spark column conditions use `&` or \` |
| Q4 | C | Local combining can reduce the volume shuffled. |
| Q5 | B | Caching is most useful when expensive results are reused. |

Each correct answer earns 3 marks.

**Common traps:** Python's `and` is not the correct operator for combining Spark column expressions. A DataFrame expression does not itself calculate a local Python scalar.

## Part B — Model answers (25 marks)

### Q6. Calculate transaction totals (8 marks)

Code:

```
from pyspark.sql.functions import col, sum

with_total = df.selectExpr(
    "*",
    "quantity * price AS total"
)
```

Alternatively:

```
with_total = df.withColumn(
    "total",
    col("quantity") * col("price")
)
```

Both produce a DataFrame retaining the original columns and adding `total`.

Calculations:

| customer | quantity | price | total |
| --- | --- | --- | --- |
| 200 | 8 | 33 | 264 |
| 200 | 5 | 111 | 555 |
| 100 | 5 | 45 | 225 |
| 100 | 5 | 59 | 295 |
| 300 | 2 | 10 | 20 |

Total sales:

$$
264 + 555 + 225 + 295 + 20 = 1359
$$

**Marking:**

- Correct code retaining the original columns: 3 marks.
- Correct five row totals: 3 marks.
- Correct grand total of 1359: 2 marks.

**Exam trap:** `quantity * price` calculates a row-level value. It does not calculate total sales across the entire dataset unless an aggregation is applied.

### Q7. Filtering and sorting (7 marks)

```
from pyspark.sql.functions import col

result = (
    df.where(
        (col("quantity") > 4)
        & (col("price") > 50)
    )
    .orderBy(
        col("price").desc(),
        col("customer").asc()
    )
)
```

Only the second row satisfies both conditions:

| customer | product | quantity | price |
| --- | --- | --- | --- |
| 200 | 8 | 5 | 111 |

**Marking:**

- Correct filter with parentheses and `&`: 3 marks.
- Correct descending price sort: 2 marks.
- Correct ascending customer tie-breaker: 1 mark.
- Correct matching row: 1 mark.

**Trap:** Both filter conditions must be true. A row with a sufficiently large quantity but a price below or equal to 50 must be excluded.

### Q8. Aggregation by customer (10 marks)

Code:

```
from pyspark.sql.functions import sum, avg, col

summary = df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity"),
    sum(col("quantity") * col("price")).alias("total_sales"),
    avg("price").alias("average_unit_price")
)
```

Expected values:

| customer | total\_quantity | total\_sales | average\_unit\_price |
| --- | --- | --- | --- |
| 100 | 10 | 520 | 52 |
| 200 | 13 | 819 | 72 |
| 300 | 2 | 20 | 10 |

Calculations:

- Customer 100: quantity $5+5=10$; sales $225+295=520$; average price $(45+59)/2=52$.
- Customer 200: quantity $8+5=13$; sales $264+555=819$; average price $(33+111)/2=72$.
- Customer 300: quantity $2$; sales $20$; average price $10$.

**Marking:**

- Correct grouping: 2 marks.
- Correct total quantity: 2 marks.
- Correct total sales expression: 2 marks.
- Correct average price: 2 marks.
- Correct results: 2 marks.

**Exam trap:** Average unit price is not the same as sales-weighted average price. The question asks for `avg(price)`, whereas a weighted average would require a different expression.

## Part C — Model answers (25 marks)

### Q9. Word count with normalisation (8 marks)

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

**Marking:**

- Lowercase normalisation: 2 marks.
- Regular-expression tokenisation: 2 marks.
- Map to `(word, 1)`: 2 marks.
- `reduceByKey()` and explanation: 2 marks.

This result differs from whitespace-based counting because punctuation is removed from token boundaries and uppercase/lowercase forms are normalised. The exact tokenisation behaviour depends on the regular expression and the definition of a word.

### Q10. Aggregation interpretation (7 marks)

Expected result:

```
[("A", 10), ("B", 5)]
```

The output order is not guaranteed without explicit sorting.

- Correct totals: 3 marks.
- `reduceByKey()` is a transformation: 1 mark.
- Execution is normally lazy until an action requests the result: 1 mark.
- `groupByKey()` gathers values rather than combining them immediately into a sum; it can move more data and use more memory: 2 marks.

### Q11. Debug the top-word query (5 marks)

The key `pair[1]` orders counts in ascending order. Because `takeOrdered()` returns the smallest elements by default, it selects low-frequency words rather than high-frequency words.

Correct code:

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

- Identify ascending-order error: 2 marks.
- Correct negative count: 2 marks.
- Alphabetical tie-breaking: 1 mark.

### Q12. Explain a shuffle (5 marks)

A shuffle redistributes data between partitions. `distinct()` commonly needs to identify duplicates across partitions; `reduceByKey()` must combine values with matching keys that may reside in different partitions; global sorting must coordinate ordering across the dataset. These operations can require network communication, serialisation and additional stages.

`map()` normally transforms each element within its current partition, so it generally does not require redistribution.

**Marking:** Definition (1), explanations for wide operations (3), explanation of `map()` (1).

## Part D — Model answers (20 marks)

### Q13. Optimise repeated work (7 marks)

The filtered RDD is referenced by three actions. Without caching, Spark may recompute the filter for each action.

Improved code:

```
filtered = lines.filter(
    lambda line: "Spark" in line
).cache()

print(filtered.count())
print(filtered.take(10))
print(filtered.count())
```

- Explain possible recomputation: 2 marks.
- Correct caching modification: 3 marks.
- Valid drawback: 2 marks.

Caching consumes memory or storage and may evict other useful data. It is not automatically beneficial for a dataset used only once or one that is cheap to recompute.

### Q14. Diagnose an unsafe collection (7 marks)

Problems:

1. `collect()` transfers every pair to the driver and may exhaust driver memory.
2. The local loop removes the opportunity to perform the aggregation distributively before transferring the result.

Improved solution:

```
answer = rdd.reduceByKey(
    lambda a, b: a + b
)
```

The result is another distributed RDD containing one aggregate per key. It is not automatically a local Python dictionary.

**Marking:** Two problems (4 marks), correct code (2 marks), explanation of distributed result (1 mark).

### Q15. Partitioning and scalability (6 marks)

- Two partitions may limit parallelism: at most two partition tasks from that stage can process data concurrently, subject to the execution model. (2 marks)
- Thousands of tiny partitions can create task-scheduling overhead and excessive small output files. (2 marks)
- Partitions are logical units of processing. Workers can execute multiple partition tasks, so the counts do not have to match. (2 marks)

## Part E — Model answer (15 marks)

### Q16. Transaction analysis

```
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, sum

spark = (
    SparkSession.builder
    .appName("TransactionAnalysis")
    .getOrCreate()
)

df = spark.read.format("json").load("transactions.json")

with_total = df.withColumn(
    "total",
    col("quantity") * col("price")
)

valid = with_total.where(
    (col("quantity") > 0)
    & (col("price") > 0)
)

summary = (
    valid
    .groupBy("customer")
    .agg(sum("total").alias("total_sales"))
    .orderBy(col("total_sales").desc())
)

summary.show()
```

**Marking:**

- Read JSON: 2 marks.
- Calculate `total`: 2 marks.
- Correct positive-value filter: 3 marks.
- Group and aggregate: 3 marks.
- Sort descending: 2 marks.
- Explain DataFrame choice and avoid full input collection: 3 marks.

**Explanation:** A DataFrame is suitable because the data is structured and the operations are relational. Spark can optimise the query plan. The computation remains distributed; there is no need to collect the complete input to the driver. `show()` is used here to display a limited sample of the final result.

**Final score interpretation**

- **85–100:** Strong applied Spark skills.
- **70–84:** Good; revisit expression syntax and aggregation details.
- **50–69:** Practise calculations and distributed-versus-local processing.
- **Below 50:** Review DataFrames, keyed aggregation and performance guidelines.

These bands are revision guidance, not official university grade boundaries.

# Practice Exam 3 — Comprehensive Integration and Exam Traps

Writing

# T5 — Practice Exam 3: Comprehensive Mock Examination

**Course:** UAB 106566 — Data Management\
**Academic year:** 2026/2027\
**Duration:** 120 minutes\
**Total:** 100 marks

## Instructions

- Answer every question.
- Explain the reasoning behind architectural and performance recommendations.
- Treat code as PySpark unless otherwise stated.
- Assume conventional RDD semantics and the classic DStream API for legacy streaming questions.
- Explicitly distinguish facts guaranteed by the code from behaviour that may depend on execution or partition ordering.

## Part A — Multiple choice (15 marks, 15 minutes)

Each question is worth 3 marks. Select one answer.

### Q1. Which statement about Spark's performance relative to Hadoop MapReduce is most accurate?

A. Spark is always exactly 100 times faster.\
B. Spark can reduce repeated disk I/O through in-memory reuse, but performance depends on the workload and configuration.\
C. Spark never writes intermediate data to disk.\
D. Spark does not use distributed execution stages.

### Q2. Which statement about classic Spark Streaming is correct?

A. A DStream is a single RDD that permanently contains every event received.\
B. A DStream represents a sequence of RDDs associated with batch intervals.\
C. A StreamingContext is a SQL aggregation function.\
D. Classic Spark Streaming cannot use RDD transformations.

### Q3. What does the CAP theorem say about a distributed system during a network partition?

A. It can always guarantee both strict consistency and availability.\
B. It must stop all computation permanently.\
C. It cannot guarantee both strict consistency and availability simultaneously during the partition.\
D. It applies only to graph databases.

### Q4. Which statement correctly compares RDDs and DataFrames?

A. RDDs always have named columns and a fixed schema.\
B. DataFrames are distributed structured tables that support query optimisation.\
C. DataFrames are necessarily local Python objects.\
D. RDDs cannot be processed in parallel.

### Q5. Which description of fault tolerance is most accurate?

A. Lineage guarantees recovery even if every original data source is permanently unavailable.\
B. Spark never loses partitions.\
C. Lineage can help recompute lost partitions when the required source data and dependencies remain available.\
D. Caching permanently replicates every record to every worker.

## Part B — Architecture, reliability and NoSQL (20 marks, 25 minutes)

### Q6. Explain an end-to-end Spark job (8 marks)

A PySpark application reads a large text file, filters lines containing `"AI"`, counts the matching lines and writes matching lines to distributed output.

Describe:

1. The roles of the driver and executors. (2 marks)
2. Why transformations can be defined before execution begins. (2 marks)
3. How the action or actions cause work to execute. (2 marks)
4. How lineage may help recover a lost partition. (2 marks)

### Q7. ACID, BASE and CAP (7 marks)

1. Expand ACID and briefly explain its four properties. (4 marks)
2. Expand BASE. (1 mark)
3. Explain the CAP trade-off during a network partition. (2 marks)

### Q8. Choose a NoSQL model (5 marks)

Match each scenario to a suitable model and justify your answer.

1. A cache of user sessions accessed by identifier.
2. Records containing variable fields and nested JSON documents.
3. A social network where relationships between users are central.

Choose from key-value, document and graph databases. Give one plausible example technology for each.

## Part C — Integrated RDD practical (25 marks, 30 minutes)

### Q9. Word-count pipeline with deterministic ranking (10 marks)

Write a complete PySpark RDD solution that:

1. Reads `data/quijote.txt`.
2. Converts each line to lowercase.
3. Extracts word-like tokens using a regular expression.
4. Counts each token.
5. Retrieves the three most frequent words, breaking ties alphabetically.

Explain why the word-count result is distributed and why the ranking key works.

### Q10. Interpret an RDD program (8 marks)

Given:

```
rdd = sc.parallelize([1, 2, 3, 4, 4])

a = rdd.map(lambda x: x + 1)
b = a.filter(lambda x: x % 2 == 0)
c = b.distinct()

print(c.count())
print(sorted(c.collect()))
```

Answer:

1. What values does `a` represent? (2 marks)
2. What values does `b` represent? (2 marks)
3. What values does `c` represent? (2 marks)
4. What do the two actions return? (2 marks)

### Q11. Narrow versus wide dependencies (7 marks)

A student claims:

> “Every transformation is a narrow transformation because transformations are lazy. Shuffles happen only when an action is called.”

Explain why this is incorrect. Use `map()`, `filter()` and `reduceByKey()` in your answer.

## Part D — DataFrames and performance diagnosis (20 marks, 25 minutes)

### Q12. Build a DataFrame query (10 marks)

Assume a DataFrame `transactions` contains:

- `customer`
- `product`
- `quantity`
- `price`

Write code that:

1. Calculates `total = quantity * price`.
2. Filters to positive quantities and prices.
3. Groups by customer.
4. Calculates total sales and maximum unit price.
5. Orders customers by total sales descending.

Explain the purpose of `withColumn()`, `where()`, `groupBy()` and `agg()`.

### Q13. Performance review (10 marks)

Review this code:

```
words = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
)

local_words = words.collect()

grouped = words.groupByKey()

cached = grouped.cache()

print(cached.count())
print(cached.count())
```

Identify **five distinct concerns or misconceptions** in this program. For each, propose an improvement or explain the actual behaviour.

## Part E — Streaming and comprehensive concepts (20 marks, 25 minutes)

### Q14. Classic Spark Streaming (10 marks)

Consider:

```
sc = SparkContext(appName="StreamCount")
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
2. What is a DStream? (2 marks)
3. Explain the processing pipeline. (3 marks)
4. What do `start()` and `awaitTermination()` do? (2 marks)
5. Give two practical limitations or considerations for this example. (2 marks)

### Q15. Structured Streaming and final misconceptions (10 marks)

Answer briefly:

1. Give two differences between DStreams and Structured Streaming. (3 marks)
2. Why is it incorrect to claim that Structured Streaming always processes every event individually in continuous mode? (2 marks)
3. Why does caching not guarantee that no recomputation will ever occur? (2 marks)
4. Explain why a global sort can be expensive. (1 mark)
5. Explain why the claim “`takeOrdered(3)` returns the three largest elements” is incorrect by default. (2 marks)

---

# SOLUTIONS AND MARKING SCHEME

## Part A — Answers (15 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| Q1 | B | In-memory reuse can reduce disk I/O, but the performance benefit is workload-dependent. |
| Q2 | B | A DStream is a time-ordered sequence of RDDs. |
| Q3 | C | During a partition, a distributed system cannot guarantee both strict consistency and availability. |
| Q4 | B | DataFrames offer structured operations and Spark SQL query optimisation. |
| Q5 | C | Lineage enables recomputation when required dependencies and source data are available. |

Each correct answer earns 3 marks.

**Common traps:** Treating benchmark figures as guarantees; assuming lineage is magic recovery independent of source availability; confusing a stream with a single RDD.

## Part B — Model answers (20 marks)

### Q6. Explain an end-to-end Spark job (8 marks)

1. **Driver and executors (2 marks):** The driver coordinates the application and schedules tasks. Executors process partitions and may cache intermediate data.
2. **Transformations (2 marks):** `filter()` and similar operations define a computation graph and are normally evaluated lazily.
3. **Actions (2 marks):** `count()` requests a count; `saveAsTextFile()` requests distributed output. Each triggers the computation required for its result or output. If both actions depend on the same uncached filtering computation, Spark may recompute that work.
4. **Lineage (2 marks):** If a partition is lost, Spark may reconstruct it from the original file and recorded dependencies, provided the source and necessary dependencies remain available.

Example:

```
matching = lines.filter(lambda line: "AI" in line)

print(matching.count())
matching.saveAsTextFile("output/matching-lines")
```

The two actions have different purposes: one returns a count to the driver; the other writes distributed output.

### Q7. ACID, BASE and CAP (7 marks)

**ACID (4 marks):**

- **Atomicity:** A transaction succeeds as a unit or is rolled back as a unit.
- **Consistency:** A transaction preserves defined integrity rules.
- **Isolation:** Concurrent transactions are controlled so that their interactions satisfy the chosen isolation guarantees.
- **Durability:** Committed changes survive failures according to the system's guarantees.

**BASE (1 mark):**

- Basically Available.
- Soft state.
- Eventual consistency.

BASE is a conceptual approach associated with some distributed systems, not a requirement that every NoSQL database must follow.

**CAP (2 marks):**

During a network partition, a distributed system cannot guarantee both strict consistency and availability simultaneously. A system may reject or delay some requests to preserve consistency, or continue serving requests while permitting temporary inconsistency.

**Common trap:** CAP is not simply a claim that a system can permanently choose any two of three independent properties under all circumstances. The trade-off concerns behaviour during a network partition.

### Q8. Choose a NoSQL model (5 marks)

1. **User session cache — key-value (2 marks):** Session identifiers map naturally to stored values. Redis is a plausible example.
2. **Variable nested JSON records — document (1.5 marks):** A document store supports records with flexible fields and nested structures. MongoDB is a plausible example.
3. **Social network relationships — graph (1.5 marks):** A graph model represents users as nodes and relationships as edges.

Other well-justified technology examples can receive credit.

## Part C — Model answers (25 marks)

### Q9. Word-count pipeline (10 marks)

```
import re

lines = sc.textFile("data/quijote.txt")

word_counts = (
    lines
    .flatMap(
        lambda line: re.findall(r"\b\w+\b", line.lower())
    )
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)

print(top_three)
```

**Marking:**

- Read input: 1 mark.
- Lowercase and regex tokenisation: 3 marks.
- Map tokens to `(word, 1)`: 2 marks.
- Aggregate by key: 2 marks.
- Correct top-three ordering and explanation: 2 marks.

The word-count RDD remains distributed. `reduceByKey()` combines counts across partitions; no complete list of input tokens needs to be transferred to the driver.

The key `(-pair[1], pair[0])` sorts by descending frequency and ascending word for ties.

**Exam trap:** The exact token counts depend on the source file and tokenisation rules. Do not invent frequencies without executing the code on the specified data.

### Q10. Interpret an RDD program (8 marks)

Original values:

```
[1, 2, 3, 4, 4]
```

After adding one:

```
a = [2, 3, 4, 5, 5]
```

After retaining even values:

```
b = [2, 4]
```

After removing duplicates:

```
c = [2, 4]
```

The actions return:

```
2
[2, 4]
```

**Marking:** 2 marks for each correct stage and output.

The exact iteration order of a distributed RDD is not guaranteed in general. The explicit `sorted()` ensures ascending order in the printed local result.

**Trap:** `distinct()` removes duplicates; `filter()` does not.

### Q11. Narrow versus wide dependencies (7 marks)

The student's statement confuses execution timing with dependency structure.

- `map()` is typically narrow because each output element depends on an input element in the same partition. (2 marks)
- `filter()` is typically narrow because it retains or discards elements locally. (2 marks)
- `reduceByKey()` is wide overall because values sharing a key can occur in different partitions and must be combined across them. (2 marks)
- Lazy evaluation does not eliminate the possibility of a shuffle. Spark determines the necessary execution plan and performs required shuffle work when an action triggers the relevant computation. (1 mark)

A shuffle may introduce network transfer, serialisation and additional execution stages.

## Part D — Model answers (20 marks)

### Q12. Build a DataFrame query (10 marks)

```
from pyspark.sql.functions import col, sum, max

with_total = transactions.withColumn(
    "total",
    col("quantity") * col("price")
)

valid = with_total.where(
    (col("quantity") > 0)
    & (col("price") > 0)
)

summary = (
    valid
    .groupBy("customer")
    .agg(
        sum("total").alias("total_sales"),
        max("price").alias("maximum_unit_price")
    )
    .orderBy(col("total_sales").desc())
)

summary.show()
```

**Marking:**

- Calculated column: 2 marks.
- Correct filtering conditions: 2 marks.
- Grouping: 2 marks.
- Both aggregate expressions: 2 marks.
- Descending sort: 2 marks.

Explanation:

- `withColumn()` returns a DataFrame with the specified column added or replaced.
- `where()` filters rows using a condition.
- `groupBy()` defines the grouping key.
- `agg()` applies aggregate functions to each group.

These operations construct a DataFrame computation; they do not normally execute every step immediately. The final `show()` triggers the computation needed to display the requested rows.

### Q13. Performance review (10 marks)

Award 2 marks for each well-explained concern and improvement, up to five distinct points.

**Concern 1 — Unsafe****`collect()`****:**

```
local_words = words.collect()
```

This transfers all word pairs to the driver and can exhaust driver memory. Remove it if it is unnecessary.

**Concern 2 — Duplicate work is not removed by collecting:**

`local_words` is not used in the subsequent computation. The collection wastes resources and does not improve the distributed `groupByKey()` operation. Delete it.

**Concern 3 —****`groupByKey()`****is inefficient for summation:**

If the goal is word count, use:

```
counts = words.reduceByKey(lambda a, b: a + b)
```

This can aggregate locally before the shuffle rather than gathering all values for each word.

**Concern 4 — Caching occurs after an expensive wide operation:**

```
cached = grouped.cache()
```

Caching can be useful if the grouped result is reused, but the program should not assume that caching eliminates the initial shuffle or immediately computes all partitions.

**Concern 5 — The result is only counted:**

```
print(cached.count())
```

This returns the number of distinct keys in the grouped RDD, not the frequency of each word. The second `count()` returns the same count if the underlying input and computation are unchanged, and caching may help avoid recomputation of available cached partitions.

An appropriate word-count solution is:

```
counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

print(counts.take(10))
```

**Additional acceptable observations:** `groupByKey()` can consume substantial memory for high-frequency keys; cache storage is finite; `take(10)` is useful for inspection but does not guarantee a meaningful or sorted sample.

## Part E — Model answers (20 marks)

### Q14. Classic Spark Streaming (10 marks)

1. **Batch interval: 2 seconds (1 mark).**
2. **DStream (2 marks):** A time-ordered sequence of RDDs, each associated with a batch interval.
3. **Pipeline (3 marks):** The socket source supplies lines; `flatMap()` extracts words; `map()` creates `(word, 1)` pairs; `reduceByKey()` aggregates counts for each batch. `pprint()` displays results.
4. **Lifecycle (2 marks):** `start()` begins streaming execution. `awaitTermination()` keeps the application waiting until the streaming computation terminates or is stopped.
5. **Practical considerations (2 marks):** Any two well-explained points, such as:
   - A TCP server must be running and accessible on port 9999.
   - A socket source is not automatically a reliable production input source.
   - A two-second interval trades waiting time against scheduling overhead.
   - If processing cannot keep up with input, the application may fall behind.
   - Production deployments may need checkpointing and suitable recovery guarantees.

### Q15. Structured Streaming and final misconceptions (10 marks)

**1\. DStreams versus Structured Streaming (3 marks)**

- DStreams represent streams as sequences of RDDs; Structured Streaming uses structured DataFrames/Datasets. (1.5 marks)
- Structured Streaming uses Spark SQL's query-planning and optimisation framework and offers a declarative API. (1.5 marks)

**2\. Continuous processing (2 marks)**

Structured Streaming commonly uses micro-batch execution. Continuous processing is available only for supported configurations and operations, so it is incorrect to claim every query processes each event individually in continuous mode.

**3\. Caching and recomputation (2 marks)**

Caching marks data for reuse; it does not guarantee that all partitions are immediately computed or remain permanently stored. Eviction, executor loss or unavailable cached partitions can require recomputation.

**4\. Global sorting (1 mark)**

A global sort may redistribute data across partitions and coordinate ordering, introducing shuffle and execution costs.

**5\.****`takeOrdered(3)`****(2 marks)**

`takeOrdered(3)` returns the three smallest elements according to the specified ordering by default. To find the largest counts, negate the count in the key or provide an appropriate descending ordering.

Example:

```
top_three = counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

---

## Final performance assessment

| Score | Interpretation | Recommended action |
| --- | --- | --- |
| 85–100 | Strong, integrated understanding | Review edge cases and practise under time pressure. |
| 70–84 | Good understanding | Focus on optimisation and precise explanations. |
| 50–69 | Developing understanding | Revisit code tracing, aggregation and execution semantics. |
| Below 50 | Significant gaps | Relearn core concepts before attempting another timed paper. |

These score bands are for self-assessment and are not official university grade boundaries.

## How to use the three exams

1. Attempt each paper without the solutions and set a timer for 120 minutes.
2. Mark your work using the rubrics. Award yourself partial credit only when your reasoning or code is genuinely correct.
3. Record every lost mark under one of these categories: conceptual misunderstanding, code error, calculation error, performance misconception or incomplete explanation.
4. Reattempt incorrect questions without looking at the answer key.
5. Prioritise word count, `reduceByKey()` versus `groupByKey()`, DataFrame aggregation, shuffles and lazy evaluation if those are recurring weaknesses.

Important: These are newly constructed practice exams based on the T5 material you supplied. They are not official UAB examinations, and their coverage cannot guarantee that every topic or detail from the original lecture slides will appear in this form.
