Absolutely. We can do everything directly here, without creating a file. You can copy the Markdown below and paste it into GitHub.

I have organized the 60 Spark MCQs into three exams of 20 questions each, following the structure we used for the Redis practice bank.

- Four options (A–D) per question.
- Exactly one correct answer.
- Correct answer and explanation immediately after each question.
- A consolidated answer key at the end.
- Extra emphasis on the lecturer's four explicit review questions.
- Questions covering conceptual knowledge, code interpretation, practical applications and common exam traps.

# T6 — Apache Spark: 60 MCQ Practice Bank

Course: Big Data / Distributed Data Processing\
Chapter: T6 — Apache Spark\
Purpose: Exam preparation based on the supplied Spark review material.

# Exam 1 — Spark Fundamentals, Transformations, Actions and Shuffles

### Q1. Which statement best describes a shuffle in Apache Spark?

A. It converts an RDD into a local Python list.\
B. It redistributes data across partitions so related records can be processed together.\
C. It permanently stores every partition in memory.\
D. It modifies an immutable RDD in place.

Correct answer: B

Explanation: A shuffle redistributes data across partitions, often across worker nodes. It is commonly necessary when an output partition depends on records from multiple input partitions, such as when values with the same key must be brought together.

### Q2. Which group contains only operations that commonly trigger a shuffle?

A. `map()`, `filter()`, `flatMap()`\
B. `first()`, `count()`, `take()`\
C. `distinct()`, `reduceByKey()`, `groupByKey()`\
D. `collect()`, `foreach()`, `saveAsTextFile()`

Correct answer: C

Explanation: `distinct()`, `reduceByKey()` and `groupByKey()` are wide transformations in common execution plans and commonly require data redistribution. Whether an operation actually shuffles can depend on partitioning and execution-plan optimizations.

### Q3. Why is `reduceByKey()` generally preferred over `groupByKey()` when calculating totals?

A. It returns all values in a Python list.\
B. It can combine values locally before data is redistributed.\
C. It avoids all network communication in every situation.\
D. It is an action that immediately returns results to the driver.

Correct answer: B

Explanation: `reduceByKey()` can perform local aggregation within partitions before the shuffle, reducing the amount of data sent across the network. `groupByKey()` groups all values for a key into an iterable without performing that equivalent reduction.

### Q4. What is the main difference between `reduceByKey()` and `groupByKey()`?

A. `reduceByKey()` aggregates values into one result per key; `groupByKey()` collects the values for each key.\
B. `reduceByKey()` sorts keys; `groupByKey()` removes duplicate keys.\
C. `reduceByKey()` is an action; `groupByKey()` is a transformation.\
D. They always produce identical data structures.

Correct answer: A

Explanation: `reduceByKey()` combines values for each key using a reduction function. `groupByKey()` produces a grouped collection of values for each key. For sums and counts, `reduceByKey()` is usually the more efficient choice.

### Q5. What happens when `collect()` is called on an RDD?

A. It returns only the number of elements.\
B. It writes the RDD to distributed storage.\
C. It triggers the required computation and returns all elements to the driver.\
D. It creates a new RDD without executing any computation.

Correct answer: C

Explanation: `collect()` is an action. It triggers the transformations needed to produce the result and transfers every element to the driver as a local collection.

### Q6. What is the principal risk of calling `collect()` on a very large RDD?

A. The RDD becomes mutable.\
B. The driver may run out of memory because all elements are brought to it.\
C. Spark automatically deletes the source data.\
D. The cluster manager stops allocating executors.

Correct answer: B

Explanation: The complete result must fit in the driver's available memory. Collecting too much data can also create substantial network traffic and may crash the driver through memory exhaustion.

### Q7. Which of the following is a transformation?

A. `count()`\
B. `collect()`\
C. `saveAsTextFile()`\
D. `filter()`

Correct answer: D

Explanation: `filter()` creates a new RDD containing elements that satisfy a condition. `count()`, `collect()` and `saveAsTextFile()` are actions.

### Q8. Which of the following is an action?

A. `map()`\
B. `flatMap()`\
C. `take(10)`\
D. `distinct()`

Correct answer: C

Explanation: `take(10)` requests up to ten elements and triggers the computation needed to obtain them. The other options are transformations that define new RDDs.

### Q9. What does lazy evaluation mean in Spark?

A. Spark never executes transformations.\
B. Spark generally records transformations and executes them when an action requires a result.\
C. Spark executes every transformation immediately.\
D. Spark stores all input data permanently in memory.

Correct answer: B

Explanation: Transformations are generally lazy. Spark builds the computation needed and executes it when an action such as `collect()`, `count()` or `saveAsTextFile()` is invoked.

### Q10. Consider the following code:

```
rdd.map(f).filter(g).collect()
```

Which operation triggers execution of the preceding transformation chain?

A. `map()`\
B. `filter()`\
C. `collect()`\
D. Neither transformation nor action executes it.

Correct answer: C

Explanation: `map()` and `filter()` define transformations. `collect()` is the action that triggers the required computation and returns the results to the driver.

### Q11. Which statement about Spark transformations is correct?

A. They modify the original RDD in place.\
B. They generally create a new RDD and are evaluated lazily.\
C. They always return a local Python list.\
D. They always write their results to storage.

Correct answer: B

Explanation: RDDs are immutable. A transformation creates a new RDD representing the result, while the original RDD remains unchanged. Transformations are generally evaluated lazily.

### Q12. Which operation returns the number of elements in an RDD?

A. `first()`\
B. `count()`\
C. `collect()`\
D. `take()`

Correct answer: B

Explanation: `count()` is an action that returns the number of elements. It returns a count rather than transferring the complete dataset to the driver.

### Q13. Which option is the most appropriate when only the first ten elements are needed?

A. `collect()`\
B. `take(10)`\
C. `groupByKey()`\
D. `saveAsTextFile()`

Correct answer: B

Explanation: `take(10)` returns at most ten elements. It is preferable to collecting an entire large RDD when only a small sample of the result is needed.

### Q14. Which statement about `saveAsTextFile()` is correct?

A. It is a transformation that creates a new RDD.\
B. It is an action that writes the RDD's elements to text files.\
C. It returns every element to the driver.\
D. It removes duplicate records.

Correct answer: B

Explanation: `saveAsTextFile()` is an action. It triggers execution and writes the output to the specified storage location; it does not return the entire dataset to the driver.

### Q15. Which statement best distinguishes narrow and wide transformations?

A. Narrow transformations always use disk; wide transformations always use memory.\
B. Narrow transformations depend on one input partition per output partition; wide transformations may depend on multiple input partitions.\
C. Narrow transformations are actions; wide transformations are transformations.\
D. Wide transformations never involve network communication.

Correct answer: B

Explanation: A narrow dependency allows an output partition to be computed from a single input partition. A wide dependency can require data from multiple input partitions and commonly causes a shuffle.

### Q16. Which operation is a typical narrow transformation?

A. `groupByKey()`\
B. `distinct()`\
C. `map()`\
D. `reduceByKey()`

Correct answer: C

Explanation: `map()` typically processes each partition independently and does not require redistribution of records. The other operations commonly have wide dependencies and may require a shuffle.

### Q17. Why are shuffles generally expensive?

A. They make RDDs mutable.\
B. They can involve network communication, disk I/O and additional memory use.\
C. They prevent Spark from using executors.\
D. They always require all data to be collected by the driver.

Correct answer: B

Explanation: Shuffles exchange data between partitions and potentially between machines. This can involve network transfer, disk operations, memory overhead and additional execution time.

### Q18. Which statement about shuffle behavior is the most accurate?

A. Every `join()` must shuffle in every possible execution plan.\
B. An operation's shuffle behavior can depend on partitioning and execution-plan optimizations.\
C. `map()` always triggers a shuffle.\
D. Shuffles occur only when using DataFrames, never with RDDs.

Correct answer: B

Explanation: Operations such as joins commonly shuffle, but a shuffle is not inevitable in every plan. Suitable existing partitioning or optimizations can sometimes avoid redistributing data.

### Q19. Which reduction-function properties are important for safe distributed aggregation with `reduceByKey()`?

A. The function must always return a string.\
B. The function must be associative and commutative.\
C. The function must modify its input values.\
D. The function must use the driver.

Correct answer: B

Explanation: Distributed aggregation can combine values in different groupings and orders. Associativity and commutativity allow the result to remain valid regardless of the order in which partial results are combined.

### Q20. Which pair consists of two actions?

A. `map()` and `filter()`\
B. `flatMap()` and `distinct()`\
C. `count()` and `first()`\
D. `reduceByKey()` and `groupByKey()`

Correct answer: C

Explanation: Both `count()` and `first()` are actions. They request computed results, whereas the other options contain transformations.

# Exam 2 — RDDs, Distributed Processing, Architecture and PySpark

### Q21. What does RDD stand for?

A. Remote Data Directory\
B. Resilient Distributed Dataset\
C. Replicated Database Driver\
D. Randomly Divided Data

Correct answer: B

Explanation: RDD stands for Resilient Distributed Dataset. It is an immutable distributed collection that Spark can process in parallel.

### Q22. Which set contains core RDD properties described in the material?

A. Mutable, local, sequential and permanently replicated\
B. Immutable, distributed, parallelizable and resilient through lineage\
C. Structured only as named columns and optimized only by Catalyst\
D. Stored exclusively on the driver

Correct answer: B

Explanation: RDDs are immutable and distributed across partitions. They support parallel processing, and lost partitions can often be recomputed using lineage information.

### Q23. Why are RDDs described as resilient?

A. Every RDD is always copied to every worker.\
B. Lost partitions can often be recomputed from the transformations that created the RDD.\
C. RDDs cannot lose data under any circumstances.\
D. The driver stores a complete duplicate of every partition.

Correct answer: B

Explanation: RDD fault recovery relies on lineage: Spark tracks the dependencies and transformations needed to reconstruct lost partitions. Resilience does not mean every partition is automatically replicated in memory.

### Q24. What happens to `rdd1` in the following code?

```
rdd1 = sc.parallelize([1, 2, 3])rdd2 = rdd1.map(lambda x: x * 2)
```

A. `rdd1` is replaced by `[2, 4, 6]`.\
B. `rdd1` remains unchanged; `rdd2` represents the transformed data.\
C. Both RDDs become local Python lists.\
D. The code immediately collects `rdd2` on the driver.

Correct answer: B

Explanation: RDDs are immutable. `map()` defines a new RDD and does not modify the original one. The computation is generally lazy until an action is called.

### Q25. Why is distributed processing useful for very large datasets?

A. It guarantees that every operation is free of network costs.\
B. It divides data and computation across machines, combining their resources.\
C. It ensures that all data fits in the memory of one computer.\
D. It eliminates the need for storage.

Correct answer: B

Explanation: Distributed processing uses multiple machines to process partitions in parallel. This can handle datasets that exceed the memory or computational capacity of a single computer.

### Q26. What is horizontal scaling?

A. Increasing the RAM of one machine only\
B. Increasing the CPU speed of one machine only\
C. Adding machines or worker nodes to a system\
D. Moving all data to the driver

Correct answer: C

Explanation: Horizontal scaling increases aggregate system resources by adding machines. Vertical scaling increases the resources of an individual machine.

### Q27. Which statement correctly describes vertical scaling?

A. Adding more worker machines to a cluster\
B. Increasing the resources of one machine\
C. Dividing one RDD into more keys\
D. Replacing all transformations with actions

Correct answer: B

Explanation: Vertical scaling means increasing the resources of a single machine, such as its CPU or memory. Adding machines is horizontal scaling.

### Q28. Why can Spark be faster than Hadoop MapReduce for iterative workloads?

A. Spark never performs disk I/O.\
B. Spark can cache reusable intermediate datasets in memory and avoid repeated disk reads and writes.\
C. Spark always uses fewer machines.\
D. Spark does not need to exchange data between partitions.

Correct answer: B

Explanation: Caching or persisting reusable intermediate datasets can reduce repeated computation and disk I/O. This is particularly useful in iterative machine learning and repeated analysis.

### Q29. Which qualification about Spark's performance advantage is correct?

A. Spark is guaranteed to be faster for every workload.\
B. Spark's performance depends on workload, data size, memory, shuffle costs, storage and configuration.\
C. Spark is always slower than Hadoop MapReduce.\
D. Spark's performance is independent of available memory.

Correct answer: B

Explanation: Spark can offer substantial advantages, especially for iterative workloads, but no speedup is universal. Actual performance depends on the workload and system configuration.

### Q30. What is the role of the Spark driver?

A. It only stores input files.\
B. It runs the application logic and coordinates execution.\
C. It is the same thing as every executor.\
D. It performs all tasks locally and prevents parallel execution.

Correct answer: B

Explanation: The driver runs the main application and coordinates work. It requests resources and organizes jobs, stages and tasks that executors run.

### Q31. What is the role of executors?

A. They allocate resources for all applications in the cluster.\
B. They run tasks on worker machines and can cache or persist data.\
C. They replace the driver application.\
D. They only display DataFrame schemas.

Correct answer: B

Explanation: Executors are processes allocated to a Spark application. They execute tasks on worker machines and can store cached or persisted data.

### Q32. What is the role of a cluster manager?

A. It allocates resources for Spark applications.\
B. It converts every RDD into a DataFrame.\
C. It performs all Python transformations on the driver.\
D. It defines the columns in a JSON file.

Correct answer: A

Explanation: The cluster manager manages resource allocation for applications. Examples mentioned in the material include Spark Standalone, YARN and Kubernetes.

### Q33. Which list contains examples of Spark cluster managers mentioned in the material?

A. Catalyst, MLlib, GraphX\
B. Standalone, YARN, Kubernetes\
C. RDD, DStream, DataFrame\
D. Python, JSON, SQL

Correct answer: B

Explanation: Spark can use Standalone, YARN or Kubernetes for cluster resource management. Catalyst, MLlib and GraphX are associated with Spark's ecosystem rather than cluster managers.

### Q34. Which sequence best represents the simplified execution flow of a Spark application?

A. Executors start the driver, then the driver creates the cluster manager.\
B. The application starts on the driver, resources are allocated, executors run tasks, and results are returned or stored.\
C. The cluster manager collects all records to the driver before any task starts.\
D. Every worker independently starts a separate Spark application.

Correct answer: B

Explanation: The driver coordinates the application and requests resources. Executors are allocated, Spark divides work into stages and tasks, and executors process partitions in parallel.

### Q35. What is PySpark?

A. A Python API for Apache Spark\
B. A standalone replacement for the Spark cluster manager\
C. A local-only version of Pandas\
D. A file format used by Spark SQL

Correct answer: A

Explanation: PySpark allows developers to write Spark applications using Python. It supports RDD processing, DataFrames, SQL operations, machine learning and streaming APIs.

### Q36. Which statement about PySpark is correct?

A. It is ordinary Python that always runs on a single machine.\
B. It lets developers describe computations that Spark can execute across a cluster.\
C. It cannot use DataFrames.\
D. It is only used to inspect JSON schemas.

Correct answer: B

Explanation: PySpark is a Python interface to Spark's distributed processing capabilities. It is not merely ordinary single-machine Python execution.

### Q37. Which method creates an RDD by distributing a local collection?

A. `sc.textFile()`\
B. `sc.parallelize()`\
C. `spark.read.json()`\
D. `df.printSchema()`

Correct answer: B

Explanation: `sc.parallelize()` distributes elements from a collection already available in the driver into RDD partitions.

### Q38. Which method is commonly used to create an RDD from a text file?

A. `sc.textFile()`\
B. `sc.parallelize()`\
C. `df.selectExpr()`\
D. `df.orderBy()`

Correct answer: A

Explanation: `textFile()` reads a supported text input and creates an RDD whose elements represent lines from the file.

### Q39. Which statement about RDD partitions is correct?

A. The number of partitions must always equal the number of worker nodes.\
B. Partitions are units of distributed work, and their number is not necessarily the number of worker nodes.\
C. Every partition must contain exactly one record.\
D. Partitions exist only in the driver.

Correct answer: B

Explanation: Spark divides distributed data into partitions that can be processed in parallel. The number of partitions is influenced by input and configuration and should not be assumed to equal the number of workers.

### Q40. Which statement best describes the relationship between the driver and the cluster manager?

A. The driver and cluster manager are necessarily the same process.\
B. The driver coordinates the Spark application, while the cluster manager allocates resources.\
C. The cluster manager performs every task and the driver is unused.\
D. The driver is always a dedicated master node of the cluster manager.

Correct answer: B

Explanation: These roles are distinct. The driver coordinates a particular application; the cluster manager allocates resources. The driver is not necessarily a dedicated master node of the cluster manager.

# Exam 3 — RDD Coding, DataFrames and Streaming

### Q41. What is the difference between `map()` and `flatMap()`?

A. `map()` removes duplicates; `flatMap()` sorts elements.\
B. `map()` produces one output per input; `flatMap()` can produce zero, one or many outputs and flattens them.\
C. `map()` is an action; `flatMap()` is a transformation.\
D. Both always produce exactly one output per input.

Correct answer: B

Explanation: `map()` applies a function once to each input element and retains one output element for each input. `flatMap()` can emit multiple outputs or none, flattening the resulting collections.

### Q42. Given the following code, what is the result?

```
rdd = sc.parallelize([1, 2, 3])rdd.map(lambda x: [x, x * 10]).collect()
```

A. `[1, 10, 2, 20, 3, 30]`\
B. `[[1, 10], [2, 20], [3, 30]]`\
C. `[1, 2, 3]`\
D. `[[1], [2], [3]]`

Correct answer: B

Explanation: `map()` returns one output per input. Since the function returns a list for each input, the RDD contains three list elements; it does not flatten those lists.

### Q43. Given the following code, what is the result?

```
rdd = sc.parallelize([1, 2, 3])rdd.flatMap(lambda x: [x, x * 10]).collect()
```

A. `[[1, 10], [2, 20], [3, 30]]`\
B. `[1, 2, 3]`\
C. `[1, 10, 2, 20, 3, 30]`\
D. `[10, 20, 30]`

Correct answer: C

Explanation: `flatMap()` flattens the collections produced for each input. The two values produced for each of the three input elements form one sequence of six elements.

### Q44. In a word-count program, what is the purpose of `flatMap(lambda line: line.split())`?

A. It groups all lines by their first character.\
B. It converts each line into individual words in one flattened RDD.\
C. It counts the number of lines.\
D. It sorts words alphabetically.

Correct answer: B

Explanation: Each line can contain multiple words. `flatMap()` emits the words from each line and flattens them into a single RDD of word elements.

### Q45. In the classic word-count pattern, why is each word mapped to `(word, 1)`?

A. To assign an initial count of one to each occurrence.\
B. To remove punctuation automatically.\
C. To sort the words alphabetically.\
D. To convert the RDD into a DataFrame.

Correct answer: A

Explanation: Mapping each word to a key-value pair `(word, 1)` represents each occurrence as one count. `reduceByKey()` can then add the counts for identical words.

### Q46. Which sequence correctly describes the classic RDD word-count pattern?

A. `filter()` → `collect()` → `sortByKey()`\
B. `flatMap()` words → map to `(word, 1)` → `reduceByKey()`\
C. `groupByKey()` → `printSchema()` → `take()`\
D. `parallelize()` → `saveAsTextFile()` → `count()`

Correct answer: B

Explanation: `flatMap()` extracts individual words, `map()` assigns each occurrence a count of one, and `reduceByKey()` adds the counts for each word.

### Q47. What is a limitation of using `line.split()` directly for word counting?

A. It always performs a distributed join.\
B. It does not automatically normalize capitalization or remove punctuation.\
C. It cannot process more than one line.\
D. It returns a DataFrame instead of words.

Correct answer: B

Explanation: Simple splitting separates text on whitespace. It does not automatically make words lowercase or remove punctuation, so terms such as `"Mancha"` and `"mancha"` may be counted separately.

### Q48. In the supplied character-count example, what does the following calculate?

```
lines.map(lambda line: len(line)).sum()
```

A. The number of lines in the file\
B. The sum of the lengths of the line strings\
C. The number of distinct characters\
D. The number of words in the file

Correct answer: B

Explanation: `textFile()` creates an RDD of lines, `map()` calculates each line's length, and `sum()` adds those lengths. Line terminators removed by line-based input reading are not included.

### Q49. Why might the character-count example need adjustment if newline separators must be counted?

A. `map()` cannot calculate string lengths.\
B. Line-based input reading does not include the removed newline separators in each line string.\
C. `sum()` always removes newline characters.\
D. `textFile()` returns one integer per file.

Correct answer: B

Explanation: The supplied material notes that the calculation counts characters in the line strings but not newline separators removed by line-based input reading. If line terminators are part of the required count, they must be accounted for explicitly.

### Q50. How can the three most frequent words be retrieved using the supplied approach?

A. `word_counts.takeOrdered(3)` with no custom ordering in every case\
B. `word_counts.takeOrdered(3, key=lambda pair: (-pair[1], pair[0]))`\
C. `word_counts.first(3)`\
D. `word_counts.collect(3)`

Correct answer: B

Explanation: Negating the count orders higher frequencies first. The word provides an alphabetical tie-breaker. A plain `takeOrdered(3)` uses the default ordering and does not automatically mean the three most frequent words.

### Q51. What is a Spark DataFrame?

A. A local Python list of dictionaries only\
B. A distributed table with named columns and associated data types\
C. A sequence of RDDs used only by legacy streaming\
D. A cluster manager

Correct answer: B

Explanation: A Spark DataFrame is a distributed, structured table. It supports named columns and data types and can benefit from Spark SQL query optimization.

### Q52. What is a key difference between an RDD and a DataFrame?

A. An RDD is always local, while a DataFrame is always distributed.\
B. An RDD is a lower-level collection of objects; a DataFrame is a structured distributed table.\
C. DataFrames cannot be processed in parallel.\
D. RDDs support named columns, but DataFrames do not.

Correct answer: B

Explanation: RDDs provide a lower-level distributed collection abstraction. DataFrames organize distributed data into named columns and are often preferable for structured analysis.

### Q53. Which Spark component optimizes DataFrame and SQL query plans?

A. StreamingContext\
B. Catalyst optimizer\
C. SparkContext\
D. YARN

Correct answer: B

Explanation: Spark SQL's Catalyst optimizer is part of the optimization infrastructure used for structured DataFrame and SQL operations.

### Q54. Which API is generally preferred for structured data according to the supplied material?

A. DataFrame API\
B. Only low-level RDD functions\
C. DStream API\
D. `collect()` for every operation

Correct answer: A

Explanation: DataFrames provide a structured abstraction and allow Spark to optimize relational operations. RDDs remain useful when specialized low-level control is needed.

### Q55. Which code reads a JSON file into a DataFrame and displays its schema?

A. `df = sc.parallelize("summary.json"); df.count()`\
B. `df = spark.read.format("json").load("/data/summary.json"); df.printSchema()`\
C. `df = spark.read.textFile("/data/summary.json"); df.collect()`\
D. `df = spark.read.format("RDD").load("/data/summary.json"); df.schemaSort()`

Correct answer: B

Explanation: `spark.read.format("json").load(...)` reads JSON data into a DataFrame. `printSchema()` displays column names, data types and nested structure.

### Q56. What is the difference between `select()` and `selectExpr()`?

A. `select()` is an action, while `selectExpr()` is a transformation.\
B. `select()` works with column names and expression objects; `selectExpr()` accepts SQL-like expression strings.\
C. `select()` sorts rows; `selectExpr()` filters rows.\
D. `selectExpr()` can only select existing columns and cannot calculate expressions.

Correct answer: B

Explanation: Both methods select columns and can create derived columns. `select()` naturally uses column expressions, while `selectExpr()` accepts SQL-like strings such as `"quantity * price AS total"`.

### Q57. Which expression filters a DataFrame to rows where `quantity` is greater than 5?

A. `df.where(col("quantity") > 5)`\
B. `df.collect(col("quantity") > 5)`\
C. `df.groupBy(col("quantity") > 5)`\
D. `df.printSchema("quantity" > 5)`

Correct answer: A

Explanation: `where()` and `filter()` retain rows satisfying a condition. They return a new DataFrame rather than modifying the original DataFrame.

### Q58. Which code sorts a DataFrame by `price` from highest to lowest?

A. `df.orderBy(col("price").asc())`\
B. `df.orderBy(col("price").desc())`\
C. `df.filter(col("price").desc())`\
D. `df.groupBy("price").desc()`

Correct answer: B

Explanation: `orderBy()` sorts rows. The `desc()` expression specifies descending order; `asc()` specifies ascending order.

### Q59. What does the following code do?

```
df.groupBy("customer").agg(    sum("quantity").alias("total_quantity"))
```

A. It returns the original rows sorted by customer.\
B. It groups rows by customer and calculates total quantity for each customer.\
C. It removes the customer column.\
D. It collects every row to the driver without aggregation.

Correct answer: B

Explanation: `groupBy()` defines the groups, and `agg()` applies the requested aggregate to each group. Grouping and aggregation commonly involve a shuffle because records with the same grouping key must be brought together.

### Q60. Which statement correctly compares DStreams and Structured Streaming?

A. Both are identical APIs based only on local Python lists.\
B. DStreams represent streams as sequences of RDDs; Structured Streaming uses the DataFrame/Dataset structured-data model.\
C. Structured Streaming is the legacy RDD-based API, while DStreams are the modern DataFrame API.\
D. DStreams cannot process data in batches.

Correct answer: B

Explanation: The legacy Spark Streaming API uses DStreams, which represent continuous data as a sequence of RDDs processed in small batches. Structured Streaming uses Spark's structured DataFrame/Dataset model and is the preferred API for most new streaming applications.

# Consolidated Answer Key

| Question | Answer | Question | Answer | Question | Answer |
| -------- | ------ | -------- | ------ | -------- | ------ |
| 1        | B      | 21       | B      | 41       | B      |
| 2        | C      | 22       | B      | 42       | B      |
| 3        | B      | 23       | B      | 43       | C      |
| 4        | A      | 24       | B      | 44       | B      |
| 5        | C      | 25       | B      | 45       | A      |
| 6        | B      | 26       | C      | 46       | B      |
| 7        | D      | 27       | B      | 47       | B      |
| 8        | C      | 28       | B      | 48       | B      |
| 9        | B      | 29       | B      | 49       | B      |
| 10       | C      | 30       | B      | 50       | B      |
| 11       | B      | 31       | B      | 51       | B      |
| 12       | B      | 32       | A      | 52       | B      |
| 13       | B      | 33       | B      | 53       | B      |
| 14       | B      | 34       | B      | 54       | A      |
| 15       | B      | 35       | A      | 55       | B      |
| 16       | C      | 36       | B      | 56       | B      |
| 17       | B      | 37       | B      | 57       | A      |
| 18       | B      | 38       | A      | 58       | B      |
| 19       | B      | 39       | B      | 59       | B      |
| 20       | C      | 40       | B      | 60       | B      |

# Coverage and Study Priority

| Exam   | Questions | Main subjects                                                                                                                        |
| ------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Exam 1 | 1–20      | Shuffles, narrow/wide transformations, transformations versus actions, lazy evaluation, `reduceByKey()`, `groupByKey()`, `collect()` |
| Exam 2 | 21–40     | RDD properties, lineage, distributed processing, scaling, Spark performance, architecture, PySpark and RDD creation                  |
| Exam 3 | 41–60     | `map()`, `flatMap()`, word count, character count, top-three words, DataFrames, SQL expressions, aggregation and streaming           |

Important source distinction: The four lecturer-explicit review themes receive priority. The other questions are derived from the additional review questions, coding examples, quick-fire facts, common exam traps and study priorities in your supplied material.

The questions are designed to test the material you supplied, not to claim coverage of lecture content that was not included in the review document.
