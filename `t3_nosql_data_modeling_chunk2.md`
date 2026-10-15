Writing

# T3 — NoSQL Data Modeling: Exam Compression (Chunk 2)

## 1\. CAP Theorem

CAP = **Consistency, Availability, Partition Tolerance**.

- **Consistency (C):** Every read receives the most recent write or an error.
- **Availability (A):** Every request receives a response, though it may not contain the latest data.
- **Partition Tolerance (P):** The system continues operating despite network communication failures between nodes.

**Exam rule:** When a network partition occurs, a distributed system must trade off consistency and availability (CP vs. AP). In practice, CA without partition tolerance is not achievable during a network partition.

## 2\. The Four NoSQL Models

| Model | Best for | Example |
| --- | --- | --- |
| Document | Flexible, structured/semi-structured data | MongoDB |
| Key-Value | Fast lookups, caching, sessions | Redis |
| Column-Family (wide-column) | Large-scale analytics and distributed data | Cassandra |
| Graph | Highly connected data and relationships | Neo4j |

### When to Use Each Model

- **Document:** Product catalogs, content management, flexible records.
- **Key-Value:** Caching, sessions, user preferences.
- **Column-Family:** Data warehouses, large-scale analytics, high-volume distributed workloads.
- **Graph:** Social networks, recommendation engines, fraud detection.

**Memory aid:** Document = objects; Key-Value = lookup; Column-Family = scale and analytics; Graph = relationships.

## 3\. Document Databases

A **document database** is a NoSQL database that stores data as documents (typically JSON-like structures) instead of relational rows and columns.

### Key Concepts

- **Document:** A self-describing record containing field-value pairs.
- **Collection:** A group of related documents.
- **Database:** Contains collections.
- **Flexible schema:** Documents in the same collection do not need identical fields.
- Documents can contain nested objects and arrays, making complex data easier to represent.

**Relational analogy:**

| Relational database | Document database |
| --- | --- |
| Table | Collection |
| Row | Document |
| Column | Field |
| Database | Database |

## 4\. MongoDB

**MongoDB** is a NoSQL document database designed for flexible data models and scalable data storage.

### Key Facts

- Stores documents in **BSON (Binary JSON)** format.
- Supports dynamic/flexible schemas.
- Supports ad hoc queries, indexing, and aggregation.
- Uses collections to group documents.
- Suitable for complex, evolving data structures.

### Main Operations — CRUD + Aggregation

| Operation | Purpose |
| --- | --- |
| `insert` | Add documents |
| `find` | Retrieve documents |
| `update` | Modify documents |
| `delete` | Remove documents |
| `aggregate` | Process and summarize data through pipeline stages |

Typical MongoDB shell examples:

```
db.students.insertOne({ name: "Ali", age: 21 })

db.students.find({ age: 21 })

db.students.updateOne(
  { name: "Ali" },
  { $set: { age: 22 } }
)

db.students.deleteOne({ name: "Ali" })

db.students.aggregate([
  { $group: { _id: "$age", count: { $sum: 1 } } }
])
```

## 5\. High-Yield Exam Comparisons

### Document vs. Relational Database

- Relational: predefined table structure, rows, columns, relationships.
- Document: JSON-like documents, flexible fields, nested structures.
- Use relational databases when structured relationships and relational constraints are central.
- Use document databases when records vary in structure or naturally contain nested data.

### Document vs. Key-Value

- Document: understands document fields and supports queries over them.
- Key-Value: retrieves a value primarily by its unique key; ideal for simple, fast lookups.

### Document vs. Graph

- Document: focuses on storing entities and their attributes.
- Graph: focuses on entities (nodes) and relationships (edges).

## 6\. Common Exam Traps

1. **BSON is not identical to JSON.** BSON is a binary-encoded format with additional data types.
2. **Flexible schema does not mean no structure.** Applications can still enforce validation rules.
3. **A collection is not a document.** A collection contains documents.
4. **MongoDB is document-oriented**, not a key-value, column-family, or graph database.
5. **Cassandra is a wide-column/column-family database**, not a traditional relational column store.
6. **CAP is about behavior during network partitions.** It does not simply mean a system can choose any two properties under all circumstances.

## 7\. Rapid Recall — Memorize These

- CAP = Consistency, Availability, Partition Tolerance.
- Four NoSQL models = Document, Key-Value, Column-Family, Graph.
- MongoDB = Document database; BSON; flexible schema.
- Redis = Key-Value database; caching and sessions.
- Cassandra = Wide-column database; distributed workloads and analytics.
- Neo4j = Graph database; connected data.
- Collection contains documents; documents contain fields.
- MongoDB operations = Insert, Find, Update, Delete, Aggregate.

## 8\. Self-Test

1. Which CAP properties conflict during a network partition? **Consistency and Availability.**
2. Which NoSQL model is best for social-network relationships? **Graph.**
3. What format does MongoDB use to store documents? **BSON.**
4. What is the MongoDB equivalent of a relational table? **Collection.**
5. Which model is ideal for caching and sessions? **Key-Value.**
6. Which database is a common wide-column example? **Cassandra.**
7. Must every document in a MongoDB collection have identical fields? **No.**
8. Which MongoDB operation summarizes data using pipeline stages? **Aggregate.**
