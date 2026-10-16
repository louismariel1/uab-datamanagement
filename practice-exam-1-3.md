# TD1–TD3: Practice exam files

The six exam documents are provided below as copy-ready Markdown. You can save each code block under its indicated filename in your GitHub repository.

Coverage note: The detailed T0 summary is still missing, so the papers emphasize the supplied T1–T5 material and test general data-system foundations only where supported by that material. No unsupported T0-specific lecture details are assumed.

# TD1 — `practice-exam-1.md`

Writing

# Practice Exam 1 — Data Management

**Topics:** T0–T5\
**Duration:** 120 minutes\
**Total marks:** 100

## Instructions

- Answer all questions.
- Show your reasoning for design and coding questions.
- SQL questions use standard relational SQL unless noted otherwise.
- MongoDB questions use `mongosh` syntax.
- Redis questions use standard Redis commands.
- Spark questions use PySpark.
- Suggested times are indicative and add up to 120 minutes.

## Section A — Multiple choice (10 marks, 10 minutes)

Choose one answer per question. Each question is worth 2 marks.

**1.** Which data-modeling level describes business entities and relationships without implementation details?

A. Physical\
B. Conceptual\
C. Logical\
D. Storage

**2.** In a relational implementation of a one-to-many relationship, where is the foreign key normally placed?

A. On the one side\
B. On the many side\
C. In a separate table in every case\
D. Nowhere

**3.** Which SQL clause filters groups after aggregation?

A. `WHERE`\
B. `ORDER BY`\
C. `HAVING`\
D. `FROM`

**4.** Which MongoDB operator requires an array to contain all specified values?

A. `$in`\
B. `$all`\
C. `$size`\
D. `$exists`

**5.** Which Redis data type is most appropriate for a leaderboard with numerical scores?

A. List\
B. Set\
C. Hash\
D. Sorted Set

## Section B — Data modeling and transactions (12 marks, 14 minutes)

### Question 2 — Model and normalize (7 marks)

A university stores the following information in one table:

`EnrollmentRecord(StudentID, StudentName, CourseID, CourseTitle, InstructorID, InstructorName, Grade)`

Assume:

- A student can enroll in many courses.
- Each course has one instructor.
- Each enrollment has one grade.
- `(StudentID, CourseID)` uniquely identifies an enrollment.
- `StudentName` depends on `StudentID`.
- `CourseTitle` and `InstructorID` depend on `CourseID`.
- `InstructorName` depends on `InstructorID`.

**a.** Identify the primary key for an enrollment row. Explain why neither `StudentID` nor `CourseID` alone is sufficient. (2)

**b.** Identify one partial dependency and one transitive dependency. (2)

**c.** Propose a normalized design with primary and foreign keys. (3)

### Question 3 — ACID in context (5 marks)

An online shop transfers €80 from a customer's account to a merchant's account. A server failure occurs after the debit but before the credit completes.

**a.** Which ACID property should prevent a half-completed transfer from being committed? (1)

**b.** Which property concerns whether committed changes survive a later crash? (1)

**c.** Explain the difference between consistency and isolation in this scenario. (3)

## Section C — SQL (20 marks, 24 minutes)

Use this schema:

```
Students(student_id, name, city)
Enrollments(student_id, course_id, grade)
Courses(course_id, title, department)
```

`Students.student_id` and `Courses.course_id` are primary keys. `Enrollments` has a composite primary key `(student_id, course_id)` and foreign keys to both parent tables.

### Question 4 — Write queries (12 marks)

**a.** Return the names of students from Barcelona in alphabetical order. (2)

**b.** Return every student and their course ID, including students with no enrollments. (3)

**c.** For each course, calculate the average grade using only enrollments with grades of at least 5. Return only courses whose resulting average exceeds 7. (4)

**d.** Count students in each city, retain cities with at least three students, and sort by count descending. (3)

### Question 5 — Interpret a join (8 marks)

Consider:

```
SELECT c.title, COUNT(*) AS n
FROM Courses c
LEFT JOIN Enrollments e
    ON c.course_id = e.course_id
GROUP BY c.course_id, c.title
HAVING COUNT(*) > 0;
```

**a.** Does this query include courses with no enrollments? Explain. (2)

**b.** Rewrite it to return all courses with a correct enrollment count, including zero. (3)

**c.** Explain the difference between `COUNT(*)` and `COUNT(e.student_id)` for this left join. (3)

## Section D — NoSQL and MongoDB (15 marks, 18 minutes)

Assume a MongoDB `movies` collection contains documents such as:

```
{
  title: "Example",
  year: 2018,
  genre: "Sci-Fi",
  genres: ["Sci-Fi", "Adventure"],
  rating: 8.2,
  director: "A. Director"
}
```

### Question 6 — Model selection and operators (7 marks)

**a.** Choose the most suitable model for each use case: key-value, graph, or document. (3)

1. User sessions retrieved by a unique session key.
2. Traversing people and friendship relationships.
3. Flexible product records containing nested attributes.

**b.** Write a MongoDB query to find movies released from 1990 through 2020 inclusive. (2)

**c.** Explain `$in` versus `$all` when querying the `genres` array. (2)

### Question 7 — Aggregation and consistency (8 marks)

**a.** Write an aggregation pipeline that returns the three highest-rated Sci-Fi movies, outputting only `title` and `rating` and excluding `_id`. (4)

**b.** What does `{ rating: { $exists: true } }` match if `rating` is present with the value `null`? Explain. (2)

**c.** During a network partition, explain the CAP trade-off between CP and AP behavior. (2)

## Section E — Redis (20 marks, 24 minutes)

### Question 8 — Commands and persistence (8 marks)

Consider:

```
SET session:42 active EX 60
TTL session:42
SET user:42:name "Mina"
HSET user:42 name "Mina" age "22"
ZADD leaderboard 120 Alice 200 Bob 160 Chen
ZINCRBY leaderboard 50 Alice
ZREVRANGE leaderboard 0 2 WITHSCORES
```

**a.** What does `EX 60` do? (1)

**b.** What do `TTL` results `-1` and `-2` mean? (2)

**c.** Give the final leaderboard scores and descending order. (3)

**d.** Give one advantage and one limitation of RDB snapshots compared with AOF persistence. (2)

### Question 9 — Safe Redis design (12 marks)

**a.** A service must store user profile fields and retrieve an individual field. Choose a Redis data type and provide commands to set and retrieve a user's email. (3)

**b.** A fixed-window rate limiter allows at most 50 requests per 60-second window. Explain why `GET`, application-side increment, and `SET` is unsafe under concurrency. Propose a safer Redis approach and explain why expiry should be initialized only when the counter is first created. (5)

**c.** A service acquires a lock using `SET lock:item7 worker-A NX EX 10`. Explain why the owner must not blindly delete the key and describe safe release. (4)

## Section F — Apache Spark (23 marks, 28 minutes)

### Question 10 — RDD semantics (10 marks)

Consider:

```
rdd = sc.parallelize([1, 2, 2, 3, 4])
a = rdd.filter(lambda x: x > 1)
b = a.map(lambda x: x * 2)
c = b.distinct()
result = c.collect()
```

**a.** What is `result`? (3)

**b.** Which operations are transformations, and which line triggers the computation needed for the result? (3)

**c.** Classify `filter`, `map`, and `distinct` as typically narrow or wide. Which normally causes a shuffle? (2)

**d.** Explain why RDD immutability does not mean that each transformation immediately computes a new dataset. (2)

### Question 11 — Word count and performance (8 marks)

**a.** Complete the pipeline. (4)

```
word_counts = (
    lines
    .__________(lambda line: line.split())
    .map(lambda word: (word, 1))
    .__________(lambda a, b: a + b)
)
```

**b.** Why is `reduceByKey()` generally preferable to `groupByKey()` for word counts? (2)

**c.** Explain one risk of using `collect()` on a very large RDD and give a safer way to inspect a small sample. (2)

### Question 12 — Architecture and DataFrames (5 marks)

**a.** Distinguish the roles of the Spark driver and executors. (2)

**b.** Write a PySpark DataFrame expression selecting `customer` and calculating `quantity * price` as `total`. (2)

**c.** In classic Spark Streaming, what is a DStream? (1)

---

**End of Exam 1 — 100 marks**

# TD1 — `practice-exam-1-solutions.md`

Writing

# Practice Exam 1 — Model Solutions and Marking Scheme

**Duration:** 120 minutes\
**Total:** 100 marks

Equivalent correct SQL, MongoDB, Redis, and PySpark solutions should receive credit. Minor syntax mistakes should lose fewer marks than conceptual errors.

## Section A — Multiple choice (10 marks)

1. **B — Conceptual.** It describes business entities and relationships.
2. **B — On the many side.** Each child row references its parent.
3. **C —****`HAVING`****.** It filters groups after grouping and aggregation.
4. **B —****`$all`****.** Every specified value must match the array.
5. **D — Sorted Set.** Each member has a score and is ordered by score.

**Marking:** 2 marks per correct answer; no partial credit.

## Section B — Data modeling and transactions (12 marks)

### Question 2 (7 marks)

**a. Primary key (2)**

The composite key `(StudentID, CourseID)` identifies an enrollment.

- `StudentID` alone is insufficient because one student can enroll in multiple courses. (1)
- `CourseID` alone is insufficient because a course can have multiple students. (1)

**b. Dependencies (2)**

- Partial dependency: `StudentID → StudentName`, because the name depends on only part of the composite key. (1)
- Transitive dependency: `CourseID → InstructorID → InstructorName`. The instructor name depends on the instructor ID, which is determined by the course ID. (1)

**c. Normalized design (3)**

One valid decomposition is:

```
Student(
    StudentID PK,
    StudentName
)

Instructor(
    InstructorID PK,
    InstructorName
)

Course(
    CourseID PK,
    CourseTitle,
    InstructorID FK
)

Enrollment(
    StudentID PK, FK,
    CourseID PK, FK,
    Grade
)
```

Award:

- 1 mark for separating student details.
- 1 mark for separating instructor details and referencing the instructor from Course.
- 1 mark for an Enrollment table with the composite key and Grade.

### Question 3 (5 marks)

**a. Atomicity (1)**

The debit and credit must commit together or neither should take effect.

**b. Durability (1)**

Once the transfer commits, its changes should survive a subsequent failure.

**c. Consistency versus isolation (3)**

- **Consistency:** The transaction must preserve defined database rules, such as valid account balances and accounting invariants. (1.5)
- **Isolation:** Concurrent transfers or account operations must not interfere improperly with one another. (1.5)

A response that merely repeats the definitions without applying them to the transfer should receive partial credit.

## Section C — SQL (20 marks)

### Question 4 (12 marks)

**a. Students from Barcelona (2)**

```
SELECT name
FROM Students
WHERE city = 'Barcelona'
ORDER BY name ASC;
```

- Correct filter: 1
- Correct sorting: 1

**b. Every student, including those without enrollments (3)**

```
SELECT s.name, e.course_id
FROM Students AS s
LEFT JOIN Enrollments AS e
    ON s.student_id = e.student_id;
```

- Correct `LEFT JOIN`: 1
- Correct join condition: 1
- Preserves students with no enrollments: 1

**c. Average qualifying grades greater than 7 (4)**

```
SELECT course_id, AVG(grade) AS average_grade
FROM Enrollments
WHERE grade >= 5
GROUP BY course_id
HAVING AVG(grade) > 7;
```

- Filters individual grades before grouping: 1
- Groups by course: 1
- Calculates the average: 1
- Filters groups with `HAVING`: 1

Courses without any qualifying enrollment are absent. If the question were intended to include every course, a `LEFT JOIN` from Courses would be needed.

**d. Student count by city (3)**

```
SELECT city, COUNT(*) AS student_count
FROM Students
GROUP BY city
HAVING COUNT(*) >= 3
ORDER BY student_count DESC;
```

- Correct grouping and count: 1
- Correct `HAVING`: 1
- Correct descending order: 1

### Question 5 (8 marks)

**a. Does it include courses with no enrollments? (2)**

No. A left join produces one null-extended row for a course with no enrollment, so `COUNT(*)` is 1 for that group. `HAVING COUNT(*) > 0` therefore retains that course. The important conclusion is that **it does include courses with no enrollments**, but reports the count as 1, which is misleading.

Award 1 mark for recognizing the null-extended row and 1 for the correct conclusion.

**b. Correct count including zero (3)**

```
SELECT c.title, COUNT(e.student_id) AS enrollment_count
FROM Courses AS c
LEFT JOIN Enrollments AS e
    ON c.course_id = e.course_id
GROUP BY c.course_id, c.title;
```

- Keeps `LEFT JOIN`: 1
- Counts a non-null column from the right table: 1
- Does not filter away zero-enrollment courses: 1

**c.****`COUNT(*)`****versus****`COUNT(e.student_id)`****(3)**

- `COUNT(*)` counts every result row, including a null-extended row from a left join. (1.5)
- `COUNT(e.student_id)` counts only non-NULL values, so it returns zero for a course with no enrollment. (1.5)

## Section D — NoSQL and MongoDB (15 marks)

### Question 6 (7 marks)

**a. Model selection (3)**

1. User sessions → **key-value**.
2. Friendship traversal → **graph**.
3. Flexible nested product records → **document**.

Award 1 mark each.

**b. Inclusive year range (2)**

```
db.movies.find({
  year: { $gte: 1990, $lte: 2020 }
})
```

Award 1 mark for each correct boundary operator.

**c.****`$in`****versus****`$all`****(2)**

- `$in` matches when at least one specified value matches.
- `$all` requires every specified value to be present in the array.

For example, `$in: ["Sci-Fi", "Adventure"]` matches a movie containing either value; `$all` requires both.

### Question 7 (8 marks)

**a. Aggregation pipeline (4)**

```
db.movies.aggregate([
  { $match: { genre: "Sci-Fi" } },
  { $sort: { rating: -1 } },
  { $limit: 3 },
  {
    $project: {
      _id: 0,
      title: 1,
      rating: 1
    }
  }
])
```

- Correct filter: 1
- Descending rating sort: 1
- Limit to three: 1
- Correct projection excluding `_id`: 1

**b.****`$exists`****with null (2)**

It matches. `$exists: true` checks whether the field is present, not whether its value is non-null.

**c. CAP trade-off (2)**

During a partition, a CP design may reject or delay some requests to preserve strong consistency. An AP design continues responding but may return stale or conflicting data until replicas converge. Award 1 mark for each behavior.

## Section E — Redis (20 marks)

### Question 8 (8 marks)

**a.****`EX 60`****(1)**

Sets the key's expiry to 60 seconds.

**b. TTL results (2)**

- `-1`: Key exists without an expiry.
- `-2`: Key does not exist.

Award 1 mark each.

**c. Leaderboard (3)**

Initial scores:

- Alice: 120
- Bob: 200
- Chen: 160

After `ZINCRBY leaderboard 50 Alice`:

- Alice: 170
- Bob: 200
- Chen: 160

Descending order: **Bob (200), Alice (170), Chen (160)**.

Award 1 mark for the updated score and 2 for the correct order and scores.

**d. RDB versus AOF (2)**

Example:

- RDB advantage: compact point-in-time snapshots and generally fast restoration.
- RDB limitation: writes after the last usable snapshot may be lost.

Award 1 mark for each valid point. A valid AOF comparison is also acceptable.

### Question 9 (12 marks)

**a. User profile (3)**

Use a Redis **Hash**:

```
HSET user:42 name "Mina" email "mina@example.com"
HGET user:42 email
```

- Appropriate data type: 1
- Correct write command: 1
- Correct field retrieval: 1

**b. Fixed-window rate limiter (5)**

A `GET` followed by application-side increment and `SET` is not atomic. Two concurrent requests may read the same count and overwrite one another, losing an increment.

One safer pattern is a Lua script:

```
local count = redis.call('INCR', KEYS[1])
if count == 1 then
    redis.call('EXPIRE', KEYS[1], 60)
end
return count
```

The script executes atomically in Redis. The application rejects requests with counts greater than 50. Setting the expiry only when the count first becomes 1 prevents each request from extending the fixed window.

Award:

- Identifies the race condition: 1
- Uses atomic increment or Lua: 2
- Explains first-request expiry: 1
- Explains rejection when count exceeds 50: 1

This is a fixed-window limiter, not a rolling-window guarantee.

**c. Safe lock release (4)**

A lock may expire and then be acquired by another worker. If the original owner blindly deletes the key, it could delete the new owner's lock.

A safe release checks the unique ownership token and deletes the key atomically:

```
if redis.call('GET', KEYS[1]) == ARGV[1] then
    return redis.call('DEL', KEYS[1])
else
    return 0
end
```

Award:

- Explains expiry/reacquisition risk: 2
- Verifies the token: 1
- Makes verification and deletion atomic: 1

## Section F — Apache Spark (23 marks)

### Question 10 (10 marks)

**a. Result (3)**

The values are filtered to `[2, 2, 3, 4]`, mapped to `[4, 4, 6, 8]`, and deduplicated.

```
[8, 4, 6]
```

The exact order of the collected result is not guaranteed because `distinct()` does not promise a particular ordering. Award full credit for the set of unique values `{4, 6, 8}` and an explicit note that order is not guaranteed.

**b. Transformations and action (3)**

- `filter`, `map`, and `distinct` are transformations. (2)
- `collect()` is an action that triggers the computation required to return the result. (1)

**c. Narrow/wide (2)**

- `filter`: narrow.
- `map`: narrow.
- `distinct`: typically wide and normally requires a shuffle.

Award 0.5 per classification and 0.5 for the shuffle explanation, capped at 2.

**d. Immutability and laziness (2)**

Each transformation defines a new RDD and leaves its input unchanged. The computation is normally deferred until an action requests a result.

### Question 11 (8 marks)

**a. Completed pipeline (4)**

```
word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

Award 2 marks for `flatMap` and 2 for `reduceByKey`.

**b. Why****`reduceByKey()`****? (2)**

It can combine values locally before the shuffle, reducing the amount of data transferred. `groupByKey()` moves values to their groups and retains all values, which is unnecessary when only a sum is needed.

**c.****`collect()`****risk and alternative (2)**

`collect()` transfers every element to the driver and may exhaust driver memory. Use `take(n)` for a small sample, or use distributed output for large results.

### Question 12 (5 marks)

**a. Driver versus executors (2)**

- Driver: coordinates the application, plans execution, and schedules tasks. (1)
- Executors: execute tasks on partitions and may cache data. (1)

**b. DataFrame expression (2)**

```
from pyspark.sql.functions import col

df.select(
    "customer",
    (col("quantity") * col("price")).alias("total")
)
```

Award 1 mark for selecting `customer` and 1 for the calculated aliased column.

**c. DStream (1)**

A DStream is a time-ordered sequence of RDDs, typically one RDD per micro-batch interval in classic Spark Streaming.

---

## Final marking summary

| Section | Marks |
| --- | --- |
| A — Multiple choice | 10 |
| B — Modeling and ACID | 12 |
| C — SQL | 20 |
| D — NoSQL and MongoDB | 15 |
| E — Redis | 20 |
| F — Spark | 23 |
| **Total** | **100** |

# TD2 — `practice-exam-2.md`

Writing

# Practice Exam 2 — Data Management

**Topics:** T0–T5\
**Duration:** 120 minutes\
**Total marks:** 100

## Instructions

Answer every question. Explain debugging decisions and show code where requested. Awarded marks reflect correctness, reasoning, and appropriate use of the supplied course concepts.

## Section A — Diagnose the concepts (10 marks, 10 minutes)

Choose one answer per question. Each question is worth 2 marks.

**1.** Which statement about normalization is correct?

A. 1NF eliminates every form of redundancy.\
B. 2NF addresses partial dependencies on a proper subset of a candidate key.\
C. 3NF means a table cannot contain foreign keys.\
D. Normalization always improves every query's performance.

**2.** What is the main risk of executing this statement without checking its predicate?

```
UPDATE Students SET age = 21;
```

A. It updates only one row.\
B. It updates every row in the table.\
C. It drops the table.\
D. It updates only rows where age is already 21.

**3.** What does this MongoDB query match?

```
db.movies.find({ rating: { $exists: true } })
```

A. Only documents with a non-null rating\
B. Documents in which rating is present, including those where it is null\
C. Only documents where rating is missing\
D. Only documents with rating greater than zero

**4.** What is the main purpose of Redis Sentinel?

A. Partition keys across hash slots\
B. Coordinate monitoring and failover for primary–replica deployments\
C. Replace RDB and AOF\
D. Convert Lists into Sorted Sets

**5.** Which PySpark statement is correct?

A. `collect()` is safe for arbitrarily large datasets.\
B. `groupByKey()` is always more efficient than `reduceByKey()`.\
C. `distinct()` typically involves a shuffle.\
D. Transformations always execute immediately.

## Section B — Relational design and SQL debugging (20 marks, 24 minutes)

Use the schema:

```
Customers(customer_id, name, city)
Orders(order_id, customer_id, amount, status)
```

### Question 2 — Keys and integrity (5 marks)

**a.** Identify a suitable primary key for each table and the foreign key relationship. (2)

**b.** Explain why `Orders.customer_id` should not normally be unique if a customer may place multiple orders. (1)

**c.** A developer deletes a customer who still has orders. Explain the purpose of referential integrity and give one possible policy for handling this deletion. (2)

### Question 3 — SQL debugging (9 marks)

A developer wants the average amount of completed orders per customer, retaining only customers whose average exceeds €100.

They write:

```
SELECT c.customer_id, c.name, AVG(o.amount) AS avg_amount
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
WHERE AVG(o.amount) > 100
  AND o.status = 'completed'
GROUP BY c.customer_id, c.name;
```

**a.** Identify two problems with the query. (3)

**b.** Rewrite the query to satisfy the requirement. (4)

**c.** Explain how your query treats customers who have no completed orders. (2)

### Question 4 — Index reasoning (6 marks)

A table has a composite index on `(city, age)`.

**a.** Explain why a query filtering by `city` can often benefit from this index. (2)

**b.** Explain why a query filtering only by `age` may not benefit as much. (2)

**c.** State one cost of adding indexes to a write-intensive table. (2)

## Section C — MongoDB and distributed-system reasoning (15 marks, 18 minutes)

Assume documents in `movies` may contain:

```
{
  title: "Sample",
  year: 2015,
  genres: ["Drama", "Mystery"],
  rating: null
}
```

### Question 5 — Query interpretation (8 marks)

**a.** Write a query for movies released strictly after 2000 and no later than 2020. (2)

**b.** Write a query for movies whose `genres` array contains both `"Drama"` and `"Mystery"`. (2)

**c.** What does the following query return regarding the example document above? Explain.

```
db.movies.find({ rating: { $exists: false } })
```

(2)

**d.** Explain the difference between `updateOne()` and `updateMany()`. (2)

### Question 6 — Aggregation and CAP (7 marks)

**a.** Write an aggregation pipeline that counts movies by `genre`, using the group key as `_id` and `$sum: 1` to count documents. (3)

**b.** A distributed service must continue answering requests during a network partition, even if some answers may be stale. Is AP or CP the closer description? Explain. (2)

**c.** Explain why “NoSQL means no transactions and no consistency” is an incorrect statement. (2)

## Section D — Redis debugging and architecture (25 marks, 30 minutes)

### Question 7 — TTL and persistence (7 marks)

A developer executes:

```
SET cache:product:9 "old-value" EX 30
SET cache:product:9 "new-value"
TTL cache:product:9
```

**a.** Explain what happens to the original expiry after the second `SET`. (2)

**b.** What could the application do if the key expires and a cache miss occurs? (2)

**c.** Compare RDB and AOF in terms of recovery and possible data loss. (3)

### Question 8 — Replication, Sentinel and Cluster (8 marks)

**a.** Distinguish replication from sharding. (2)

**b.** What does Sentinel add to a primary–replica deployment? (2)

**c.** Redis Cluster uses 16,384 hash slots. Describe how a key is assigned to a slot and how the slot determines the destination node. (2)

**d.** Explain why replication is not a substitute for backups. (2)

### Question 9 — Diagnose unsafe operations (10 marks)

**a.** An application performs `GET stock`, checks whether stock is sufficient, then issues `DECRBY stock quantity`. Explain the concurrency problem and propose a safer approach. (4)

**b.** A lock owner uses `SET lock:order5 token-A NX EX 10`, then blindly deletes the lock when it finishes. Explain the failure scenario and the safe release principle. (3)

**c.** A developer implements a 60-second fixed-window counter but refreshes its expiry after every request. Explain why this differs from a fixed window whose expiry is initialized on the first request. (3)

## Section E — Apache Spark debugging and performance (30 marks, 38 minutes)

### Question 10 — Code tracing (10 marks)

Consider:

```
rdd = sc.parallelize([1, 2, 3, 4, 5])
filtered = rdd.filter(lambda x: x % 2 == 1)
mapped = filtered.map(lambda x: x * 10)
result = mapped.take(2)
```

**a.** What values can `result` contain? (3)

**b.** Which operations are transformations and which is the action? (2)

**c.** Does `take(2)` necessarily compute every partition of the source RDD? Explain. (2)

**d.** Explain how lineage helps recover a lost partition. (3)

### Question 11 — Performance diagnosis (10 marks)

A program groups every word in a very large text dataset using `groupByKey()` and then counts the number of values in each group. It calls `collect()` to return the entire result to the driver.

**a.** Explain why `groupByKey()` is inefficient for this particular task and suggest a replacement. (3)

**b.** Explain two possible costs of a shuffle. (2)

**c.** Explain why `collect()` is risky and suggest a safer output or inspection strategy. (3)

**d.** Explain when caching may be useful and one cost of caching. (2)

### Question 12 — DataFrame debugging (6 marks)

A developer writes:

```
df.filter((df.quantity > 5) and (df.price < 100))
```

**a.** Explain the problem and correct the expression. (3)

**b.** Write an expression that groups by `customer` and calculates the sum of `quantity` as `total_quantity`. (3)

### Question 13 — Streaming (4 marks)

**a.** In classic Spark Streaming, what does a DStream represent? (2)

**b.** Explain one trade-off when reducing a micro-batch interval. (2)

---

**End of Exam 2 — 100 marks**

# TD2 — `practice-exam-2-solutions.md`

Writing

# Practice Exam 2 — Model Solutions and Marking Scheme

**Duration:** 120 minutes\
**Total:** 100 marks

## Section A — Multiple choice (10 marks)

1. **B.** 2NF addresses partial dependencies on a proper subset of a candidate key.
2. **B.** Without `WHERE`, every row is updated.
3. **B.** `$exists: true` matches a present field even if its value is `null`.
4. **B.** Sentinel monitors deployments and coordinates failover.
5. **C.** `distinct()` typically requires redistribution of data.

Award 2 marks per correct answer.

## Section B — Relational design and SQL (20 marks)

### Question 2 (5 marks)

**a.** `Customers.customer_id` is the customer primary key; `Orders.order_id` is the order primary key. `Orders.customer_id` references `Customers.customer_id`. (2)

**b.** A customer may place multiple orders, so the customer ID must be allowed to repeat in Orders. A uniqueness constraint would incorrectly enforce at most one order per customer. (1)

**c.** Referential integrity prevents an order from referencing a nonexistent customer. Possible policies include rejecting deletion while dependent orders exist, cascading deletion when appropriate, or using a carefully designed nullable reference policy. (2)

### Question 3 (9 marks)

**a. Two problems (3)**

- `AVG(o.amount)` is an aggregate and should be filtered with `HAVING`, not `WHERE`. (1.5)
- The status condition should filter individual orders before aggregation; customers without matching completed orders need deliberate treatment. (1.5)

**b. Correct query (4)**

```
SELECT c.customer_id, c.name, AVG(o.amount) AS avg_amount
FROM Customers AS c
JOIN Orders AS o
    ON c.customer_id = o.customer_id
WHERE o.status = 'completed'
GROUP BY c.customer_id, c.name
HAVING AVG(o.amount) > 100;
```

Award:

- Correct join: 1
- Row filter in `WHERE`: 1
- Grouping: 1
- Aggregate filter in `HAVING`: 1

An equivalent `LEFT JOIN` solution with the status condition in the `ON` clause is acceptable if it handles customers without completed orders correctly.

**c. Customers without completed orders (2)**

The provided solution uses an inner join, so customers with no completed orders do not appear. This is reasonable because their average of completed orders is undefined. Award full credit for explaining this and partial credit for a consistent alternative interpretation.

### Question 4 (6 marks)

**a.** The index is ordered first by `city`, so the DBMS can often locate a city range efficiently. (2)

**b.** With no leading `city` constraint, an index on `(city, age)` may not efficiently locate all rows for a given age. The optimizer might choose a table scan or another index. (2)

**c.** Indexes consume storage and must be maintained on writes, increasing write latency and work. (2)

## Section C — MongoDB and distributed systems (15 marks)

### Question 5 (8 marks)

**a.**

```
db.movies.find({
  year: { $gt: 2000, $lte: 2020 }
})
```

Award 1 mark per correct boundary.

**b.**

```
db.movies.find({
  genres: { $all: ["Drama", "Mystery"] }
})
```

Award 2 marks for correct `$all` syntax and values.

**c.**

```
db.movies.find({ rating: { $exists: false } })
```

The example document does **not** match because `rating` exists, even though its value is `null`. (2)

**d.** `updateOne()` modifies at most one matching document; `updateMany()` modifies all matching documents. (2)

### Question 6 (7 marks)

**a.**

```
db.movies.aggregate([
  {
    $group: {
      _id: "$genre",
      count: { $sum: 1 }
    }
  }
])
```

- `$group`: 1
- `_id: "$genre"`: 1
- `$sum: 1`: 1

**b.** AP is the closer description. It continues responding during a partition but may return stale or temporarily inconsistent results. (2)

**c.** NoSQL includes multiple models and consistency options. Some NoSQL systems support transactions, and their consistency guarantees depend on the database, operation, and configuration. (2)

## Section D — Redis (25 marks)

### Question 7 (7 marks)

**a.** A plain `SET` without an expiry option replaces the value and removes the previous TTL. The key normally becomes persistent until deleted or assigned another expiry. (2)

**b.** In a cache-aside flow, the application fetches the value from the authoritative database and may populate Redis again. Redis does not automatically refetch the source value. (2)

**c.** RDB saves snapshots and is usually compact with convenient restoration, but changes after the last usable snapshot may be lost. AOF records write operations and can provide more granular recovery, but recovery may require replaying the log and durability depends on fsync policy. (3)

### Question 8 (8 marks)

**a.** Replication copies data to other nodes; sharding distributes different portions of the dataset across nodes. (2)

**b.** Sentinel monitors primary and replicas and coordinates promotion of a replica when the primary is judged unavailable. It does not shard the dataset. (2)

**c.**

```
slot = CRC16(key) mod 16384
```

The calculated slot is mapped to the node currently responsible for that slot. (2)

**d.** Logical deletion or corruption may propagate to replicas. Independent backups preserve recovery points that may predate the error. (2)

### Question 9 (10 marks)

**a. Stock concurrency (4)**

Separate read-check-decrement operations can race: two requests may both observe enough stock and both decrement, potentially overselling. Use a Lua script to read, validate, and decrement atomically. (2 for the race; 2 for the atomic approach)

Example:

```
local stock = tonumber(redis.call('GET', KEYS[1]) or '0')
local quantity = tonumber(ARGV[1])

if quantity <= 0 then return -2 end
if stock < quantity then return -1 end

return redis.call('DECRBY', KEYS[1], quantity)
```

**b. Lock release (3)**

The lock may expire and another worker may acquire it before the original owner deletes it. The original owner must compare the key with its unique token and delete it atomically only if the token still matches. (1 for failure scenario; 2 for safe release)

**c. Fixed-window expiry (3)**

Refreshing expiry on every request creates a sliding timeout from the latest request and can keep the counter alive indefinitely under continuous traffic. A fixed window initializes expiry only when the counter is first created. (3)

## Section E — Spark (30 marks)

### Question 10 (10 marks)

**a.** The odd values are `[1, 3, 5]`; mapping gives `[10, 30, 50]`. `take(2)` returns the first two elements according to the RDD's partition/order behavior, typically `[10, 30]` for this parallelized input. (3)

**b.** `filter` and `map` are transformations; `take` is an action. (2)

**c.** Not necessarily. `take(2)` may stop once enough results are collected, although execution depends on partition layout and scheduling. (2)

**d.** Lineage records the dependencies and operations used to construct an RDD. If a partition is lost, Spark can recompute it from the source and its lineage when the required inputs remain available. (3)

### Question 11 (10 marks)

**a.** Use `reduceByKey()` to combine counts by word, for example:

```
counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

`reduceByKey()` can aggregate locally before shuffling. `groupByKey()` transfers and retains all values even though only a count is needed. (3)

**b.** Shuffle costs can include network transfer, serialization, disk I/O, memory pressure, and additional stage scheduling. Any two: 2 marks.

**c.** `collect()` transfers all output to driver memory and can cause memory exhaustion. Use `take(n)` for a small sample, or write the full result to distributed storage with an appropriate output operation. (3)

**d.** Cache a costly dataset reused by multiple actions to avoid recomputation. Caching consumes memory or other storage resources and may cause eviction or overhead. (2)

### Question 12 (6 marks)

**a.** Python's `and` evaluates a Python boolean expression and cannot combine Spark `Column` predicates as intended. Use `&` and parentheses:

```
df.filter((df.quantity > 5) & (df.price < 100))
```

- Identifies the problem: 1
- Uses `&`: 1
- Parenthesizes the conditions: 1

**b.**

```
from pyspark.sql.functions import sum

df.groupBy("customer").agg(
    sum("quantity").alias("total_quantity")
)
```

- Correct grouping: 1
- Correct aggregation: 1
- Correct alias: 1

### Question 13 (4 marks)

**a.** A DStream represents a time-ordered sequence of RDDs, generally one RDD per micro-batch. (2)

**b.** A shorter batch interval can reduce waiting time, but increases scheduling overhead and may cause the stream to fall behind if processing cannot keep up. (2)

## Final marking summary

| Section | Marks |
| --- | --- |
| A — Multiple choice | 10 |
| B — Relational design and SQL | 20 |
| C — MongoDB and distributed systems | 15 |
| D — Redis | 25 |
| E — Spark | 30 |
| **Total** | **100** |

# TD3 — `practice-exam-3.md`

Writing

# Practice Exam 3 — Data Management

**Topics:** T0–T5\
**Duration:** 120 minutes\
**Total marks:** 100

## Instructions

Answer all questions. This paper emphasizes integrated design, practical implementation, and justification of trade-offs. Show working and explain assumptions.

## Section A — Short conceptual questions (10 marks, 10 minutes)

Answer briefly. Each question is worth 2 marks.

**1.** Distinguish throughput from latency.

**2.** Explain why a foreign key does not necessarily have to be unique.

**3.** Explain why an index can improve read performance while reducing write performance.

**4.** Distinguish a MongoDB document from a relational row in terms of schema flexibility.

**5.** Explain why Redis replication does not eliminate the need for backups.

## Section B — Relational design and SQL (20 marks, 24 minutes)

A training platform stores courses and learner registrations.

```
Learner(learner_id, name, email)
Course(course_id, title, instructor_id, instructor_name)
Registration(learner_id, course_id, registered_at, score)
```

Assume learners can register for multiple courses, each course has one instructor, and each registration has one score.

### Question 2 — Design and normalization (7 marks)

**a.** Propose primary keys and foreign keys for the three tables. (3)

**b.** Explain why storing `instructor_name` in every course row may cause redundancy if instructors teach multiple courses. Propose a normalized alternative. (2)

**c.** Identify the relationship cardinality between Learner and Course, and explain how it is implemented relationally. (2)

### Question 3 — Analytical SQL (8 marks)

Write a query that returns each instructor ID and instructor name with the number of distinct learners registered in their courses. Include only instructors with at least 10 distinct learners, ordered by learner count descending.

You may assume these tables:

```
Instructor(instructor_id, instructor_name)
Course(course_id, title, instructor_id)
Registration(learner_id, course_id, registered_at, score)
```

Marks: join logic (2), distinct learner count (2), grouping (2), `HAVING` and ordering (2).

### Question 4 — Index and transaction decisions (5 marks)

**a.** An application frequently searches registrations by `course_id` and then by `registered_at`. Propose a composite index and explain why column order matters. (3)

**b.** Two concurrent transactions attempt to update the same registration score. Identify the ACID property most directly associated with controlling improper interference and explain briefly. (2)

## Section C — NoSQL and MongoDB application (15 marks, 18 minutes)

A product catalogue stores documents in `products`:

```
{
  name: "Trail Pack",
  category: "Outdoor",
  price: 75,
  tags: ["hiking", "travel"],
  stock: 12
}
```

### Question 5 — Query construction (7 marks)

**a.** Find products priced from 50 inclusive to 100 exclusive. (2)

**b.** Find products whose tags contain both `"hiking"` and `"travel"`. (2)

**c.** Update the stock to 20 for one matching product named `"Trail Pack"`. Use `$set` and an appropriate update method. (2)

**d.** Explain why `updateMany()` should be chosen only when changing all matching documents is intended. (1)

### Question 6 — Aggregation and database selection (8 marks)

**a.** Write an aggregation pipeline that calculates the average price per category and sorts categories by average price descending. (4)

**b.** A recommendation system frequently traverses connections between users, products, and shared interests. Which NoSQL model is a strong candidate, and why? (2)

**c.** A high-volume event system needs partitioned, distributed data organized around access by partition key. Which model could be appropriate, and name one example database. (2)

## Section D — Redis system design (25 marks, 30 minutes)

### Question 7 — Cache design (7 marks)

An API reads product details from a relational database. Popular products are requested frequently, but the catalogue is much larger than available cache memory.

**a.** Describe a cache-aside read flow. (3)

**b.** Explain why lazy loading is a reasonable choice for this workload. (2)

**c.** Explain one way stale cache data can arise and one strategy to limit its impact. (2)

### Question 8 — Redis architecture and persistence (8 marks)

A service needs horizontal distribution of a large keyspace, replication, and automatic handling of some node failures.

**a.** Explain why Redis Cluster is more appropriate than a standalone instance for horizontal data distribution. (2)

**b.** Explain the role of hash slots in routing keys. Include the number of slots and the slot formula. (2)

**c.** Compare RDB and AOF and recommend a persistence approach if both convenient snapshots and more granular recovery are desirable. (3)

**d.** Name one reason backups are still necessary. (1)

### Question 9 — Atomic inventory reservation (10 marks)

An online store has a Redis key `stock:item42` containing available inventory. A request supplies a quantity to reserve.

**a.** Explain why separate application-side commands to read stock, validate quantity, and decrement stock can oversell inventory. (3)

**b.** Write a Lua script that:

- Reads the stock, treating a missing key as zero.
- Returns `-2` if requested quantity is non-positive.
- Returns `-1` if stock is insufficient.
- Otherwise decrements stock and returns the remaining amount. (5)

**c.** State one important limitation: why does an atomic Redis stock decrement not automatically make an entire order workflow transactional? (2)

## Section E — Apache Spark practical design (30 marks, 38 minutes)

### Question 10 — Word count and ranking (10 marks)

You are given a large text dataset represented by `lines`, an RDD of strings.

**a.** Write a PySpark RDD pipeline to split lines into words, map each word to `(word, 1)`, and count each word with `reduceByKey()`. (5)

**b.** Write code to return the three most frequent words, with alphabetical ordering used to break ties. (3)

**c.** Explain one limitation of the basic `line.split()` approach when counting words. (2)

### Question 11 — Execution and optimization (10 marks)

Consider:

```
base = sc.parallelize([1, 2, 3, 4, 5, 6])
evens = base.filter(lambda x: x % 2 == 0)
squares = evens.map(lambda x: x * x)
unique = squares.distinct()
unique.cache()
answer = unique.count()
```

**a.** Determine `answer`. (2)

**b.** Identify the transformations and the action. (2)

**c.** Explain why `distinct()` commonly causes a shuffle. (2)

**d.** Does `cache()` immediately compute and store every partition? Explain. (2)

**e.** Give one circumstance in which caching this RDD would be useful and one reason it may not help here. (2)

### Question 12 — DataFrames and streaming (10 marks)

Assume a DataFrame `sales` has columns `customer`, `quantity`, and `price`.

**a.** Write PySpark code to calculate `quantity * price` as `total`, filter to rows where `total > 100`, and select `customer` and `total`. (4)

**b.** Write code to calculate total quantity per customer. (3)

**c.** Explain one difference between classic DStream-based Spark Streaming and Structured Streaming. (2)

**d.** Name one trade-off associated with a shorter micro-batch interval in classic Spark Streaming. (1)

---

**End of Exam 3 — 100 marks**

# TD3 — `practice-exam-3-solutions.md`

Writing

# Practice Exam 3 — Model Solutions and Marking Scheme

**Duration:** 120 minutes\
**Total:** 100 marks

Equivalent correct solutions are acceptable. Practical answers are marked for logic as well as syntax.

## Section A — Short conceptual questions (10 marks)

**1\. Throughput versus latency (2)**

- Throughput is the number of operations completed per unit of time. (1)
- Latency is the delay associated with an operation or request. (1)

**2\. Foreign-key uniqueness (2)**

A foreign key may repeat because multiple child records can reference the same parent. Uniqueness is appropriate only when the relationship or business rule requires it, such as a suitably constrained one-to-one relationship.

**3\. Index read/write trade-off (2)**

Indexes can help locate qualifying rows without scanning an entire table. Inserts, updates, and deletes may require index maintenance, increasing write work; indexes also consume storage.

**4\. MongoDB documents versus relational rows (2)**

Relational tables typically define columns and types through a schema. MongoDB documents in a collection can have different fields and nested structures, although validation rules can still impose constraints.

**5\. Replication versus backups (2)**

Replicas copy data and can improve redundancy or read capacity. Accidental deletion or corruption may propagate to replicas, whereas independent backups provide recovery points.

## Section B — Relational design and SQL (20 marks)

### Question 2 (7 marks)

**a. Keys (3)**

```
Learner(
    learner_id PK,
    name,
    email
)

Course(
    course_id PK,
    title,
    instructor_id FK
)

Registration(
    learner_id PK, FK,
    course_id PK, FK,
    registered_at,
    score
)
```

Assume a learner registers at most once per course. If repeated registrations are allowed, an alternative primary key, such as a registration ID, may be required.

- Correct learner key: 1
- Correct course/instructor relationship: 1
- Correct registration composite key and references: 1

**b. Instructor normalization (2)**

If instructors teach multiple courses, repeating the instructor name creates redundancy and risks inconsistent updates. Create an `Instructor(instructor_id PK, instructor_name)` table and reference it from Course. Award 1 mark for identifying the anomaly and 1 for the alternative.

**c. Cardinality (2)**

Learner and Course have an M:N relationship: one learner can take many courses, and one course can have many learners. Registration is the junction table, with foreign keys to both entities.

### Question 3 — Analytical SQL (8 marks)

```
SELECT
    i.instructor_id,
    i.instructor_name,
    COUNT(DISTINCT r.learner_id) AS learner_count
FROM Instructor AS i
JOIN Course AS c
    ON c.instructor_id = i.instructor_id
JOIN Registration AS r
    ON r.course_id = c.course_id
GROUP BY i.instructor_id, i.instructor_name
HAVING COUNT(DISTINCT r.learner_id) >= 10
ORDER BY learner_count DESC;
```

Marking:

- Correct joins: 2
- `COUNT(DISTINCT r.learner_id)`: 2
- Correct grouping: 2
- `HAVING` threshold and descending ordering: 2

### Question 4 (5 marks)

**a. Composite index (3)**

```
CREATE INDEX idx_registration_course_date
ON Registration(course_id, registered_at);
```

The index can support queries filtering by `course_id` and then narrowing or ordering by `registered_at`. Column order matters because composite indexes are organized by their leading columns; an index beginning with `course_id` may be less useful for a query filtering only by `registered_at`.

- Correct columns and order: 1
- Explains leading-column usefulness: 1
- Explains why order matters: 1

**b. Isolation (2)**

Isolation governs how concurrent transactions interact. Appropriate isolation and concurrency control help prevent improper interference, although the exact behavior depends on the DBMS and transaction isolation level.

## Section C — NoSQL and MongoDB (15 marks)

### Question 5 (7 marks)

**a. Price range (2)**

```
db.products.find({
  price: { $gte: 50, $lt: 100 }
})
```

**b. Both tags (2)**

```
db.products.find({
  tags: { $all: ["hiking", "travel"] }
})
```

**c. Update one product (2)**

```
db.products.updateOne(
  { name: "Trail Pack" },
  { $set: { stock: 20 } }
)
```

**d.****`updateMany()`****(1)**

It modifies every matching document, which can unintentionally change multiple records if the filter is broader than expected. Use it only when updating all matches is intended.

### Question 6 (8 marks)

**a. Average price by category (4)**

```
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      average_price: { $avg: "$price" }
    }
  },
  {
    $sort: { average_price: -1 }
  }
])
```

- Grouping by category: 1
- Correct `$avg`: 1
- Correct output field: 1
- Descending sort: 1

**b. Recommendation graph (2)**

A graph database is a strong candidate because the application repeatedly traverses relationships among users, products, and interests. Neo4j is one example.

**c. Distributed event data (2)**

A wide-column database such as Apache Cassandra can be suitable for high-volume distributed workloads organized around partition keys. The actual schema should be designed around access patterns.

## Section D — Redis (25 marks)

### Question 7 (7 marks)

**a. Cache-aside flow (3)**

1. Read the product key from Redis.
2. If it is a hit, return the cached value.
3. If it is a miss, read the authoritative database, return the result, and optionally populate Redis.

Award 1 mark per step.

**b. Lazy loading (2)**

Only requested products need to enter the cache, making it suitable when the catalogue is much larger than memory and only a subset is frequently accessed.

**c. Staleness and mitigation (2)**

Staleness can occur when the database changes but the cached copy is not refreshed or invalidated. Mitigations include deleting or updating the cache on writes, using TTLs, and designing explicit invalidation/error handling.

### Question 8 (8 marks)

**a. Cluster versus standalone (2)**

A standalone instance is limited by its own resources and does not horizontally shard the keyspace. Redis Cluster distributes hash slots and their keys across multiple nodes.

**b. Hash slots (2)**

Redis Cluster uses 16,384 slots:

```
slot = CRC16(key) mod 16384
```

The slot assignment maps the key to the node responsible for that slot.

**c. RDB and AOF (3)**

- RDB stores point-in-time snapshots, typically compact and convenient for restoration. (1)
- AOF records write operations and may provide more granular recovery, depending on synchronization policy. (1)
- Using both can provide complementary benefits: convenient snapshots and operation-log recovery. (1)

**d. Backups (1)**

Backups can provide recovery points after logical deletion, corruption, or errors that propagate to replicas.

### Question 9 (10 marks)

**a. Concurrency risk (3)**

Two requests can both read the same stock level and independently conclude that enough stock exists. Both may then decrement, overselling inventory. The read, validation, and decrement should execute atomically.

**b. Lua script (5)**

```
local stock = tonumber(redis.call('GET', KEYS[1]) or '0')
local quantity = tonumber(ARGV[1])

if quantity <= 0 then
    return -2
end

if stock < quantity then
    return -1
end

return redis.call('DECRBY', KEYS[1], quantity)
```

Marking:

- Reads and converts stock, handling a missing key: 1
- Reads and validates quantity: 1
- Returns `-2` for non-positive quantity: 1
- Returns `-1` for insufficient stock: 1
- Performs decrement and returns remaining stock: 1

**c. Workflow limitation (2)**

Redis atomicity applies to the script's Redis operations, not automatically to separate actions such as charging a payment provider, inserting an order in SQL, or sending a confirmation. A larger workflow needs appropriate transaction boundaries, idempotency, and failure recovery.

## Section E — Apache Spark (30 marks)

### Question 10 (10 marks)

**a. Word count (5)**

```
word_counts = (
    lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

- `flatMap` splits lines into individual words: 2
- Maps words to `(word, 1)`: 1
- Aggregates with `reduceByKey`: 2

**b. Top three with alphabetical tie-breaking (3)**

```
top_three = word_counts.takeOrdered(
    3,
    key=lambda pair: (-pair[1], pair[0])
)
```

- Uses `takeOrdered(3)`: 1
- Sorts by descending count using negative frequency: 1
- Uses the word as a tie-breaker: 1

**c. Limitation of****`split()`****(2)**

It splits on whitespace but does not normalize case or remove punctuation. For example, `"Spark"` and `"spark"` or `"spark,"` may be counted separately.

### Question 11 (10 marks)

**a. Answer (2)**

The even numbers are 2, 4, and 6. Their squares are 4, 16, and 36. They are already unique, so:

```
answer = 3
```

**b. Transformations and action (2)**

- `filter`, `map`, and `distinct` are transformations. (1)
- `count()` is the action. (1)

`cache()` is a persistence hint, not an action that immediately computes all partitions.

**c. Shuffle (2)**

`distinct()` must identify duplicates across partitions, so values may need to be redistributed or combined across partitions. This normally involves a shuffle.

**d. Cache behavior (2)**

No. `cache()` marks the RDD for persistence when it is computed. Evaluation is lazy, so the subsequent `count()` triggers the required computation and caching as partitions are processed.

**e. When caching helps (2)**

Caching helps if the same expensive RDD is reused by multiple actions. It may not help here because the RDD is used only once; caching adds storage overhead without necessarily avoiding future recomputation.

### Question 12 (10 marks)

**a. Calculate, filter, and select (4)**

```
from pyspark.sql.functions import col

result = (
    sales
    .withColumn("total", col("quantity") * col("price"))
    .filter(col("total") > 100)
    .select("customer", "total")
)
```

- Correct calculated column: 2
- Correct filter: 1
- Correct selection: 1

**b. Total quantity per customer (3)**

```
from pyspark.sql.functions import sum

totals = sales.groupBy("customer").agg(
    sum("quantity").alias("total_quantity")
)
```

- Group by customer: 1
- Sum quantity: 1
- Correct alias: 1

**c. Classic versus Structured Streaming (2)**

Classic Spark Streaming uses DStreams, which are sequences of RDDs. Structured Streaming uses structured DataFrames/Datasets and integrates with Spark SQL's query-planning framework.

**d. Shorter micro-batch interval (1)**

It can reduce waiting time but increases scheduling overhead and may make it harder to keep up with incoming data.

## Final marking summary

| Section | Marks |
| --- | --- |
| A — Concepts | 10 |
| B — Relational design and SQL | 20 |
| C — NoSQL and MongoDB | 15 |
| D — Redis | 25 |
| E — Spark | 30 |
| **Total** | **100** |

## Important corrections and final checks

Before committing the files, apply these two corrections to TD1.

1. In `practice-exam-1-solutions.md`, Question 5(a), replace the opening sentence with:

> Yes. The `LEFT JOIN` creates one null-extended row for a course without enrollments. `COUNT(*)` returns 1 for that row, so `HAVING COUNT(*) > 0` retains the course. The query therefore includes courses with no enrollments but incorrectly reports their count as 1.

2. In `practice-exam-1.md`, the section time allocations total 118 minutes, not 120. Change the instructions to say that the section times are indicative and leave a two-minute review buffer.

The three exams now have distinct emphases:

- TD1: Core concepts and foundational applications.
- TD2: Debugging, query reasoning, and performance.
- TD3: Integrated practical exercises and system-design decisions.

All six documents use a 100-mark grading scheme, include model answers and marking criteria, and are structured for independent use. The remaining limitation is that T0-specific coverage cannot be verified until its actual revision summary is supplied.
