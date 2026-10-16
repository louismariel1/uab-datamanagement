# Data Management 2026–2027 — Three Practice Exams

The three practice exams will cover T0–T5, using the compressed revision guides you supplied as the authoritative reference. Each exam will be designed for 120 minutes, with a balanced mix of conceptual understanding, practical application, code and query interpretation, and realistic exam traps.

The exams will be distinct rather than simply rewording the same questions. Each will include a separate answer key with explanations and marking guidance.

## 1. Exam blueprint

I'll use the following structure for each exam.

| Topic | Focus | Target marks |
| --- | --- | --- |
| T0 — Introduction to Data Systems | Foundations, system requirements and core concepts | 10 |
| T1 — Data Models & Data Storage | Modeling, relationships, normalization, keys and ACID | 15 |
| T2 — SQL | CRUD, joins, aggregation, transactions and indexes | 25 |
| T3 — NoSQL Databases & Data Modeling | Database selection, MongoDB, query operators and aggregation | 15 |
| T4 — Redis Introduction & Persistence | Data structures, caching, persistence, architecture and practical commands | 20 |
| T5 — Apache Spark | Architecture, RDDs, transformations, DataFrames, performance and streaming | 15 |
| Total | All six topics | 100 |

The allocation gives SQL substantial practical coverage while ensuring that Redis and Spark receive meaningful coding and interpretation questions. T0 receives a smaller allocation because its introductory concepts recur throughout the other topics.

Each exam will use the same topic weighting but different scenarios, datasets, code, and reasoning tasks.

## 2. Timing and question format

Each exam will follow this two-hour structure.

| Section | Content | Time | Marks |
| --- | --- | --- | --- |
| A | Multiple-choice and misconception checks | 15 min | 15 |
| B | Short-answer conceptual questions | 20 min | 15 |
| C | SQL, data modeling and NoSQL exercises | 30 min | 25 |
| D | Redis commands, persistence and practical scenarios | 25 min | 25 |
| E | Spark coding, execution tracing and performance | 25 min | 15 |
| F | Integrated design question | 5 min | 5 |
| Total |  | 120 min | 100 |

The marks in this table are the initial structure. For the final papers, I'll distribute the marks across topics so that the total remains consistent with the topic-weighting table, while retaining the variety of question formats.

The questions will test more than recall. Students will need to explain why an answer is correct, identify what a query or program actually does, correct faulty code, and justify design choices.

## 3. How the three exams will differ

### Practice Exam 1 — Core Competence

Difficulty: Moderate

Establishes mastery of the fundamentals: relational modeling, SQL query construction, MongoDB operators, Redis data structures, and Spark RDD execution.

- Emphasis on writing and interpreting correct code.
- Straightforward scenarios with carefully chosen distractors.
- Foundational questions on ACID, CAP, persistence and distributed processing.

### Practice Exam 2 — Applied Reasoning

Difficulty: Moderate to challenging

Focuses on applying concepts to less familiar situations.

- SQL joins, aggregation and `WHERE` versus `HAVING`.
- MongoDB aggregation pipelines and operator boundaries.
- Redis cache staleness, failover, locks and atomic inventory changes.
- Spark shuffles, partitioning, `reduceByKey()` and `groupByKey()`.

### Practice Exam 3 — Integrated Challenge

Difficulty: Challenging

Emphasizes debugging, trade-offs, and combining concepts across topics.

- Diagnose schema and normalization problems.
- Repair incorrect queries and unsafe Redis workflows.
- Evaluate consistency, persistence and scaling trade-offs.
- Trace Spark programs and explain performance bottlenecks.

## 4. Detailed question plan for all three papers

### Practice Exam 1 — Core Competence

| Question | Topic | Planned question | Marks |
| --- | --- | --- | --- |
| 1 | T0 | Distinguish throughput, latency and reliability in a data-system scenario | 5 |
| 2 | T0–T1 | Identify conceptual, logical and physical modeling decisions | 5 |
| 3 | T1 | Model a many-to-many student–course relationship and identify keys | 5 |
| 4 | T1 | Diagnose 1NF, 2NF and 3NF violations | 5 |
| 5 | T2 | Write SQL queries using filtering, sorting and aggregation | 10 |
| 6 | T2 | Interpret `INNER JOIN` versus `LEFT JOIN` results | 5 |
| 7 | T2 | Explain index trade-offs and composite-index column order | 5 |
| 8 | T3 | Select an appropriate NoSQL model for several scenarios | 5 |
| 9 | T3 | Predict MongoDB query results using `$gt`, `$in` and `$all` | 5 |
| 10 | T3 | Interpret a MongoDB aggregation pipeline | 5 |
| 11 | T4 | Predict Redis String, Hash, Set and Sorted Set command results | 8 |
| 12 | T4 | Compare RDB and AOF recovery behavior | 5 |
| 13 | T4 | Diagnose TTL and cache-aside behavior | 7 |
| 14 | T5 | Trace an RDD word-count program and identify when execution occurs | 10 |
| 15 | T5 | Explain narrow versus wide transformations and the risks of `collect()` | 5 |
| 16 | T0–T5 | Recommend a data-system design for a small application and justify the choices | 5 |
|  |  | Total | 100 |

### Practice Exam 2 — Applied Reasoning

| Question | Topic | Planned question | Marks |
| --- | --- | --- | --- |
| 1 | T0 | Choose suitable performance metrics for a high-traffic service | 5 |
| 2 | T1 | Correct an incorrect relational schema and its foreign-key placement | 5 |
| 3 | T1 | Explain how normalization prevents update anomalies | 5 |
| 4 | T2 | Repair SQL statements containing logical and aggregation errors | 10 |
| 5 | T2 | Write a grouped query using `HAVING` and a join | 10 |
| 6 | T2 | Evaluate a B+ tree index and explain its range-query benefits | 5 |
| 7 | T3 | Distinguish ACID, BASE and CAP in a distributed-system scenario | 5 |
| 8 | T3 | Correct a MongoDB query involving missing fields, `null` and array operators | 5 |
| 9 | T3 | Design an aggregation pipeline with filtering, grouping and sorting | 5 |
| 10 | T4 | Select Redis deployment architecture for failover versus horizontal sharding | 7 |
| 11 | T4 | Debug a fixed-window rate limiter and explain the atomicity requirement | 8 |
| 12 | T4 | Analyze a distributed-lock or inventory-update implementation | 10 |
| 13 | T5 | Compare `reduceByKey()` with `groupByKey()` and identify shuffle costs | 5 |
| 14 | T5 | Write or repair DataFrame filtering, column expressions and aggregation | 10 |
| 15 | T5 | Explain the effect of caching, partition count and batch intervals | 5 |
| 16 | T0–T5 | Recommend a solution for a growing event-processing application | 5 |
|  |  | Total | 100 |

### Practice Exam 3 — Integrated Challenge

| Question | Topic | Planned question | Marks |
| --- | --- | --- | --- |
| 1 | T0 | Evaluate conflicting reliability, scalability and maintainability requirements | 5 |
| 2 | T1 | Redesign a denormalized schema and explain its trade-offs | 5 |
| 3 | T2 | Analyze SQL involving multiple joins, `NULL` values and aggregation | 10 |
| 4 | T2 | Diagnose a dangerous modification statement and recommend a safe transaction | 5 |
| 5 | T2 | Explain why an index may fail to improve a particular query | 5 |
| 6 | T3 | Compare document, key-value, wide-column and graph models for a mixed workload | 5 |
| 7 | T3 | Trace and correct a MongoDB aggregation pipeline | 5 |
| 8 | T3 | Analyze a consistency–availability decision during a network partition | 5 |
| 9 | T4 | Determine the possible recovery state after Redis failures under different persistence policies | 8 |
| 10 | T4 | Diagnose stale-cache behavior and propose an invalidation strategy | 7 |
| 11 | T4 | Debug a leaderboard or lock-release implementation | 10 |
| 12 | T4 | Explain the difference between replication, sharding, failover and backups | 5 |
| 13 | T5 | Trace an RDD pipeline with duplicate values and tied word frequencies | 7 |
| 14 | T5 | Debug a DataFrame program involving column expressions and grouping | 8 |
| 15 | T5 | Diagnose a Spark performance problem involving shuffles, caching and driver memory | 5 |
| 16 | T5 | Compare classic Spark Streaming with Structured Streaming | 5 |
| 17 | T0–T5 | Design an integrated data solution and defend its technology choices | 5 |
|  |  | Total | 100 |

## 5. Answer keys and marking scheme

Each exam should have its own companion solutions document. The solutions will contain:

- Model answers for all questions, including correct SQL, MongoDB, Redis and PySpark code where appropriate.
- Reasoning, explaining why the answer is correct rather than just supplying the final result.
- Output tracing, with intermediate steps for queries, commands and code.
- Common wrong answers, explaining the misconception or trap involved.
- Partial-credit guidance, awarding marks for correct reasoning even when a minor syntax error occurs.
- Acceptable alternatives, where more than one solution is technically valid.
- A marking rubric, totaling 100 marks for each paper.

The answer keys will distinguish between requirements that are essential for correctness and implementation choices that depend on the workload or DBMS.

## 6. GitHub-ready document structure

I recommend organizing the six Markdown documents as follows:

text
```
practice-exams/
├── README.md
├── practice-exam-1.md
├── practice-exam-1-solutions.md
├── practice-exam-2.md
├── practice-exam-2-solutions.md
├── practice-exam-3.md
└── practice-exam-3-solutions.md
```

The `README.md` will summarize the syllabus coverage, exam duration, marks, recommended study approach and links to each paper and its solutions.

## 7. Quality checks before finalizing the papers

- Each exam contains exactly 100 marks and is designed for 120 minutes.
- All six topics are represented in every paper.
- No exam depends on lecture content absent from the supplied revision guides.
- Questions use consistent schemas, datasets, command syntax and assumptions.
- Code questions have unambiguous expected results or explicitly state where behavior can vary.
- Redis atomicity, persistence and distributed-lock limitations are represented accurately.
- Spark questions respect lazy evaluation, partitioning and shuffle semantics.
- The three papers vary in scenarios, code, reasoning and difficulty.
- Solutions match the questions exactly, with marks adding up correctly.

The exam plan is now defined. The next step is to turn this blueprint into the six complete Markdown documents, with the actual question statements and fully worked solutions.
