# T4. NoSQL Databases — Exam Compression (Chunk 1)

Data Management (2026–27) · Close-book exam preparation · Chunk 1 of T4

This compression covers the material received so far. The lecturer's review questions have not appeared in this chunk. The questions in the final section are additional exam-practice questions created from the lecture content, with model solutions.

## 1. Learning objectives

You must be able to:

- Distinguish SQL and NoSQL databases.
- Explain the advantages and properties of NoSQL databases.
- Identify the four major NoSQL data models.
- Understand the fundamentals of MongoDB.
- Perform basic MongoDB CRUD and aggregation operations.

Note: MongoDB and its operations are listed as objectives, but their detailed explanations have not yet appeared in this chunk.

## 2. SQL vs. NoSQL

### SQL (relational databases)

- Data model: Tables containing rows and columns.
- Schema: Fixed, defined before storing information.
- Relationships: Explicit relationships between entities.
- Consistency: Commonly associated with strong consistency and transactional integrity.
- Scaling: Typically vertical scaling — increasing the resources of a single server.
- Best suited for: Structured data and systems where relationships and data integrity are critical.
- Examples: Banking, ERP systems, university systems.

### NoSQL (non-relational databases)

- Data model: Multiple models, including document, key-value, column-family and graph.
- Schema: Flexible or less-schema; accommodates changing data structures.
- Scaling: Designed to support horizontal scaling — adding servers to a cluster.
- Performance: Often optimized for high-volume, low-latency read/write workloads.
- Availability: Many systems support high availability and fault tolerance in distributed environments.
- Best suited for: Large-scale applications handling diverse, semi-structured or unstructured data.
- Examples: Social media, IoT, e-commerce and streaming platforms.

### Comparison table

| Property | SQL | NoSQL |
| --- | --- | --- |
| Data organization | Tables | Multiple data models |
| Schema | Fixed | Flexible |
| Scaling | Typically vertical | Typically horizontal |
| Consistency | Strong consistency commonly supported | Depends on system; eventual consistency is common in distributed designs |
| Relationships | Explicit relational structure | Varies by data model |
| Typical use | Structured, relationship-intensive data | Large-scale, diverse or rapidly changing data |

Exam distinction: SQL and NoSQL are not simply “consistent” versus “inconsistent.” NoSQL systems differ in their consistency guarantees, and many support strong consistency for particular operations or configurations.

## 3. Advantages and properties of NoSQL

| Property | Definition | Why it matters |
| --- | --- | --- |
| Scalability | Capacity can be increased horizontally by adding servers | Supports growing workloads and datasets |
| Flexibility | Data can be stored without requiring one rigid, universal schema | Accommodates changing and diverse data |
| High performance | Systems may optimize particular read/write access patterns | Enables low-latency operations |
| Availability | Services can remain accessible despite certain failures | Reduces downtime |
| Fault tolerance | The system can continue operating despite failures of individual nodes | Improves resilience |
| Cost-effectiveness | Some systems are open-source and can run on commodity hardware | May reduce licensing and infrastructure costs |

### Key concept: horizontal vs. vertical scaling

- Vertical scaling: Upgrade an existing server, for example, by adding RAM or CPU capacity.
- Horizontal scaling: Add more servers or nodes to distribute the workload.

NoSQL databases are often designed for horizontal scaling, although actual capabilities depend on the database and its architecture.

## 4. The four major NoSQL data models

Memorize these four categories and recognize their basic structures.

Types of NoSQL databases - Choosing an AWS NoSQL Database

1. Document databases

Store records as documents, often in JSON-like formats. Fields and document structures can vary.

Typical use: product catalogs, user profiles and content management.

What Is a NoSQL Database? A Guide to Modern Data Storage

2. Key-value databases

Store values associated with unique keys. Access commonly occurs by looking up a key.

Typical use: caching, sessions and fast lookups.

What Is a NoSQL Database? A Guide to Modern Data Storage

3. Column-family databases

Organize data into rows and flexible groups of columns, suited to distributed, large-scale datasets.

Typical use: large analytical or time-series workloads.

Codelab - Build a Movie Recommendation Chatbot using Neo4j and Vertex AI | Google Codelabs

4. Graph databases

Represent data as nodes and relationships (edges), often with properties.

Typical use: social networks, recommendation systems and relationship analysis.

Memory aid: Document = records; Key-value = lookup; Column-family = grouped columns; Graph = relationships.

## 5. ACID vs. BASE

These concepts describe different priorities in database transaction processing and distributed data systems.

### ACID — commonly associated with relational transactions

- A — Atomicity: A transaction completes entirely or not at all.
- C — Consistency: A transaction preserves defined integrity constraints.
- I — Isolation: Concurrent transactions are controlled so they do not interfere improperly.
- D — Durability: Committed changes persist, even after a failure.

### BASE — an approach associated with many distributed NoSQL systems

- B — Basically Available: The system aims to remain responsive despite some failures.
- S — Soft State: State may change over time because of asynchronous updates.
- E — Eventual Consistency: Replicas are expected to converge if updates stop and synchronization proceeds successfully.

| ACID | BASE |
| --- | --- |
| Emphasizes transaction correctness and integrity | Emphasizes availability and distributed operation |
| Strong transactional guarantees | May tolerate temporary inconsistencies |
| Committed changes are durable | State may change through background synchronization |
| Often used for critical transactional workloads | Often used for large-scale distributed workloads |

Important exam qualification: ACID and BASE are useful conceptual contrasts, not mutually exclusive categories. NoSQL databases can support ACID transactions, and relational databases can be distributed.

## 6. CAP theorem

The lecture introduces three properties of distributed systems:

- C — Consistency: Every read receives the latest successful write, or an error, according to the strong consistency model.
- A — Availability: Every request to a non-failing node receives a response, although it may not contain the latest data.
- P — Partition tolerance: The system continues operating despite communication failures that split nodes into groups unable to communicate.

### Core principle

When a network partition occurs, a distributed system must choose between guaranteeing consistency and guaranteeing availability for affected operations. It cannot guarantee both simultaneously in the strict CAP sense while also tolerating that partition.

- A CP-oriented system may reject or delay some requests to preserve consistency.
- An AP-oriented system may continue answering requests while allowing temporary inconsistencies.

Do not interpret CAP as saying that every system permanently chooses only two properties under all conditions. The trade-off concerns behavior during network partitions.

### Relationship between ACID, BASE and CAP

| Concept | Main question |
| --- | --- |
| ACID | How are transactions kept correct and reliable? |
| BASE | How can distributed systems prioritize availability and tolerate temporary inconsistency? |
| CAP | What trade-off arises between consistency and availability during a network partition? |

These concepts are related but are not interchangeable.

## 7. SQL recap relevant to this chapter

The lecture recaps these SQL fundamentals:

- `INSERT`, `SELECT`, `UPDATE`, `DELETE` — CRUD operations.
- `GROUP BY` and `HAVING` — aggregation and filtering groups.
- `JOIN` — combining data from related tables.
- Indexes — improve lookup and query performance by reducing the amount of data that must be searched in suitable queries.

NoSQL extends the database landscape with different data models and scaling approaches; it does not make SQL concepts obsolete.

# Exam-practice questions with model solutions

These are newly created practice questions, not lecturer review questions.

## Question 1 — SQL vs. NoSQL (6 marks)

Explain three differences between SQL and NoSQL databases and give one suitable application for each.

Model solution:

1. Data structure: SQL uses tables with rows and columns; NoSQL supports document, key-value, column-family and graph models.
2. Schema: SQL commonly requires a predefined schema; NoSQL often permits more flexible data structures.
3. Scaling: SQL systems traditionally scale vertically, whereas many NoSQL systems are designed for horizontal scaling.

Examples: A banking transaction system may use a relational database to enforce transactional integrity. A social-media platform may use NoSQL to manage diverse data and large distributed workloads.

## Question 2 — Advantages of NoSQL (5 marks)

A social-media company experiences rapidly growing traffic and frequently adds new profile fields. Explain three reasons why a NoSQL database might be appropriate.

Model solution:

- Horizontal scalability: Additional nodes can help distribute growing workloads.
- Schema flexibility: New profile fields can be introduced without requiring every record to follow an identical rigid structure.
- Performance and availability: Some NoSQL systems are optimized for high-volume access and can replicate data to improve availability.

The suitability depends on the selected database and workload; NoSQL is not automatically faster or cheaper in every situation.

## Question 3 — Identify the data model (8 marks)

Match each scenario to the most appropriate NoSQL data model and justify your answer.

a. A shopping application stores product descriptions whose fields differ between products.

b. A system retrieves a cached session using a unique session identifier.

c. A social network analyzes friendships and connections between users.

d. A distributed system stores enormous volumes of records organized into column families.

Model solution:

- a. Document: Product information can be stored in flexible documents.
- b. Key-value: A session identifier can be used as the key for retrieving its value.
- c. Graph: Users are nodes and friendships are edges.
- d. Column-family: Data is organized into rows and groups of columns.

## Question 4 — ACID definitions (8 marks)

Define each ACID property and explain why atomicity matters in a bank transfer.

Model solution:

- Atomicity: All operations in a transaction succeed, or none do.
- Consistency: Integrity constraints remain satisfied.
- Isolation: Concurrent transactions are controlled to prevent improper interference.
- Durability: Committed changes survive failures.

In a transfer, the sender's account should not be debited without the corresponding recipient credit. Atomicity ensures that the debit and credit are committed together or rolled back together.

## Question 5 — BASE and eventual consistency (6 marks)

A distributed shopping application updates a customer's address. One replica displays the new address while another briefly displays the old address. Explain this behavior using BASE.

Model solution:

- Basically Available: The system aims to continue responding to requests.
- Soft State: Replica contents may change as asynchronous updates propagate.
- Eventual Consistency: If updates stop and replication succeeds, the replicas are expected to converge to the same address.

Temporary inconsistency may be an accepted trade-off, but it must be appropriate for the application's requirements.

## Question 6 — CAP theorem (8 marks)

During a network partition, a distributed database must decide how to handle requests. Explain the difference between CP and AP behavior.

Model solution:

- CP behavior: The system prioritizes consistency and partition tolerance. It may refuse or delay some requests rather than return potentially stale data.
- AP behavior: The system prioritizes availability and partition tolerance. It continues responding to requests, potentially returning stale or conflicting data temporarily.

Neither choice is universally superior. The correct choice depends on whether the application prioritizes consistent answers or continued service during a partition.

## Question 7 — Scenario-based database selection (8 marks)

Choose SQL or NoSQL for each application and justify your choice.

a. A university database stores students, modules and enrollments.

b. An IoT platform receives millions of sensor readings from distributed devices.

c. A banking system processes account transfers.

Model solution:

- a. SQL: Structured entities, relationships and integrity constraints are central.
- b. NoSQL may be appropriate: Distributed ingestion, horizontal scaling and high data volumes may favor a suitable NoSQL model. The exact choice depends on the read/write and analytical requirements.
- c. SQL is a strong candidate: ACID transactions and consistency are important for transfers. A NoSQL database with suitable transactional guarantees could also work.

## Question 8 — Correct a statement (4 marks)

Evaluate the following claim: “NoSQL databases have no schema, are always eventually consistent, and cannot support transactions.”

Model solution:

The claim is incorrect in all three respects:

- NoSQL databases often have flexible schemas, but applications can still enforce structure and validation.
- Consistency guarantees depend on the database, configuration and operation.
- Some NoSQL databases support ACID transactions.

## Question 9 — Explain the scaling difference (5 marks)

A database server is upgraded from 16 GB to 64 GB of RAM. Is this vertical or horizontal scaling? What change would represent the other type?

Model solution:

The upgrade is vertical scaling because the existing server receives more resources. Horizontal scaling would involve adding more servers or nodes to distribute the workload.

## Question 10 — Short integrated question (6 marks)

Explain how ACID, BASE and CAP differ and how they relate to database design decisions.

Model solution:

- ACID describes transaction guarantees: atomicity, consistency, isolation and durability.
- BASE describes an approach that prioritizes availability and tolerates temporary inconsistency in distributed systems.
- CAP describes the trade-off between consistency and availability when a network partition occurs.

They help guide design choices according to application requirements, including transaction correctness, availability and distributed operation. They are related concepts, but they address different questions.

## 8. Last-minute memorization sheet

| Term | Remember this |
| --- | --- |
| SQL | Tables, predefined schema, relationships |
| NoSQL | Multiple models, flexible structure, often horizontal scaling |
| Four NoSQL models | Document, key-value, column-family, graph |
| ACID | Atomicity, Consistency, Isolation, Durability |
| BASE | Basically Available, Soft State, Eventual Consistency |
| CAP | Consistency, Availability, Partition tolerance |
| Vertical scaling | Upgrade one server |
| Horizontal scaling | Add servers or nodes |
| Eventual consistency | Replicas converge after updates propagate |
| Index | Helps speed up suitable queries |

## 9. Lecturer review questions

No lecturer review questions were explicitly included in Chunk 1. The exam-practice questions above are additional questions and must not be mistaken for questions extracted from the lecture.

When you submit Chunk 2, I will produce its own exam compression, preserve any lecturer review questions exactly as written, and keep the material separate until you explicitly mark the final chunk. Then we can merge all chunks into one consolidated T4 chapter exam compression.
