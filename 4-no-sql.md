# 1\. NoSQL Databases: Introduction

 - **SQL/relational databases** use tables, rows, columns, fixed schemas, and defined relationships. They are best for structured data and strong consistency.
- **NoSQL databases** use flexible data models and are designed for large-scale, diverse, semi-structured, or unstructured data.
- Key NoSQL characteristics:
  - **Horizontal scalability** — add more servers/nodes.
  - **Flexible schemas** — data structures can change easily.
  - **High performance** — fast read/write operations.
  - **High availability and fault tolerance**.
  - **Cost-effective**, often using open-source technologies.

 ### 2\. SQL vs. NoSQL

 | SQL | NoSQL |
| --- | --- |
| Tables/rows/columns | Multiple data models |
| Fixed schema | Flexible schema |
| Usually vertical scaling | Usually horizontal scaling |
| Strong consistency | Often eventual consistency |
| Structured data | Semi/unstructured data |
| Examples: banking, ERP | Examples: social media, IoT, e-commerce |

### 3\. ACID vs. BASE

 - **ACID:** Atomicity, Consistency, Isolation, Durability → focuses on **strong consistency and correctness**.
- **BASE:** Basically Available, Soft State, Eventual Consistency → focuses on **availability, scalability, and flexibility**.

 ### 4\. CAP Theorem

 In a distributed system, network partitions create a trade-off between:

 - **Consistency**
- **Availability**
- **Partition Tolerance**

 NoSQL systems commonly emphasize **availability and scalability**, while accepting eventual consistency when appropriate.

 ### 5\. Four NoSQL Data Models

 1. **Document** — flexible structured data; example: **MongoDB**.
2. **Key-Value** — very fast lookups and caching; example: **Redis**.
3. **Column-Based/Column-Family** — analytics and big-data workloads; example: **Cassandra**.
4. **Graph** — highly connected data; example: **Neo4j**.

 **Key idea:** Different NoSQL models solve different problems.

 ### 6\. When to Use Each Model

 - **Document:** product catalogs, content management, applications with changing/flexible data structures.
- **Key-Value:** sessions, caching, and user preferences.
- **Column-Based:** analytics, large-scale data processing, and workloads that frequently access particular columns.
- **Graph:** social networks, recommendation engines, fraud detection, and other highly connected data.

---

 # 7\. Document Databases

 A **document database** stores data as **JSON-like documents** rather than traditional rows and columns.

 - A **document** is a self-describing record containing **field-value pairs**.
- Documents can be grouped into **collections**.
- A collection is similar to a **table** in a relational database.
- A document is roughly similar to a **row**.
- Documents in the same collection **do not need to have exactly the same fields**, because the schema is flexible.

---

 # 8\. MongoDB

 **MongoDB** is a NoSQL document database designed to store, manage, and retrieve large amounts of data.

 - Uses a **flexible document-oriented model** with dynamic schemas.
- Stores data in **BSON (Binary JSON)** format.
- Supports complex data structures.
- Provides querying, indexing, ad-hoc queries, and aggregation.
- Main operations include:
  - **Insert**
  - **Find**
  - **Update**
  - **Delete**
  - **Aggregate**

---

 # 9\. MongoDB: Inserting Data

 - **`insertOne()`** → adds a single document.
- **`insertMany()`** → adds multiple documents at once.
- These operations can initialize/populate a collection.

---

 # 10\. MongoDB: Counting Documents

 - **`countDocuments()`** → counts documents in a collection.

```
db.collectionName.countDocuments()
```

---

 # 11\. MongoDB: Retrieving Documents

 - **`find()`** → retrieves documents from a collection.
- `find()` with no query criteria returns all documents.
- **`find().pretty()`** → formats the output to make it easier to read.

---

 # 12\. Conditional Queries

 MongoDB's **`find(condition)`** allows documents to be filtered according to conditions.

 General form:

```
db.collectionName.find({
  key: { ConditionalOperator: value }
})
```

 Conditional operators allow comparisons and other filtering operations, such as finding movies released after a particular year.

---

 # 13\. Logical Operators

 Logical operators allow multiple conditions to be combined:

 - **`$and`** → all conditions must be true.
- **`$or`** → at least one condition must be true.
- **`$not`** → negates a condition.

 They are useful for creating more complex queries.

---

 # 14\. `$exists` Operator

 The **`$exists`** operator checks whether a particular field is present in a document.

 - **`$exists: true`** → field exists, even if its value is `null`.
- **`$exists: false`** → field does not exist.

 Example:

```
db.collectionName.find({
  key: { $exists: true }
})
```

---

 # 15\. Array Operators

 Array operators allow MongoDB to filter documents based on **array fields**.

 - **`$in`** → matches if an array contains **any** of the specified values.
- **`$all`** → matches if an array contains **all** specified values.
- **`$size`** → matches arrays containing a specific **number of elements**.

 Example uses include finding movies belonging to `"Action"` or `"Drama"` and finding movies with exactly two genres.

---

 # 16\. MongoDB: Updating Documents

 ## `updateOne()`

 - Updates **a single document** matching a filter.
- It finds the **first matching document** and applies the update.

```
db.collection.updateOne(
  <filter>,
  <update>,
  <options>
)
```

 Common form:

```
db.collection.updateOne(
  <filter>,
  { $set: { <newField>: <value> } }
)
```

 - **`$set`** assigns a new value to a field.
- **`upsert: true`** → inserts a new document if no document matches the filter.

 ## `updateMany()`

 - Updates **all documents** matching a filter.
- Useful when the same change must be applied to multiple documents.

```
db.collection.updateMany(
  <filter>,
  <update>,
  <options>
)
```

 ### Key Difference

 | Method | Purpose |
| --- | --- |
| `updateOne()` | Updates the **first matching document** |
| `updateMany()` | Updates **all matching documents** |

---

 # 17\. MongoDB: Sorting Documents

 ### `sort()`

 - **`sort()`** arranges query results in a specified order.
- It can be used with `find()` or `aggregate()`.

```
db.collection.find().sort({
  <field>: <1 or -1>
})
```

 - **`1`** → ascending.
- **`-1`** → descending.
- Multiple fields can be used.

 Example:

```
db.movies.find().sort({
  genre: 1,
  released: -1
})
```

 This sorts:

 1. By `genre` ascending.
2. Then by `released` descending when documents have the same genre.

---

 # 18\. MongoDB: Deleting Documents

 ### `deleteOne()`

 - Deletes the **first document** matching a filter.

```
db.collection.deleteOne(<filter>)
```

 ### `deleteMany()`

 - Deletes **all documents** matching a filter.

```
db.collection.deleteMany(<filter>)
```

 ### Key Difference

 | Method | Purpose |
| --- | --- |
| `deleteOne()` | Deletes the **first matching document** |
| `deleteMany()` | Deletes **all matching documents** |

---

 # 19\. MongoDB CRUD — Quick Revision

 | CRUD | MongoDB Methods | Purpose |
| --- | --- | --- |
| **Create** | `insertOne()`, `insertMany()` | Add documents |
| **Read** | `find()`, `countDocuments()` | Retrieve/count documents |
| **Update** | `updateOne()`, `updateMany()` | Modify documents |
| **Delete** | `deleteOne()`, `deleteMany()` | Remove documents |

Additional query functionality:

 - **Conditional operators** → filter based on values.
- **Logical operators** → combine conditions.
- **`$exists`** → check whether a field exists.
- **Array operators** → query array contents.
- **`sort()`** → order query results.
- **`$set`** → modify field values.
- **`upsert`** → insert when no matching document exists.

---

 # 20\. NoSQL Data Modeling: Column-Based Databases

 A **column-based database** organizes data around **columns/column families** rather than storing complete rows in the traditional relational style.

 ### Why Column-Based Storage?

 Column-oriented storage is particularly useful when a query needs only certain columns.

 For example, if a query only needs `current_balance`, the database can access that column instead of reading every field in every row.

 This can:

 - Reduce the amount of data read.
- Improve query performance.
- Be especially useful for **analytics and large datasets**.

 ### Column-Based vs. Row-Oriented Access

 - **Column-oriented storage** → efficient when queries access a small number of columns across many records.
- **Row-oriented storage** → efficient when a query accesses most/all fields of a particular row.

---

 # 21\. Elements of a Column-Based Database

 A column-based/column-family database contains several important concepts.

 ### Column

 A **column** is a basic unit of storage.

 A column has:

 - A **name**
- A **value**
- An associated **timestamp**, indicating when it was written.

 ### Column Family

 A **column family** is a group of related columns stored together.

 - Related columns can be accessed efficiently as a group.
- It acts somewhat like a **logical table** for organizing data.

 ### Row

 A **row** is a collection of columns associated with a row key.

 Important characteristic:

 - Rows in a columnar database **do not necessarily have the same set of columns**.

 ### Row Key

 A **row key** is the **unique identifier for each row** within a column family.

 ### Simplified Structure

```
Column Family
   ├── Row Key 1
   │     ├── Column A → Value + Timestamp
   │     ├── Column B → Value + Timestamp
   │
   ├── Row Key 2
   │     ├── Column A → Value + Timestamp
   │     └── Column C → Value + Timestamp
```

 **Key idea:** Column-family databases provide flexible rows and efficient access to related columns.

---

 # 22\. NoSQL Data Modeling: Graph Databases

 A **graph database** uses graph structures to store and represent data.

 Instead of traditional tables or documents, it uses:

 - **Nodes**
- **Edges/relationships**
- **Properties**

 ### Nodes

 Nodes represent **entities** or objects.

 Examples:

```
Person
Product
Company
City
```

 ### Edges

 Edges represent **relationships between nodes**.

 For example:

```
Alice ──FRIENDS_WITH──> Bob
```

 The relationship itself can contain information/properties.

 ### Why Graph Databases?

 Graph databases are especially useful for:

 - **Many-to-many relationships**
- Highly connected data
- Dynamic relationships
- Fast relationship traversal

 Examples include:

 - Social networks
- Recommendation systems
- Fraud detection
- Network analysis

---

 # 23\. Two Common Graph Database Models

 There are two major graph models:

 1. **RDF (Resource Description Framework)**
2. **Property Graph**

 They both represent connected data but focus on different purposes.

 | RDF Graph | Property Graph |
| --- | --- |
| Focuses on data integration and semantics | Focuses on analytics and connected-data querying |
| Common in Web of Data applications | Designed for efficient graph traversal |
| Uses triples | Uses nodes, relationships, and properties |
| Strong focus on meaning/semantics | Strong focus on practical connected-data modeling |

---

 # 24\. RDF — Resource Description Framework

 **RDF** is a standard for exchanging and integrating data, particularly on the Web.

 It is especially suitable for:

 - **Semantic Web**
- **Web of Data**
- Data integration
- Applications where data needs explicit meaning and reasoning

 ### RDF Triple

 The basic unit of RDF is a **triple**:

```
(subject, predicate, object)
```

 For example:

```
(Alice, knows, Bob)
```

 Here:

 - **Alice** → subject
- **knows** → predicate
- **Bob** → object

 The triple represents a relationship between two resources.

 ### URI

 RDF elements can be identified using **Uniform Resource Identifiers (URIs)**.

 URIs provide unique, globally resolvable references for resources.

 **Key idea:**

 > RDF is mainly about representing and integrating data with explicit meaning.

---

 # 25\. Property Graphs

 A **property graph** is designed to represent connected data in a more descriptive and practical way.

 It consists of:

 - **Nodes**
- **Relationships**
- **Properties**

 ### Nodes

 - Can have **labels** identifying their role/type.
- Can store **properties as key-value pairs**.

 Example:

```
Person
name = "Alice"
age = 25
```

 ### Relationships

 Relationships:

 - Connect two nodes.
- Are typically **directed**.
- Have a **type/name**.
- Can contain their own **properties**.
- Have a start node and an end node.

 Example:

```
Alice ──WORKS_AT──> Company
```

 The relationship could contain:

```
since = 2024
role = "Engineer"
```

 ### Main Advantage

 Property graphs are designed for **efficient storage, querying, and traversal of connected data**.

 **Key idea:**

 > Property graphs focus on efficiently working with entities, relationships, and their properties.

---

 # 26\. RDF vs. Property Graph — Exam Comparison

 | Feature | RDF | Property Graph |
| --- | --- | --- |
| Basic structure | Triple | Nodes + relationships + properties |
| Triple format | Subject–Predicate–Object | Node–Relationship–Node |
| Main focus | Data integration/semantics | Analytics and graph traversal |
| Properties | Represented through additional triples | Directly stored on nodes/relationships |
| Identifiers | URIs | Node/relationship identifiers |
| Common use | Semantic Web, linked data | Connected-data applications |

### Easy Memory Trick

 **RDF = Meaning + Integration**

 **Property Graph = Relationships \+ Properties + Traversal**

---

 # 27\. Overall NoSQL Data Modeling

 The major NoSQL models covered are:

 | Model | Main Structure | Best For |
| --- | --- | --- |
| **Document** | Documents/collections | Flexible application data |
| **Key-Value** | Key → Value | Caching, sessions, fast lookups |
| **Column-Based** | Columns/column families/rows | Analytics and large-scale data |
| **Graph** | Nodes + relationships | Highly connected data |

The choice of model **depends on the application's requirements**.

---

 # 28\. ⭐ Final Overall Takeaway — Chunks 1–4 + 6

 The material progresses from **NoSQL fundamentals → database models → MongoDB → CRUD/querying → advanced NoSQL data modeling**.

 ### Core concepts

 - **NoSQL** provides **flexibility, scalability, availability, and performance**.
- **ACID** emphasizes strong consistency and correctness.
- **BASE** emphasizes availability and eventual consistency.
- **CAP theorem** explains trade-offs involving consistency, availability, and partition tolerance.
- There are four major NoSQL models:
  - **Document**
  - **Key-Value**
  - **Column-Based**
  - **Graph**

 ### MongoDB

 Remember the CRUD operations:

```
CREATE → insertOne(), insertMany()
READ   → find(), countDocuments()
UPDATE → updateOne(), updateMany()
DELETE → deleteOne(), deleteMany()
```

 Also remember:

```
sort()       → sort results
$set         → change field values
$exists      → check field existence
$in          → match any listed array/value
$all         → match all listed array values
$size        → match array length
$and/$or     → combine conditions
upsert       → insert if no match exists
```

 ### Advanced data modeling

 - **Column-based databases** → organize data around columns/column families and are useful when queries access particular columns across many records.
- Important column-family concepts:
  - **Column**
  - **Column Family**
  - **Row**
  - **Row Key**
  - **Timestamp**
- **Graph databases** → represent connected data using nodes, edges, and properties.
- Two important graph models:
  - **RDF** → triples, semantics, data integration, Web of Data.
  - **Property Graph** → nodes, relationships, properties, efficient traversal and analytics.

 ### ⭐ One-line exam summary

 **NoSQL offers flexible and scalable alternatives to relational databases; MongoDB demonstrates the document model and CRUD operations, while column-based and graph databases provide specialized models for large-scale column access and highly connected data.**
