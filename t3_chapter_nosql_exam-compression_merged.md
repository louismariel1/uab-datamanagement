# T3 — NoSQL Databases & Data Modeling

## Complete Exam Compression — Data Management 2026–27

**Purpose:** Close-book exam preparation\
**Scope:** All seven supplied chunks merged into one chapter\
**Coverage:** SQL vs. NoSQL, ACID/BASE/CAP, the four NoSQL models, MongoDB CRUD and queries, aggregation, key-value databases, wide-column databases, and graph databases.

---

# 1\. SQL vs. NoSQL

## 1.1 SQL databases

SQL databases are traditionally relational databases that organize data into tables containing rows and columns.

**Key characteristics**

- **Structure:** Tables, rows and columns.
- **Schema:** Usually predefined, with specified columns and data types.
- **Relationships:** Represented using keys and relational constraints.
- **Integrity:** Commonly provides strong transactional guarantees through ACID transactions.
- **Scaling:** Traditionally associated with vertical scaling, although distributed SQL systems also exist.
- **Typical applications:** Banking, university systems, ERP and transaction processing.

## 1.2 NoSQL databases

NoSQL databases support data models beyond the traditional relational table model.

**Key characteristics**

- **Structure:** Documents, key-value pairs, wide-column data or graphs.
- **Schema:** Often flexible, allowing records to have different fields or structures.
- **Scaling:** Many systems are designed for horizontal scaling across multiple nodes.
- **Performance:** Often optimized for specific access patterns and high-volume workloads.
- **Availability:** Replication and distributed architectures can improve availability and fault tolerance.
- **Typical applications:** Social media, IoT, e-commerce, caching, recommendation systems and streaming platforms.

## 1.3 High-yield comparison

| Property | SQL | NoSQL |
| --- | --- | --- |
| Data organization | Tables | Multiple data models |
| Schema | Usually predefined | Often flexible |
| Relationships | Foreign keys, joins and constraints | Model-dependent |
| Scaling | Traditionally vertical | Often horizontal |
| Transactions | Commonly ACID | Varies; some support ACID transactions |
| Best suited for | Structured, relationship-intensive data | Diverse data and specialized large-scale workloads |
| Examples | PostgreSQL, MySQL | MongoDB, Redis, Cassandra, Neo4j |

**Exam trap:** NoSQL does not mean “no schema,” “no transactions” or “no consistency.” These capabilities depend on the particular database and its configuration.

## 1.4 Advantages of NoSQL

- **Scalability:** Adding servers can distribute increasing workloads.
- **Flexibility:** Different records can have different fields and nested structures.
- **Performance:** Specialized data models can optimize particular queries.
- **Availability:** Replication can keep services accessible during some failures.
- **Fault tolerance:** Distributed architectures can continue operating despite certain node failures.
- **Cost-effectiveness:** Some systems use open-source software and commodity hardware.

These are potential advantages, not guarantees that every NoSQL database is faster or cheaper than every SQL database.

## 1.5 Vertical vs. horizontal scaling

- **Vertical scaling (scale up):** Increase the resources of an existing server, such as CPU or RAM.
- **Horizontal scaling (scale out):** Add servers or nodes to distribute storage and processing.

Example: Upgrading a server from 16 GB to 64 GB RAM is vertical scaling. Adding three more database nodes is horizontal scaling.

---

# 2\. ACID, BASE and CAP

These concepts address different aspects of database correctness and distributed system behavior.

## 2.1 ACID — Transaction guarantees

| Property | Definition | Example |
| --- | --- | --- |
| **A — Atomicity** | A transaction completes entirely or not at all. | A bank transfer cannot commit only the debit. |
| **C — Consistency** | A transaction preserves defined integrity constraints. | Account balances and database constraints remain valid. |
| **I — Isolation** | Concurrent transactions are controlled to prevent improper interference. | Simultaneous transactions do not produce invalid intermediate results. |
| **D — Durability** | Committed changes persist despite failures. | A confirmed transfer is not lost after a restart. |

**Bank transfer example:** Transferring €100 requires debiting the sender and crediting the recipient. Atomicity ensures both operations commit together or both are rolled back.

## 2.2 BASE — Distributed-system approach

BASE describes an approach associated with many distributed NoSQL architectures.

- **B — Basically Available:** The system aims to continue responding despite certain failures.
- **S — Soft State:** Data may change as asynchronous updates and replication occur.
- **E — Eventual Consistency:** If updates stop and replication succeeds, replicas are expected to converge.

**Example:** One shopping-system replica shows a customer's new address while another temporarily shows the old address. After replication, both should show the new address.

BASE can accept temporary inconsistency when continued availability is important.

## 2.3 CAP theorem

CAP stands for **Consistency, Availability and Partition Tolerance**.

- **C — Consistency:** Every read receives the latest successful write or an error, according to the strong consistency model.
- **A — Availability:** Every request to a non-failing node receives a response, though the data may be stale.
- **P — Partition Tolerance:** The system continues operating despite network communication failures that divide nodes into groups unable to communicate.

### The CAP trade-off

When a network partition occurs, a distributed system cannot guarantee both strong consistency and availability for all affected operations while tolerating that partition.

| Design | Priority | Possible behavior during a partition |
| --- | --- | --- |
| **CP** | Consistency + partition tolerance | Some requests are rejected or delayed to prevent inconsistent results. |
| **AP** | Availability + partition tolerance | Requests continue to receive responses, potentially with stale or conflicting data. |

**Exam traps**

- CAP concerns behavior **during a network partition**.
- It does not mean that every database permanently chooses only two properties under all circumstances.
- CA behavior may be possible when there is no partition, but partition tolerance introduces the consistency–availability trade-off.
- Eventual consistency is not the same as immediate strong consistency.

## 2.4 ACID vs. BASE vs. CAP

| Concept | Main question |
| --- | --- |
| ACID | How are transactions kept correct and reliable? |
| BASE | How can a distributed system remain available while tolerating temporary inconsistency? |
| CAP | What trade-off arises between consistency and availability during a network partition? |

They are related but not interchangeable. A NoSQL database can support ACID transactions, and a relational database can operate in a distributed architecture.

---

# 3\. The Four Major NoSQL Data Models

Memorize: **Document, Key-Value, Column-Family, Graph.**

| Model | Structure | Typical uses | Example |
| --- | --- | --- | --- |
| Document | JSON-like documents with fields and nested data | Product catalogs, profiles, content management | MongoDB |
| Key-value | Unique keys associated with values | Caching, sessions, preferences | Redis |
| Column-family (wide-column) | Rows with flexible groups of columns | Distributed, high-volume data | Cassandra |
| Graph | Nodes, edges and properties | Social networks, recommendations, fraud detection | Neo4j |

**Memory aid**

- Document = records and objects.
- Key-value = fast lookup.
- Column-family = distributed data and grouped columns.
- Graph = relationships and connections.

## 3.1 Choosing the appropriate model

- Choose **document** when records have varying fields or nested structures.
- Choose **key-value** when data is retrieved primarily using a unique key.
- Choose **wide-column** when the workload benefits from partitioned, distributed data organized into flexible column families.
- Choose **graph** when relationships and traversals between entities are central to the workload.

Database choice depends on query patterns, consistency requirements, scale, data structure and operational constraints.

---

# 4\. Document Databases and MongoDB

## 4.1 Document database fundamentals

A document database stores self-describing records containing field-value pairs, commonly using JSON-like structures.

| Document concept | Meaning | Relational analogy |
| --- | --- | --- |
| Database | Contains collections | Database |
| Collection | Groups related documents | Table |
| Document | One record with fields and values | Row |
| Field | A named attribute | Column |

Documents can contain:

- Different fields within the same collection.
- Nested objects.
- Arrays.
- Values of different supported data types.

A flexible schema does not prevent an application from enforcing validation rules.

## 4.2 MongoDB essentials

MongoDB is a **document-oriented NoSQL database**.

- Stores documents in **BSON (Binary JSON)** format.
- Supports flexible document structures.
- Provides queries, indexes and aggregation.
- Organizes documents into collections.
- Supports CRUD operations and multi-stage aggregation pipelines.

**Important:** BSON is not identical to JSON. It is a binary-encoded format supporting additional data types.

## 4.3 CRUD operations

CRUD = **Create, Read, Update, Delete.**

| Operation | MongoDB method | Purpose |
| --- | --- | --- |
| Create | `insertOne()`, `insertMany()` | Add documents |
| Read | `find()` | Retrieve documents |
| Update | `updateOne()`, `updateMany()` | Modify documents |
| Delete | `deleteOne()`, `deleteMany()` | Remove documents |
| Aggregate | `aggregate()` | Process and summarize documents |

Examples use a `movies` collection unless stated otherwise.

### A. Insert documents

Insert one document:

```
db.students.insertOne({
  name: "Ali",
  age: 21
})
```

Insert multiple documents:

```
db.students.insertMany([
  { name: "Ali", age: 21 },
  { name: "Sara", age: 22 }
])
```

- `insertOne()` inserts one document.
- `insertMany()` inserts multiple documents.
- These operations can create a collection if it does not exist.

### B. Retrieve documents

Retrieve all documents:

```
db.movies.find()
```

Retrieve matching documents:

```
db.students.find({ age: 21 })
```

Format displayed results for readability:

```
db.movies.find().pretty()
```

`pretty()` affects presentation, not the stored data.

### C. Count documents

Count every document:

```
db.movies.countDocuments()
```

Count documents matching a condition:

```
db.movies.countDocuments({
  year: { $gt: 2000 }
})
```

### D. Update documents

Update one matching document:

```
db.movies.updateOne(
  { title: "The Matrix" },
  { $set: { director: "New Director" } }
)
```

Update every matching document:

```
db.movies.updateMany(
  { genre: "Adventure" },
  { $set: { genre: "Action" } }
)
```

`$set` assigns a new value to a field or creates the field if it does not exist.

| Method | Effect |
| --- | --- |
| `updateOne()` | Updates at most one matching document |
| `updateMany()` | Updates all matching documents |

### E. Upsert

An upsert updates a matching document or inserts a new document when no match exists.

```
db.students.updateOne(
  { name: "Ali" },
  { $set: { age: 22 } },
  { upsert: true }
)
```

- `upsert: false` is the default: no matching document means no insertion.
- `upsert: true`: MongoDB inserts a document if no match is found.

**Exam trap:** `updateOne()` does not update every matching document.

### F. Delete documents

Delete one matching document:

```
db.movies.deleteOne({
  title: "The Matrix"
})
```

Delete all matching documents:

```
db.movies.deleteMany({
  genre: "Comedy"
})
```

- `deleteOne()` deletes at most one matching document.
- `deleteMany()` deletes all matching documents.
- A filter determines which documents are affected.

---

# 5\. MongoDB Query Operators

## 5.1 Comparison operators

| Operator | Meaning | Example |
| --- | --- | --- |
| `$gt` | Greater than | `{ year: { $gt: 2000 } }` |
| `$gte` | Greater than or equal to | `{ year: { $gte: 2000 } }` |
| `$lt` | Less than | `{ year: { $lt: 2000 } }` |
| `$lte` | Less than or equal to | `{ year: { $lte: 2000 } }` |
| `$eq` | Equal to | `{ year: { $eq: 2000 } }` |
| `$ne` | Not equal to | `{ director: { $ne: "Nolan" } }` |
| `$in` | Matches a value in a specified list | `{ year: { $in: [1990, 2000] } }` |
| `$nin` | Does not match any value in a specified list | `{ year: { $nin: [1990, 2000] } }` |

Examples:

```
// Movies released after 2010
db.movies.find({
  year: { $gt: 2010 }
})

// Movies released from 1990 through 2020
db.movies.find({
  year: { $gte: 1990, $lte: 2020 }
})

// Movies not directed by Christopher Nolan
db.movies.find({
  director: { $ne: "Christopher Nolan" }
})
```

**Boundary trap**

- `$gt` excludes the boundary; `$gte` includes it.
- `$lt` excludes the boundary; `$lte` includes it.

## 5.2 Logical operators

| Operator | Meaning |
| --- | --- |
| `$and` | Every condition must be true |
| `$or` | At least one condition must be true |
| `$not` | Negates a condition |
| `$nor` | None of the specified conditions may be true |

Example — AND:

```
db.movies.find({
  $and: [
    { year: { $gt: 1990 } },
    { director: "The Wachowskis" }
  ]
})
```

For independent fields, implicit AND is usually simpler:

```
db.movies.find({
  year: { $gt: 1990 },
  director: "The Wachowskis"
})
```

Example — OR:

```
db.movies.find({
  $or: [
    { year: { $lt: 1990 } },
    { year: { $gt: 2000 } }
  ]
})
```

Example — NOT:

```
db.movies.find({
  year: { $not: { $eq: 2000 } }
})
```

## 5.3 Field existence: `$exists`

Check whether a field exists:

```
db.movies.find({
  rating: { $exists: true }
})
```

Check whether a field is missing:

```
db.movies.find({
  rating: { $exists: false }
})
```

**Exam trap:** `$exists: true` matches a present field even when its value is `null`. Field existence and having a non-null value are different conditions.

## 5.4 Array operators

| Operator | Meaning |
| --- | --- |
| `$in` | An array contains at least one specified value |
| `$all` | An array contains every specified value |
| `$size` | An array has exactly the specified number of elements |

Find movies with either Action or Drama:

```
db.movies.find({
  genres: { $in: ["Action", "Drama"] }
})
```

Find movies with both Action and Drama:

```
db.movies.find({
  genres: { $all: ["Action", "Drama"] }
})
```

Find movies with exactly two genres:

```
db.movies.find({
  genres: { $size: 2 }
})
```

**Memorize:** `$in` = ANY; `$all` = ALL; `$size` = exact length.

## 5.5 Sorting with `sort()`

- `1` = ascending.
- `-1` = descending.

```
// Oldest first
db.movies.find().sort({ year: 1 })

// Newest first
db.movies.find().sort({ year: -1 })
```

Sort using multiple fields:

```
db.movies.find().sort({
  genre: 1,
  year: -1
})
```

MongoDB sorts by `genre` first, then by `year` in descending order within the preceding sort order.

**Exam trap:** `1` means ascending; `-1` means descending.

---

# 6\. MongoDB Aggregation Pipeline

## 6.1 Definition

`aggregate()` processes documents through a sequence of pipeline stages. Each stage transforms, filters, groups or orders the documents and passes its output to the next stage.

General syntax:

```
db.movies.aggregate([
  { /* stage 1 */ },
  { /* stage 2 */ },
  { /* stage 3 */ }
])
```

## 6.2 Five important stages

| Stage | Purpose |
| --- | --- |
| `$match` | Filters documents |
| `$group` | Groups documents and calculates aggregate values |
| `$project` | Includes, excludes or reshapes fields |
| `$sort` | Sorts documents |
| `$limit` | Restricts the number of documents passed onward |

Memory aid: **Match → Group → Project → Sort → Limit.**

This is a memorization aid, not a mandatory pipeline order.

## 6.3 Example A — Count movies by genre

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

Explanation:

- `_id: "$genre"` groups documents by genre.
- `$sum: 1` adds one for every document in each group.
- `count` stores the resulting total.

**Exam trap:** In `$group`, `_id` defines the grouping key. It can be a field expression or a more complex grouping expression.

## 6.4 Example B — Top three highest-rated Sci-Fi movies

```
db.movies.aggregate([
  { $match: { genre: "Sci-Fi" } },
  { $sort: { rating: -1 } },
  { $limit: 3 }
])
```

Pipeline logic:

1. `$match` selects Sci-Fi movies.
2. `$sort` puts the highest ratings first.
3. `$limit` keeps only the first three results.

The example assumes the rating field is named `rating`.

## 6.5 Example C — Project selected fields

```
db.movies.aggregate([
  {
    $project: {
      title: 1,
      year: 1,
      _id: 0
    }
  }
])
```

This includes `title` and `year` while excluding `_id`.

## 6.6 `find()` vs. `aggregate()`

| `find()` | `aggregate()` |
| --- | --- |
| Retrieves matching documents | Processes documents through pipeline stages |
| Supports filtering and sorting | Supports filtering, grouping, reshaping and other transformations |
| Suitable for ordinary retrieval | Suitable for summaries and multi-stage processing |

---

# 7\. Key-Value Databases

## 7.1 Definition and structure

A key-value database stores values under unique keys, similar to a dictionary or map.

- **Key:** Identifier used to retrieve a value.
- **Value:** Data associated with the key; depending on the database, this can be a string, number, serialized object or another supported structure.

Example:

`"session_123" → session data`

## 7.2 Main characteristics

- Simple data model.
- Fast direct lookups by key.
- Often suitable for caching and session storage.
- Useful for independent records that do not require complex relational queries.

## 7.3 Basic operations

| Operation | Purpose |
| --- | --- |
| Insert | Add a key-value pair |
| Fetch | Retrieve a value by key |
| Update | Change the value associated with a key |
| Delete | Remove a key-value pair |

Example database: **Redis**.

**Exam rule:** Choose a key-value database when direct retrieval using a unique key is the dominant access pattern.

---

# 8\. Column-Family / Wide-Column Databases

## 8.1 Definition

Wide-column databases organize distributed data into rows identified by row keys, with flexible groups of columns organized into column families.

Example: **Apache Cassandra**.

### Important terminology

| Term | Meaning |
| --- | --- |
| Keyspace | Namespace and organizational container for tables and related database structures |
| Column | A named data attribute/value; some wide-column models also associate timestamp or version metadata |
| Column family | A logical grouping of related columns or a table-like data structure, depending on the system |
| Row | Data associated with a row key |
| Row key / partition key | Identifier or key used to locate and distribute data, depending on the database |

Rows in wide-column systems can have different sets of columns, depending on the data model.

## 8.2 Advantages and use cases

- Handles large, distributed datasets.
- Supports high-volume workloads.
- Can accommodate flexible column structures.
- Often suits time-series, event, telemetry and large-scale operational data.

**Exam rule:** Cassandra is a wide-column database, not a traditional relational database and not simply a conventional analytical column store.

## 8.3 Row-oriented vs. column-oriented analytical storage

Do not confuse wide-column NoSQL databases with traditional column-oriented analytical storage. The terms describe related but different storage concepts.

| Property | Row-oriented storage | Column-oriented analytical storage |
| --- | --- | --- |
| Organization | Values stored by record | Values grouped by column |
| Best suited for | Retrieving complete records | Scanning or aggregating selected columns |
| Typical workload | Transaction processing | Analytics and data warehousing |
| Example query | Retrieve one user's full record | Calculate balances across millions of records |

Example: An analytical query that reads only `current_balance` across millions of users may benefit from column-oriented storage because it can avoid reading unrelated columns.

**Exam rule:** Row-oriented storage is generally effective for retrieving complete records; column-oriented analytical storage is generally effective for scanning and aggregating selected columns.

---

# 9\. Graph Databases

## 9.1 Definition

A graph database represents data as **nodes, edges (relationships) and properties**.

- **Nodes:** Represent entities such as people, products or companies.
- **Edges / relationships:** Connect nodes and represent associations.
- **Properties:** Store attributes about nodes or relationships.

Example: A social network represents users as nodes and friendships as relationships.

## 9.2 Advantages and use cases

Graph databases are useful when relationships are central to the data and queries.

Applications include:

- Social networks.
- Recommendation systems.
- Fraud detection.
- Network analysis.
- Connected-data exploration.

They are designed to support relationship-oriented queries and graph traversal.

## 9.3 Two common graph models

| Feature | RDF graph | Property graph |
| --- | --- | --- |
| Full name | Resource Description Framework | Property graph |
| Main focus | Semantic meaning, data integration and interoperability | Practical querying and analysis of connected data |
| Basic structure | Subject–Predicate–Object triples | Nodes, relationships and properties |
| Identifiers | URIs commonly identify resources | Nodes typically have labels and key-value properties |
| Relationships | Predicates connect resources | Named, directed relationships can have properties |
| Typical uses | Semantic Web, linked data, reasoning | Recommendations, graph analytics and traversal |

## 9.4 RDF graphs

RDF stands for **Resource Description Framework**.

It is a standard for representing and exchanging data, supporting semantic descriptions and integration across different sources.

The basic unit is a triple:

**(Subject, Predicate, Object)**

Example: `Alice — knows — Bob`

- Subject = Alice.
- Predicate = knows.
- Object = Bob.

A URI (Uniform Resource Identifier) identifies a resource so that different systems can refer to it consistently.

**Memorize:** RDF = semantic data + integration + triples + reasoning.

## 9.5 Property graphs

A property graph contains labeled nodes, directed relationships and key-value properties.

Example:

`(Alice:Person) —[WORKS_AT {since: 2024}]→ (Company)`

Interpretation:

- Alice and Company are nodes.
- `Person` is a node label.
- `WORKS_AT` is the relationship type.
- The arrow represents direction.
- `since: 2024` is a relationship property.

Relationships can have a type, direction, start node, end node and properties.

**Memorize:** Property graph = nodes + labeled, directed relationships + properties.

---

# 10\. High-Yield Comparisons and Exam Traps

| Comparison | Correct distinction |
| --- | --- |
| SQL vs. NoSQL | Relational tables vs. multiple data models |
| Vertical vs. horizontal scaling | Upgrade one server vs. add servers |
| ACID vs. BASE | Transaction guarantees vs. an availability-oriented distributed approach |
| CAP consistency vs. availability | Strong consistency vs. responding during partitions |
| Document vs. key-value | Field-aware document queries vs. primarily key-based retrieval |
| Document vs. graph | Entity attributes vs. relationship-centered data |
| `$gt` vs. `$gte` | Strictly greater vs. greater than or equal |
| `$in` vs. `$all` | At least one listed array value vs. every listed value |
| `$exists: true` vs. non-null | Field present vs. field having a non-null value |
| `updateOne()` vs. `updateMany()` | At most one update vs. all matching updates |
| `deleteOne()` vs. `deleteMany()` | At most one deletion vs. all matching deletions |
| `sort({field: 1})` vs. `sort({field: -1})` | Ascending vs. descending |
| `$match` vs. `$group` | Filter documents vs. group and aggregate |
| MongoDB vs. Cassandra | Document database vs. wide-column database |
| RDF vs. property graph | Semantic triples vs. labeled nodes and directed relationships |

Additional traps:

1. A collection contains documents; a document contains fields.
2. BSON is not identical to JSON.
3. Flexible schema does not mean there is no structure or validation.
4. NoSQL databases can support ACID transactions.
5. CAP's consistency–availability trade-off concerns network partitions.
6. `$sum: 1` inside `$group` commonly counts documents in each group.
7. The five aggregation stages listed in this compression do not have to appear in one fixed order.
8. The best database depends on the application's requirements.

---

# 11\. Integrated Exam-Practice Questions with Model Answers

These questions are synthesized from the supplied lecture material. They are **additional practice questions, not verified lecturer review questions**.

## Question 1 — SQL vs. NoSQL (6 marks)

Explain three differences between SQL and NoSQL and give one appropriate application for each.

**Model answer:**

1. SQL uses relational tables; NoSQL supports document, key-value, wide-column and graph models.
2. SQL commonly uses a predefined schema; NoSQL often permits more flexible structures.
3. SQL has traditionally been associated with vertical scaling; many NoSQL systems are designed for horizontal scaling.

A banking transaction system may use SQL for structured relationships and transactional integrity. A social-media platform may use NoSQL to manage diverse data and distributed workloads.

## Question 2 — NoSQL advantages (5 marks)

A social-media company experiences increasing traffic and frequently adds profile fields. Explain three reasons why NoSQL may be suitable.

**Model answer:**

- Horizontal scalability can distribute growing workloads.
- Flexible schemas accommodate new fields and varying document structures.
- Some systems provide high performance and availability through workload-specific optimization and replication.

Suitability depends on the actual database and workload.

## Question 3 — Identify the NoSQL model (8 marks)

Match each application to a suitable model.

a. Product records have different fields.

b. A session is retrieved using its unique identifier.

c. A social network analyzes friendships.

d. A distributed application stores large volumes of flexible, column-family data.

**Model answer:**

- a. Document.
- b. Key-value.
- c. Graph.
- d. Wide-column / column-family.

## Question 4 — ACID (8 marks)

Define ACID and explain why atomicity matters in a bank transfer.

**Model answer:**

- Atomicity: All operations succeed or none do.
- Consistency: Integrity constraints remain satisfied.
- Isolation: Concurrent transactions do not interfere improperly.
- Durability: Committed changes persist after failures.

Atomicity prevents the sender from being debited without the recipient being credited.

## Question 5 — BASE and eventual consistency (6 marks)

One replica shows a customer's new address while another temporarily shows the old address. Explain this using BASE.

**Model answer:**

- Basically Available: The system aims to respond to requests.
- Soft State: Replica contents may change as updates propagate.
- Eventual Consistency: Successful synchronization should eventually make the replicas agree.

## Question 6 — CAP (8 marks)

Explain CP and AP behavior during a network partition.

**Model answer:**

- CP prioritizes consistency and partition tolerance; it may reject or delay some requests.
- AP prioritizes availability and partition tolerance; it continues responding but may return stale data.

Neither is universally superior. The correct choice depends on application requirements.

## Question 7 — Select a database (8 marks)

Choose SQL or NoSQL and justify your choices.

a. A university stores students, modules and enrollments.

b. An IoT platform ingests millions of sensor readings.

c. A banking application processes transfers.

**Model answer:**

- a. SQL is a strong candidate because structured relationships and integrity constraints are central.
- b. NoSQL, such as a suitable wide-column database, may be appropriate for distributed ingestion and large workloads.
- c. SQL is a strong candidate because ACID transactions are important, although a NoSQL database with appropriate transactional guarantees could also work.

## Question 8 — Correct a statement (4 marks)

“NoSQL databases have no schema, are always eventually consistent and cannot support transactions.”

**Model answer:**

The statement is incorrect:

- NoSQL databases often have flexible schemas but can enforce validation.
- Consistency guarantees depend on the system and configuration.
- Some NoSQL databases support ACID transactions.

## Question 9 — Scaling (5 marks)

A server is upgraded from 16 GB to 64 GB RAM. Identify the scaling type and explain the alternative.

**Model answer:**

This is vertical scaling because resources are added to an existing server. Horizontal scaling would add servers or nodes.

## Question 10 — ACID, BASE and CAP (6 marks)

Explain how the three concepts differ.

**Model answer:**

- ACID describes transaction guarantees.
- BASE describes an approach that prioritizes availability and tolerates temporary inconsistency.
- CAP describes the consistency–availability trade-off during a network partition.

They guide different aspects of database design.

## Question 11 — MongoDB query operators (6 marks)

Write queries to:

a. Find movies released after 2010.

b. Find movies released after 1990 and before 2020.

c. Find movies directed by someone other than Christopher Nolan.

**Model answer:**

```
// a
db.movies.find({
  year: { $gt: 2010 }
})

// b
db.movies.find({
  year: { $gt: 1990, $lt: 2020 }
})

// c
db.movies.find({
  director: { $ne: "Christopher Nolan" }
})
```

## Question 12 — Array operators (6 marks)

Explain the difference between `$in`, `$all` and `$size`.

**Model answer:**

- `$in`: Matches an array containing at least one specified value.
- `$all`: Matches an array containing every specified value.
- `$size`: Matches an array with exactly the specified number of elements.

## Question 13 — MongoDB updates and deletions (6 marks)

Explain the difference between `updateOne()`, `updateMany()`, `deleteOne()` and `deleteMany()`.

**Model answer:**

`updateOne()` updates at most one matching document; `updateMany()` updates all matching documents. `deleteOne()` deletes at most one matching document; `deleteMany()` deletes all matching documents.

## Question 14 — Aggregation (8 marks)

Write an aggregation pipeline that counts movies in each genre.

**Model answer:**

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

`$group` groups movies by genre. `$sum: 1` counts documents within each group.

## Question 15 — Graph models (6 marks)

Compare RDF graphs and property graphs.

**Model answer:**

RDF uses Subject–Predicate–Object triples and emphasizes semantic meaning, interoperability and data integration. Property graphs use nodes, directed relationships and key-value properties, emphasizing practical queries and analysis of connected data.

---

# 12\. Last-Minute Memorization Sheet

| Term | Memorize |
| --- | --- |
| SQL | Tables, relationships, predefined schema |
| NoSQL | Multiple models, often flexible structure |
| Four NoSQL models | Document, Key-value, Wide-column, Graph |
| Vertical scaling | Upgrade one server |
| Horizontal scaling | Add servers or nodes |
| ACID | Atomicity, Consistency, Isolation, Durability |
| BASE | Basically Available, Soft State, Eventual Consistency |
| CAP | Consistency, Availability, Partition Tolerance |
| CP | Preserve consistency during a partition, potentially rejecting requests |
| AP | Continue responding during a partition, potentially with stale data |
| MongoDB | Document database; BSON |
| Collection | Contains documents |
| `insertOne()` | Insert one document |
| `insertMany()` | Insert multiple documents |
| `find()` | Retrieve documents |
| `countDocuments()` | Count documents |
| `updateOne()` | Update at most one match |
| `updateMany()` | Update all matches |
| `deleteOne()` | Delete at most one match |
| `deleteMany()` | Delete all matches |
| `$gt`, `$gte` | Greater than; greater than or equal |
| `$lt`, `$lte` | Less than; less than or equal |
| `$in`, `$all` | Any listed value; all listed values |
| `$exists` | Check field presence |
| `$size` | Exact array length |
| `sort: 1` | Ascending |
| `sort: -1` | Descending |
| `$match` | Filter documents |
| `$group` | Group and aggregate |
| `$sum: 1` | Count documents within a group |
| `$project` | Select or reshape fields |
| `$limit` | Restrict the number of results |
| Key-value | Fast retrieval using a key |
| Wide-column | Distributed data organized into flexible column families |
| Row-oriented storage | Efficient access to complete records |
| Column-oriented analytical storage | Efficient scans of selected columns |
| Graph database | Nodes, edges and properties |
| RDF | Subject, Predicate, Object |
| URI | Uniform Resource Identifier |
| Property graph | Labeled nodes and directed relationships |

---

# 13\. Final Exam Strategy

If you have very little revision time, prioritize these areas:

1. **Memorize the four NoSQL models**, their structures, use cases and examples.
2. **Know ACID, BASE and CAP**, especially the difference between CP and AP during a partition.
3. **Practice MongoDB syntax:** CRUD, comparison operators, logical operators, arrays, `$exists`, updates, deletions and sorting.
4. **Understand aggregation pipelines:** `$match`, `$group`, `$project`, `$sort`, `$limit` and `$sum: 1`.
5. **Distinguish the data models:** MongoDB vs. Redis vs. Cassandra vs. Neo4j.
6. **Review graph concepts:** RDF triples, URIs, property graphs, nodes, relationships and properties.
7. **Avoid common traps:** `$gt` vs. `$gte`, `$in` vs. `$all`, `updateOne()` vs. `updateMany()`, and `deleteOne()` vs. `deleteMany()`.

**Final takeaway:** SQL and NoSQL are different approaches to data management, not a simple choice between consistency and inconsistency. Choose a database model according to the data structure, access patterns, scalability, relationship requirements, transaction guarantees and availability needs.

**Scope note:** This document merges the seven supplied chunks. No lecturer review questions were explicitly identified in the supplied material, so the integrated practice questions above are synthesized study questions rather than verified lecturer questions. Where source terminology was ambiguous, the compression distinguishes wide-column NoSQL databases from conventional column-oriented analytical storage.
