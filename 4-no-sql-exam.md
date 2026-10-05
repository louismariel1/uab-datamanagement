# Non-MCQ Exam Questions and Answers — Chunks 1–6

 Below is a comprehensive set of **exam-style questions with model answers**, covering the NoSQL fundamentals, MongoDB, CRUD, querying, column-based databases, and graph databases from **Chunks 1–6**.

---

 ## Part A — NoSQL Fundamentals

 ### 1\. What is a NoSQL database? How does it differ from a relational database?

 **Answer:**

 A **NoSQL database** is a non-relational database designed to handle large-scale, diverse, semi-structured, or unstructured data using flexible data models.

 Unlike relational databases, which store data in **tables with rows and columns and usually use fixed schemas**, NoSQL databases can use document, key-value, column-family, or graph models.

 Key differences include:

 - Relational databases usually use **fixed schemas**; NoSQL databases generally use **flexible schemas**.
- Relational databases commonly scale vertically; NoSQL systems are designed for **horizontal scaling**.
- Relational databases emphasize strong consistency; many NoSQL systems can use **eventual consistency**.
- NoSQL is particularly useful for large-scale, distributed, and rapidly changing applications.

---

 ### 2. Explain the main characteristics of NoSQL databases.

 **Answer:**

 The major characteristics of NoSQL databases are:

 1. **Horizontal scalability** — capacity can be increased by adding more servers or nodes.
2. **Flexible schemas** — data structures can change without requiring a rigid schema.
3. **High performance** — optimized for fast reads and writes.
4. **High availability** — systems can continue operating despite failures.
5. **Fault tolerance** — distributed architectures can handle node failures.
6. **Support for diverse data** — suitable for structured, semi-structured, and unstructured data.
7. **Cost effectiveness** — many NoSQL systems are open-source and can run on commodity hardware.

---

 ### 3\. Compare SQL and NoSQL databases.

 **Answer:**

 | Feature | SQL | NoSQL |
| --- | --- | --- |
| Data structure | Tables | Documents, key-value, columns, graphs |
| Schema | Usually fixed | Usually flexible |
| Scaling | Often vertical | Often horizontal |
| Data | Structured | Semi-structured/unstructured |
| Consistency | Traditionally strong | Often eventual, depending on system |
| Relationships | Explicit relational relationships | Depends on data model |
| Typical use | Banking, ERP | IoT, social media, e-commerce |

The choice depends on the application's requirements.

---

 ### 4\. What is horizontal scalability?

 **Answer:**

 **Horizontal scalability** means increasing system capacity by adding more servers or nodes to a distributed system.

 For example, instead of replacing one server with a much more powerful server, a NoSQL system can add several additional servers and distribute the workload among them.

 This makes horizontal scaling particularly useful for large-scale distributed applications.

---

 ### 5\. Explain ACID properties.

 **Answer:**

 ACID describes four properties that help ensure reliable database transactions:

 - **Atomicity** — a transaction is treated as one unit; it either completes fully or does not happen.
- **Consistency** — a transaction moves the database from one valid state to another valid state.
- **Isolation** — concurrent transactions should not improperly interfere with each other.
- **Durability** — once a transaction is committed, its changes are preserved.

 ACID focuses strongly on **correctness, reliability, and consistency**.

---

 ### 6\. What is BASE? How does it differ from ACID?

 **Answer:**

 BASE stands for:

 - **Basically Available**
- **Soft State**
- **Eventually Consistent**

 BASE systems emphasize **availability, scalability, and flexibility** rather than requiring immediate strong consistency everywhere.

 The key difference is:

 > **ACID prioritizes strong consistency and transactional correctness, while BASE prioritizes availability and scalability with eventual consistency.**

---

 ### 7\. Explain the CAP theorem.

 **Answer:**

 The **CAP theorem** describes a trade-off among three properties in a distributed database system:

 - **Consistency (C)** — every read receives the most recent applicable data.
- **Availability (A)** — every request receives a response.
- **Partition Tolerance (P)** — the system continues operating despite communication failures between nodes.

 When a network partition occurs, a distributed system must make a trade-off between **consistency and availability**.

 NoSQL systems commonly prioritize **partition tolerance** and may choose availability with eventual consistency depending on application requirements.

---

 ### 8\. Why is partition tolerance important in distributed databases?

 **Answer:**

 In a distributed system, different nodes communicate over a network. Network failures can prevent nodes from communicating with each other.

 **Partition tolerance** means the database continues operating even when such communication failures occur.

 This is important because distributed systems are expected to operate across multiple machines and networks, where failures can occur.

---

 ## Part B — NoSQL Data Models

 ### 9\. What are the four major NoSQL data models?

 **Answer:**

 The four major NoSQL models are:

 1. **Document database**
2. **Key-value database**
3. **Column-based/column-family database**
4. **Graph database**

 Each model is optimized for different types of applications and data.

---

 ### 10\. Explain the document data model and give an example of its use.

 **Answer:**

 A document database stores information as **JSON-like documents** containing field-value pairs.

 Documents are usually grouped into **collections**.

 For example:

```
{
  "name": "Alice",
  "age": 25,
  "skills": ["Java", "MongoDB"]
}
```

 Documents in the same collection can have different fields because the schema is flexible.

 **Example:** MongoDB.

 Typical uses include:

 - Product catalogs
- Content management
- User profiles
- Applications with changing data structures

---

 ### 11\. Explain the key-value data model.

 **Answer:**

 A key-value database stores information as pairs:

```
Key → Value
```

 The key uniquely identifies the value.

 For example:

```
"user123" → "Alice"
```

 The value can be a string, number, JSON object, array, or other data.

 Key-value databases are especially suitable for:

 - Caching
- Sessions
- User preferences
- Fast lookups

 **Example:** Redis.

---

 ### 12\. What is a column-based database, and when is it useful?

 **Answer:**

 A column-based database organizes data around **columns or column families** rather than traditional row-oriented storage.

 It is particularly useful when queries need only a few columns from a very large dataset.

 For example, if a query only needs `current_balance`, the system may access that column without reading every other field.

 This can reduce data read and improve performance for:

 - Analytics
- Large-scale data processing
- Data warehousing
- Queries accessing particular columns across many records

---

 ### 13\. What is a graph database? Give three use cases.

 **Answer:**

 A graph database represents data using:

 - **Nodes** — entities
- **Edges/relationships** — connections between entities
- **Properties** — information associated with nodes or relationships

 Graph databases are particularly effective for highly connected data.

 Examples include:

 - Social networks
- Recommendation systems
- Fraud detection
- Network analysis

---

 ### 14\. Which NoSQL model would you choose for each scenario?

 **Question:**

 Choose the most appropriate NoSQL model for:

 a. User session storage\
 b. Product catalog with varying attributes\
 c. Social network relationships\
 d. Large-scale analytical queries

 **Answer:**

 a. **Key-value** — fast access using a session key.

 b. **Document** — product documents can have different attributes.

 c. **Graph** — relationships between users are central.

 d. **Column-based** — efficient for analytical queries over large datasets.

---

 # Part C — MongoDB and Document Databases

 ### 15\. What is MongoDB?

 **Answer:**

 MongoDB is a **NoSQL document database** designed to store, manage, and retrieve large amounts of data.

 Important characteristics include:

 - Document-oriented storage
- Flexible/dynamic schemas
- BSON storage format
- Querying and indexing
- Aggregation capabilities
- CRUD operations
- Support for complex data structures

---

 ### 16\. What is BSON in MongoDB?

 **Answer:**

 **BSON** stands for **Binary JSON**.

 MongoDB stores documents using BSON, which extends JSON-like data representation with additional data matching the specified criteria, or the total number when no filter is supplied types and a binary representation.

 Thus, although MongoDB documents appear similar to JSON, MongoDB internally uses BSON.

---

 ### 17\. Explain the relationship between a collection and a document in MongoDB.

 **Answer:**

 A **collection** is a group of MongoDB documents.

 The approximate relational equivalent is:

 | MongoDB | Relational Database |
| --- | --- |
| Database | Database |
| Collection | Table |
| Document | Row |
| Field | Column |

However, the comparison is approximate because MongoDB documents have a flexible schema and do not necessarily contain identical fields.

---

 ### 18\. Why can documents in the same MongoDB collection have different fields?

 **Answer:**

 MongoDB uses a **flexible schema**.

 Therefore, documents in the same collection do not have to contain exactly the same fields.

 For example:

```
{
  "name": "Alice",
  "age": 25
}
```

 and:

```
{
  "name": "Bob",
  "email": "bob@example.com",
  "country": "Netherlands"
}
```

 can belong to the same collection.

 This flexibility is useful when application data changes frequently.

---

 # Part D — MongoDB CRUD

 ### 19\. Explain CRUD operations in MongoDB.

 **Answer:**

 CRUD stands for:

 - **Create** — `insertOne()`, `insertMany()`
- **Read** — `find()`, `countDocuments()`
- **Update** — `updateOne()`, `updateMany()`
- **Delete** — `deleteOne()`, `deleteMany()`

 CRUD represents the fundamental operations performed on database data.

---

 ### 20\. Explain `insertOne()` and `insertMany()`.

 **Answer:**

 `insertOne()` inserts a single document:

```
db.movies.insertOne({
  title: "The Matrix",
  year: 1999
})
```

 `insertMany()` inserts multiple documents at once:

```
db.movies.insertMany([
  { title: "The Matrix", year: 1999 },
  { title: "Inception", year: 2010 }
])
```

 Therefore:

 - `insertOne()` → one document
- `insertMany()` → multiple documents

---

 ### 21\. How do you count documents in a MongoDB collection?

 **Answer:**

 Use:

```
db.movies.countDocuments()
```

 `countDocuments()` returns the number of documents matching the specified criteria, or the total number when no filter is supplied.

---

 ### 22\. How do you retrieve documents from MongoDB?

 **Answer:**

 Use the `find()` method.

 To retrieve all documents:

```
db.movies.find()
```

 To retrieve matching documents:

```
db.movies.find({
  year: 1999
})
```

 `find().pretty()` can be used to make output easier to read in environments that support it:

```
db.movies.find().pretty()
```

---

 ### 23\. What is the purpose of a conditional query in MongoDB?

 **Answer:**

 A conditional query filters documents according to specified criteria.

 For example:

```
db.movies.find({
  year: { $gt: 2000 }
})
```

 This finds movies whose `year` is greater than 2000.

 Conditional operators allow MongoDB to perform comparisons and more specific filtering.

---

 # Part E — MongoDB Query Operators

 ### 24\. Explain `$and`, `$or`, and `$not`.

 **Answer:**

 - **`$and`** requires all specified conditions to be true.
- **`$or`** requires at least one condition to be true.
- **`$not`** negates a condition.

 For example, `$or` can be used when a movie should match either of two conditions.

 These operators allow complex filtering logic to be constructed.

---

 ### 25\. Explain the `$exists` operator.

 **Answer:**

 `$exists` checks whether a field is present in a document.

```
db.users.find({
  email: { $exists: true }
})
```

 This finds documents where the `email` field exists.

```
db.users.find({
  email: { $exists: false }
})
```

 This finds documents where the `email` field does not exist.

 An important point is that:

 > `$exists: true` means the field is present, even if its value is `null`.

---

 ### 26\. Explain the MongoDB array operators `$in`, `$all`, and `$size`.

 **Answer:**

 - **`$in`** matches if a value matches any value from a specified list.
- **`$all`** requires an array to contain all specified values.
- **`$size`** matches arrays containing a specified number of elements.

 For example, if:

```
{
  "genres": ["Action", "Drama"]
}
```

 then:

 - `$in` can find documents containing `"Action"` or `"Drama"`.
- `$all` can require both `"Action"` and `"Drama"`.
- `$size: 2` can find arrays containing exactly two elements.

---

 # Part F — MongoDB Updates

 ### 27\. Explain `updateOne()`.

 **Answer:**

 `updateOne()` updates the **first document that matches a filter**.

 Example:

```
db.movies.updateOne(
  { title: "The Matrix" },
  { $set: { year: 2000 } }
)
```

 It searches for a matching document and updates one matching document.

---

 ### 28\. Explain `updateMany()`.

 **Answer:**

 `updateMany()` updates **all documents that match a specified filter**.

 Example:

```
db.movies.updateMany(
  { genre: "Comedy" },
  { $set: { category: "Comedy Film" } }
)
```

 Every matching document receives the update.

---

 ### 29\. What is the difference between `updateOne()` and `updateMany()`?

 **Answer:**

 | Method | Behavior |
| --- | --- |
| `updateOne()` | Updates the first matching document |
| `updateMany()` | Updates all matching documents |

Use `updateOne()` when only one matching document should be modified.

 Use `updateMany()` when the same change must be applied to every matching document.

---

 ### 30\. What is `$set` in MongoDB?

 **Answer:**

 `$set` is an update operator used to assign a value to a field.

 Example:

```
db.users.updateOne(
  { name: "Alice" },
  { $set: { age: 26 } }
)
```

 This changes or creates the `age` field with the value `26`.

---

 ### 31\. What is `upsert`?

 **Answer:**

 `upsert` combines update and insert behavior.

 When:

```
upsert: true
```

 is specified, MongoDB updates a matching document if one exists.

 If no document matches the filter, MongoDB inserts a new document.

 Conceptually:

 > **Match found → update.\
>  No match → insert.**

---

 # Part G — MongoDB Sorting and Deletion

 ### 32\. Explain the `sort()` method.

 **Answer:**

 `sort()` arranges query results according to one or more fields.

```
db.movies.find().sort({
  year: 1
})
```

 Here:

 - `1` = ascending
- `-1` = descending

 For example:

```
db.movies.find().sort({
  genre: 1,
  released: -1
})
```

 means:

 1. Sort by `genre` ascending.
2. For documents with the same genre, sort `released` descending.

---

 ### 33\. What is the difference between `deleteOne()` and `deleteMany()`?

 **Answer:**

 `deleteOne()` removes the **first document matching a filter**.

```
db.movies.deleteOne({
  genre: "Comedy"
})
```

 `deleteMany()` removes **all documents matching a filter**.

```
db.movies.deleteMany({
  genre: "Comedy"
})
```

 Therefore:

 > `deleteOne()` → first matching document\
>  `deleteMany()` → all matching documents

---

 # Part H — Column-Based Databases

 ### 34\. Explain the difference between row-oriented and column-oriented storage.

 **Answer:**

 In **row-oriented storage**, data belonging to a complete row is stored together.

 This is useful when a query needs most or all fields of individual records.

 In **column-oriented storage**, values belonging to the same column are stored together.

 This is useful when a query needs only a few columns across a large number of records.

 For example, a query requiring only:

```
current_balance
```

 may avoid reading unrelated columns.

 Thus, column-oriented storage can reduce the amount of data read for analytical workloads.

---

 ### 35\. What is a column in a column-family database?

 **Answer:**

 A **column** is a basic unit of storage.

 It contains:

 - A **name**
- A **value**
- A **timestamp**

 For example:

```
balance → 5000 → timestamp
```

 The timestamp indicates when the value was written.

---

 ### 36\. What is a column family?

 **Answer:**

 A **column family** is a group of related columns stored together.

 It provides a logical way of organizing related data and can be viewed somewhat like a logical table.

 Column families are particularly associated with column-family NoSQL databases.

---

 ### 37\. What is a row key?

 **Answer:**

 A **row key** is the unique identifier for a row within a column family.

 It allows the database to identify and access a particular row.

 For example:

```
Row Key: customer123
```

 could identify all columns associated with customer `customer123`.

---

 ### 38\. Do all rows in a column-family database need to contain the same columns?

 **Answer:**

 No.

 One important characteristic of column-family databases is that rows do not necessarily need to have exactly the same set of columns.

 For example:

```
Row 1:
name
email
balance

Row 2:
name
balance
address
```

 This provides flexibility in representing data.

---

 ### 39\. Explain why column-based databases can be efficient for analytics.

 **Answer:**

 Analytical queries often process very large numbers of records but require only a small number of columns.

 Column-oriented storage allows the system to read only the required columns rather than the entire records.

 This can:

 - Reduce I/O
- Reduce the amount of data processed
- Improve analytical query performance
- Work efficiently with large datasets

---

 # Part I — Graph Databases

 ### 40\. What is a graph database?

 **Answer:**

 A graph database stores data using graph structures consisting primarily of:

 - **Nodes**
- **Edges/relationships**
- **Properties**

 Nodes represent entities, while relationships represent connections between entities.

 For example:

```
Alice ──FRIENDS_WITH──> Bob
```

 Graph databases are particularly effective when relationships between entities are central to the application.

---

 ### 41\. What are nodes and edges in a graph database?

 **Answer:**

 A **node** represents an entity or object.

 Examples:

 - Person
- Company
- Product
- City

 An **edge** represents a relationship between nodes.

 For example:

```
Alice ──WORKS_AT──> Company
```

 Here:

 - Alice = node
- Company = node
- `WORKS_AT` = relationship/edge

---

 ### 42\. What are properties in a property graph?

 **Answer:**

 Properties are key-value attributes associated with nodes or relationships.

 For example, a Person node might contain:

```
name = "Alice"
age = 25
```

 A relationship might contain:

```
since = 2024
role = "Engineer"
```

 Therefore, properties provide additional descriptive information about entities and relationships.

---

 ### 43\. Why are graph databases suitable for social networks?

 **Answer:**

 Social networks contain large numbers of relationships such as:

 - Friends
- Followers
- Likes
- Memberships
- Connections

 A graph database represents these relationships directly as edges between nodes.

 This makes graph databases effective for traversing connected data and answering relationship-oriented questions such as:

 > "Who are Alice's friends?"

 or:

 > "Which users are connected to both Alice and Bob?"

---

 # Part J — RDF and Property Graphs

 ### 44\. What is RDF?

 **Answer:**

 **RDF**, or **Resource Description Framework**, is a standard for representing and integrating data, particularly on the Web.

 It is commonly used for:

 - Semantic Web applications
- Web of Data
- Data integration
- Representing information with explicit meaning

 The basic RDF structure is a **triple**:

```
Subject – Predicate – Object
```

---

 ### 45\. Explain an RDF triple with an example.

 **Answer:**

 An RDF triple consists of:

 1. **Subject**
2. **Predicate**
3. **Object**

 Example:

```
(Alice, knows, Bob)
```

 Here:

 - Alice = subject
- knows = predicate
- Bob = object

 The triple states that Alice has a `knows` relationship with Bob.

---

 ### 46\. What is the purpose of a URI in RDF?

 **Answer:**

 A **URI (Uniform Resource Identifier)** provides an identifier for a resource.

 URIs can provide unique, globally referenceable identifiers for resources in RDF.

 This is important for **data integration and the Web of Data**, because different datasets can refer to resources using consistent identifiers.

---

 ### 47\. What is a property graph?

 **Answer:**

 A property graph represents connected data using:

 - Nodes
- Relationships
- Properties

 Nodes can have labels and properties.

 Relationships connect nodes and can have their own types and properties.

 For example:

```
Alice ──WORKS_AT──> Company
```

 The relationship might have:

```
since = 2024
role = "Engineer"
```

 Property graphs are designed for efficient **graph traversal, querying, analytics, and connected-data applications**.

---

 ### 48\. Compare RDF and property graphs.

 **Answer:**

 | Feature | RDF | Property Graph |
| --- | --- | --- |
| Basic structure | Subject-Predicate-Object triple | Nodes + relationships + properties |
| Main focus | Semantics and integration | Traversal and connected-data querying |
| Properties | Represented using triples | Directly stored on nodes/relationships |
| Identifiers | URIs | Node/relationship identifiers |
| Common applications | Semantic Web, linked data | Analytics, recommendations, connected applications |

A useful memory trick is:

 > **RDF = Meaning + Integration**

 > **Property Graph = Relationships \+ Properties + Traversal**

---

 # Part K — Application and Scenario Questions

 ### 49\. A company needs to store user sessions and retrieve them extremely quickly using a session ID. Which NoSQL model would you recommend and why?

 **Answer:**

 A **key-value database** would be appropriate.

 The session ID can act as the key:

```
session123 → session data
```

 Key-value databases are optimized for fast key-based lookups and are commonly used for **sessions and caching**.

---

 ### 50\. An e-commerce company sells products where different products have different attributes. Which NoSQL model would be suitable?

 **Answer:**

 A **document database** would be suitable.

 Products can be stored as documents with different fields.

 For example:

```
{
  "name": "Laptop",
  "ram": "16GB",
  "storage": "1TB"
}
```

 Another product could have:

```
{
  "name": "Shoes",
  "size": 42,
  "color": "Black"
}
```

 The flexible schema makes the document model suitable for varying product attributes.

---

 ### 51\. A company needs to analyze billions of records but frequently queries only a few columns. Which database model would be appropriate?

 **Answer:**

 A **column-based database** would be appropriate.

 Column-oriented storage can access only the required columns instead of reading entire rows.

 This can reduce data access and improve performance for large-scale analytical workloads.

---

 ### 52\. A social media platform needs to identify relationships between users and recommend friends. Which NoSQL model would be most appropriate?

 **Answer:**

 A **graph database** would be appropriate.

 Users can be represented as nodes and relationships such as `FRIENDS_WITH` can be represented as edges.

 Graph traversal makes it efficient to analyze connections between users.

---

 ### 53\. A website needs to represent globally identifiable resources and integrate information from multiple datasets. Would RDF or a property graph be more appropriate?

 **Answer:**

 **RDF** would generally be more appropriate.

 RDF is designed for:

 - Data integration
- Explicit semantics
- Web of Data
- Semantic Web applications
- Globally identifiable resources using URIs

---

 ### 54\. A company needs to efficiently traverse relationships between customers, products, and purchases. Would RDF or a property graph be more suitable?

 **Answer:**

 A **property graph** would generally be suitable because it is designed for practical connected-data querying and efficient graph traversal.

 Nodes can represent customers and products, while relationships can represent purchases and contain properties such as dates or amounts.

---

 # Part L — Integrated Exam Questions

 ### 55\. Explain how the four major NoSQL models differ and when each should be used.

 **Answer:**

 The four major NoSQL models are designed for different data access patterns.

 **Document model:**

 - Stores JSON-like documents.
- Flexible schema.
- Suitable for product catalogs, content, and user profiles.
- Example: MongoDB.

 **Key-value model:**

 - Stores key-value pairs.
- Very fast key-based retrieval.
- Suitable for caching, sessions, and preferences.
- Example: Redis.

 **Column-based model:**

 - Organizes data around columns/column families.
- Efficient when queries access particular columns across many records.
- Suitable for analytics and large-scale processing.
- Example: Cassandra.

 **Graph model:**

 - Stores nodes, relationships, and properties.
- Suitable for highly connected data.
- Used in social networks, recommendations, and fraud detection.
- Example: Neo4j.

 The correct model depends primarily on the **data structure and access/query requirements**.

---

 ### 56\. Explain how MongoDB demonstrates the document NoSQL model.

 **Answer:**

 MongoDB stores data as **BSON documents** rather than traditional relational rows.

 Documents are grouped into collections.

 For example:

```
{
  title: "The Matrix",
  year: 1999,
  genres: ["Action", "Sci-Fi"]
}
```

 MongoDB's flexible schema means different documents can contain different fields.

 It also provides:

 - CRUD operations
- Query operators
- Array operators
- Sorting
- Updating
- Deletion
- Aggregation
- Indexing

 Thus, MongoDB demonstrates how the document model provides flexible data storage and powerful querying.

---

 ### 57\. Describe the complete MongoDB CRUD process with examples.

 **Answer:**

 **Create:**

```
db.movies.insertOne({
  title: "The Matrix",
  year: 1999
})
```

 **Read:**

```
db.movies.find({
  year: 1999
})
```

 **Update:**

```
db.movies.updateOne(
  { title: "The Matrix" },
  { $set: { year: 2000 } }
)
```

 **Delete:**

```
db.movies.deleteOne({
  title: "The Matrix"
})
```

 Therefore:

```
CREATE → insertOne(), insertMany()
READ   → find(), countDocuments()
UPDATE → updateOne(), updateMany()
DELETE → deleteOne(), deleteMany()
```

---

 ### 58\. Explain how MongoDB queries can become more complex using operators.

 **Answer:**

 MongoDB provides multiple types of operators.

 **Logical operators:**

```
$and
$or
$not
```

 These combine or negate conditions.

 **Field-existence operator:**

```
$exists
```

 Checks whether a field is present.

 **Array operators:**

```
$in
$all
$size
```

 These allow queries against array contents.

 **Comparison/conditional operators** can also filter values according to conditions.

 Together, these operators allow MongoDB to perform complex and flexible document queries.

---

 ### 59\. Explain the difference between MongoDB querying and aggregation.

 **Answer:**

 A normal `find()` query is primarily used to **retrieve documents matching specified criteria**.

 For example:

```
db.movies.find({
  year: { $gt: 2000 }
})
```

 Aggregation is designed for **processing, transforming, grouping, and analyzing data** through a pipeline.

 For example:

```
db.movies.aggregate([
  { $match: { genre: "Sci-Fi" } },
  { $sort: { rating: -1 } },
  { $limit: 3 }
])
```

 The aggregation pipeline can perform multiple operations sequentially.

 Thus:

 > **`find()` → retrieve/filter documents**

 > **`aggregate()` → process, transform, summarize, and analyze documents**

---

 ### 60\. Explain why choosing the correct NoSQL data model is important.

 **Answer:**

 Different NoSQL models optimize different access patterns.

 For example:

 - A **key-value** database is excellent for fast lookups.
- A **document** database is useful for flexible application data.
- A **column-based** database is useful for large-scale analytical workloads.
- A **graph** database is useful for highly connected data.

 Choosing the wrong model can result in inefficient queries, unnecessary complexity, and poor performance.

 Therefore, database selection should be based on:

 - Data structure
- Query patterns
- Scalability requirements
- Performance requirements
- Relationship complexity
- Consistency and availability requirements

---

 # Part M — High-Value Long-Answer Questions

 ### 61\. Discuss the advantages and disadvantages of NoSQL databases compared with relational databases.

 **Answer:**

 NoSQL databases provide several advantages.

 ### Advantages

 - **Flexible schemas** allow changing data structures.
- **Horizontal scalability** makes it easier to handle increasing workloads.
- They can provide **high availability** and fault tolerance.
- They support different data models for specialized applications.
- They can provide high performance for specific access patterns.
- They are well suited to large-scale and distributed applications.

 ### Disadvantages or trade-offs

 - Some NoSQL systems do not provide the same level of strong consistency as traditional relational systems.
- Relationships may be less naturally represented in some NoSQL models.
- Query languages and features differ significantly between NoSQL systems.
- Choosing the correct data model requires understanding application access patterns.
- Some applications require the transactional guarantees traditionally associated with relational databases.

 Therefore, NoSQL is not universally better than SQL. The appropriate choice depends on application requirements.

---

 ### 62\. Explain the progression from NoSQL fundamentals to advanced data modeling.

 **Answer:**

 The material progresses through several stages.

 First, NoSQL is introduced as a flexible and scalable alternative to traditional relational databases.

 Next, important distributed-system concepts are introduced:

 - ACID
- BASE
- CAP theorem

 Then the four major NoSQL models are introduced:

 - Document
- Key-value
- Column-based
- Graph

 MongoDB is then studied as an example of the **document model**.

 MongoDB topics include:

 - Documents and collections
- BSON
- CRUD
- Queries
- Logical operators
- `$exists`
- Array operators
- Updates
- Sorting
- Deletion

 The material then moves into advanced data modeling:

 - Column-based databases
- Columns
- Column families
- Rows
- Row keys
- Timestamps

 Finally, graph databases are introduced, including:

 - Nodes
- Relationships
- Properties
- RDF
- RDF triples
- URIs
- Property graphs

 The overall progression is:

```
NoSQL Fundamentals
        ↓
SQL vs NoSQL
        ↓
ACID / BASE / CAP
        ↓
NoSQL Data Models
        ↓
MongoDB
        ↓
CRUD + Queries
        ↓
Column-Based Databases
        ↓
Graph Databases
        ↓
RDF vs Property Graphs
```

---

 ### 63\. Compare document, key-value, column-based, and graph databases in one answer.

 **Answer:**

 | Model | Structure | Main Strength | Typical Uses |
| --- | --- | --- | --- |
| **Document** | Documents/collections | Flexible schema | Product catalogs, content |
| **Key-value** | Key → value | Very fast lookups | Sessions, caching |
| **Column-based** | Columns/column families | Large-scale column access | Analytics |
| **Graph** | Nodes + relationships | Relationship traversal | Social networks |

The most important principle is:

 > **Choose the NoSQL model based on how the application stores and accesses its data.**

---

 ### 64\. Explain RDF and property graphs and discuss their major differences.

 **Answer:**

 Both RDF and property graphs represent connected data, but they have different design goals.

 **RDF** uses triples:

```
Subject → Predicate → Object
```

 For example:

```
Alice → knows → Bob
```

 RDF emphasizes:

 - Semantics
- Data integration
- Interoperability
- Web of Data
- Semantic Web
- URIs

 A **property graph** uses:

```
Nodes + Relationships + Properties
```

 For example:

```
Alice ──WORKS_AT──> Company
```

 The nodes and relationship can contain properties such as:

```
Alice:
age = 25

WORKS_AT:
since = 2024
role = "Engineer"
```

 Property graphs emphasize:

 - Graph traversal
- Connected-data querying
- Analytics
- Practical relationship modeling

 In short:

 > **RDF focuses on meaning, semantics, and integration.**

 > **Property graphs focus on relationships, properties, traversal, and connected-data analysis.**

---

 # Final Exam Revision Sheet

 ## Most Important Definitions

 - **NoSQL** → flexible, scalable non-relational database approach.
- **ACID** → Atomicity, Consistency, Isolation, Durability.
- **BASE** → Basically Available, Soft State, Eventually Consistent.
- **CAP** → Consistency, Availability, Partition Tolerance.
- **Document database** → stores JSON-like documents.
- **Key-value database** → stores key-value pairs.
- **Column-family database** → organizes data around columns/column families.
- **Graph database** → stores nodes and relationships.
- **RDF** → semantic data model based on triples.
- **Property graph** → nodes + relationships \+ properties.

 ## MongoDB Commands to Memorize

```
insertOne()       → insert one document
insertMany()      → insert many documents

find()            → retrieve documents
countDocuments()  → count documents

updateOne()       → update first matching document
updateMany()      → update all matching documents
$set              → assign/change a field
upsert            → update or insert

deleteOne()       → delete first matching document
deleteMany()      → delete all matching documents

sort()             → sort results
```

 ## MongoDB Operators

```
$and       → all conditions
$or        → at least one condition
$not       → negate condition

$exists    → field exists/doesn't exist

$in        → any specified value
$all       → all specified values
$size      → exact array length
```

 ## Column Database Concepts

```
Column
Column Family
Row
Row Key
Timestamp
```

 Remember:

 > **Column-oriented → good when reading a few columns across many records.**

 ## Graph Database Concepts

```
Node          → entity
Relationship  → connection
Property      → attribute
```

 ## RDF

```
Subject → Predicate → Object
```

 Example:

```
Alice → knows → Bob
```

 ## Property Graph

```
Node + Relationship + Properties
```

 ### ⭐ Ultimate Exam Memory Line

 **NoSQL provides flexible and scalable data storage through four major models: document, key-value, column-based, and graph; MongoDB demonstrates the document model with CRUD and powerful querying, while column-family databases optimize large-scale column access and graph databases optimize connected data through nodes, relationships, and properties, with RDF emphasizing semantic meaning and data integration.**
