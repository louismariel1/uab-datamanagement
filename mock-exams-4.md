I have the complete T3 material. I’ll use it as the examinable scope for the new set, covering NoSQL fundamentals, ACID/BASE/CAP, all four NoSQL data models, MongoDB CRUD and query operators, aggregation pipelines, key-value and wide-column databases, and graph databases.

The three exams will each be 60 minutes and 60 points, with a different emphasis:

- Mock Exam 1 — Core Concepts: definitions, conceptual distinctions, database selection, and foundational MongoDB queries.
- Mock Exam 2 — Applied Problems: CRUD operations, query operators, aggregation pipelines, and practical data-model selection.
- Mock Exam 3 — Comprehensive Challenge: integrated scenarios, CAP trade-offs, query debugging, aggregation, and graph/data-model design.

Each exam will include a complete answer key, worked solutions, explanations, and marking guidance. The output will be formatted as GitHub-ready Markdown, with fenced code blocks for MongoDB commands and clearly separated questions and solutions.

Below is the complete Markdown document. You can save it as `T3_NoSQL_Practice_Exams.md` in your GitHub repository.

Writing

# Data Management 2026–2027

# T3 — NoSQL Databases & Data Modeling

## Three 60-Minute Practice Exams with Comprehensive Solutions

**Exam scope:** T3 — NoSQL Databases & Data Modeling\
**Number of exams:** 3\
**Duration:** 60 minutes per exam\
**Maximum score:** 60 points per exam\
**Total available:** 180 points

---

# Mock Exam 1 — Core Concepts

**Duration:** 60 minutes\
**Total marks:** 60 points\
**Difficulty:** Foundational to intermediate

## Instructions

- Answer all questions.
- Select one answer for each multiple-choice question.
- Write valid MongoDB shell syntax where required.
- Explain conceptual answers briefly but precisely.
- Assume MongoDB documents use BSON internally.
- The answer key and marking guide appear after the exam.

## Section A — Multiple Choice (12 points)

**12 questions × 1 point**

### Q1. SQL versus NoSQL

Which statement about NoSQL databases is correct?

A. They never use schemas.\
B. They cannot support ACID transactions.\
C. They support multiple data models beyond traditional relational tables.\
D. They always outperform relational databases.

### Q2. Scaling

A company upgrades a database server from 16 GB RAM to 128 GB RAM without adding another server. What type of scaling is this?

A. Horizontal scaling\
B. Vertical scaling\
C. Replication\
D. Sharding

### Q3. ACID

Which ACID property ensures that a transaction either completes entirely or has no effect?

A. Consistency\
B. Isolation\
C. Durability\
D. Atomicity

### Q4. BASE

What does eventual consistency mean?

A. All replicas must return identical data immediately after every write.\
B. Replicas may temporarily disagree but are expected to converge when updates stop and replication succeeds.\
C. Transactions are never supported.\
D. Every read must return an error during normal operation.

### Q5. CAP theorem

During a network partition, a distributed database must choose between which two guarantees if it continues tolerating that partition?

A. Atomicity and durability\
B. Horizontal and vertical scaling\
C. Strong consistency and availability\
D. Schema flexibility and transactions

### Q6. Data models

Which database model is most appropriate when queries primarily retrieve values using unique keys?

A. Graph\
B. Key-value\
C. Document\
D. Relational

### Q7. MongoDB structure

In MongoDB, which statement is correct?

A. A collection contains databases.\
B. A document contains collections.\
C. A collection groups documents.\
D. A field contains an entire database.

### Q8. MongoDB comparison operators

Which filter finds movies released strictly after 2015?

A. `{ year: { $gte: 2015 } }`\
B. `{ year: { $gt: 2015 } }`\
C. `{ year: { $lt: 2015 } }`\
D. `{ year: { $lte: 2015 } }`

### Q9. MongoDB updates

Which operation updates all documents matching a filter?

A. `updateOne()`\
B. `insertMany()`\
C. `updateMany()`\
D. `deleteMany()`

### Q10. Aggregation

Which MongoDB aggregation stage groups documents and calculates aggregate values?

A. `$match`\
B. `$project`\
C. `$sort`\
D. `$group`

### Q11. Wide-column databases

Which database is a typical example of a wide-column NoSQL database?

A. MongoDB\
B. Redis\
C. Apache Cassandra\
D. Neo4j

### Q12. Graph databases

Which representation is fundamental to RDF?

A. Tables, rows and foreign keys\
B. Subject–Predicate–Object triples\
C. Key-value pairs only\
D. Documents containing nested arrays only

## Section B — Short-Answer Questions (12 points)

### Q13. SQL and NoSQL comparison (3 points)

Explain three differences between SQL and NoSQL databases. Include data organization, schema flexibility, and scaling.

### Q14. ACID, BASE and CAP (3 points)

Briefly explain the purpose of each concept:

1. ACID
2. BASE
3. CAP theorem

Explain why they are not interchangeable.

### Q15. Choosing a database model (3 points)

Choose the most appropriate NoSQL model for each scenario and justify each choice.

1. A web application stores user sessions and retrieves them using session IDs.
2. A product catalog stores products with nested specifications and varying attributes.
3. A social network frequently explores connections between users.

Choose from document, key-value, wide-column and graph databases.

### Q16. RDF and property graphs (3 points)

Explain two differences between RDF graphs and property graphs. Give one example of a property graph relationship that contains a property.

## Section C — MongoDB Fundamentals (18 points)

Use the following sample documents in a collection called `movies`:

```
[
  {
    title: "The Matrix",
    year: 1999,
    genre: "Sci-Fi",
    rating: 8.7,
    genres: ["Action", "Sci-Fi"]
  },
  {
    title: "Inception",
    year: 2010,
    genre: "Sci-Fi",
    rating: 8.8,
    genres: ["Action", "Sci-Fi", "Thriller"]
  },
  {
    title: "Interstellar",
    year: 2014,
    genre: "Sci-Fi",
    rating: 8.7,
    genres: ["Adventure", "Sci-Fi"]
  },
  {
    title: "Toy Story",
    year: 1995,
    genre: "Animation",
    rating: 8.3,
    genres: ["Animation", "Comedy"]
  },
  {
    title: "Arrival",
    year: 2016,
    genre: "Sci-Fi",
    genres: ["Drama", "Sci-Fi"]
  }
]
```

### Q17. Read and filter (4 points)

Write MongoDB queries to:

1. Retrieve all movies released after 2000.
2. Retrieve movies released from 2000 through 2015, inclusive.
3. Retrieve movies whose genre is either `Sci-Fi` or `Animation`.
4. Retrieve movies whose `rating` field exists.

### Q18. Arrays and logical operators (4 points)

Write queries to:

1. Find movies whose `genres` array contains at least one of `Action` or `Drama`.
2. Find movies whose `genres` array contains both `Action` and `Sci-Fi`.
3. Find movies whose `genres` array contains exactly two elements.
4. Find movies released before 2000 or after 2015.

### Q19. Update and delete (4 points)

Write MongoDB commands to:

1. Change the rating of `The Matrix` to `9.0`, creating the field if necessary.
2. Change the genre to `Action` for every document whose current `genre` is `Adventure`.
3. Delete one movie titled `Toy Story`.
4. Explain the difference between `deleteOne()` and `deleteMany()`.

### Q20. Upsert and field existence (3 points)

Consider:

```
db.students.updateOne(
  { student_id: 101 },
  { $set: { age: 22 } },
  { upsert: true }
)
```

Answer the following:

1. What happens if a document with `student_id: 101` exists?
2. What happens if no matching document exists?
3. Explain the difference between checking whether `rating` exists and checking whether `rating` is non-null.

### Q21. Sorting (3 points)

Write a query that:

- Selects Sci-Fi movies.
- Sorts them by rating from highest to lowest.
- Uses year ascending as the secondary sort order.

Explain what `-1` and `1` mean in the sort specification.

## Section D — Applied Data Modeling (18 points)

### Q22. Select a data model (4 points)

Recommend a suitable database model for each workload and justify your choice.

1. A caching service stores temporary values indexed by user session keys.
2. An IoT platform stores large volumes of distributed sensor readings.
3. An application stores customer profiles with nested addresses and preferences.
4. A fraud detection platform analyzes networks of accounts and transactions.

### Q23. Vertical and horizontal scaling (3 points)

A business initially operates a database on one server. Its data volume and request rate increase substantially.

1. Describe a vertical scaling solution.
2. Describe a horizontal scaling solution.
3. Give one advantage and one limitation of horizontal scaling.

### Q24. CAP scenario (4 points)

A distributed shopping application has two replicas that become unable to communicate because of a network partition.

The business must decide whether to accept purchases on both sides of the partition or reject some operations to avoid conflicting inventory updates.

1. Explain the consistency–availability trade-off.
2. Describe how a CP-oriented design might behave.
3. Describe how an AP-oriented design might behave.
4. Explain why this is not a universal statement that every database permanently chooses only two CAP properties.

### Q25. Aggregation fundamentals (4 points)

Consider:

```
db.movies.aggregate([
  { $match: { genre: "Sci-Fi" } },
  {
    $group: {
      _id: "$genre",
      count: { $sum: 1 },
      averageRating: { $avg: "$rating" }
    }
  },
  { $project: { _id: 0, count: 1, averageRating: 1 } }
])
```

Explain:

1. What `$match` does.
2. What `_id: "$genre"` means inside `$group`.
3. What `$sum: 1` calculates.
4. What happens to documents with a missing `rating` when `$avg` is calculated.

### Q26. Row-oriented versus column-oriented storage (3 points)

An analytics system calculates the average balance across 20 million customers but needs only the `balance` field.

Explain why column-oriented analytical storage may be advantageous. Contrast this with a workload that retrieves a complete customer record by ID.

---

# Mock Exam 1 — Complete Solutions and Marking Guide

## Section A — Answers (12 points)

| Question | Answer | Explanation |
| --- | --- | --- |
| Q1 | C | NoSQL includes document, key-value, wide-column and graph models. |
| Q2 | B | Increasing the resources of one server is vertical scaling. |
| Q3 | D | Atomicity means all operations commit together or none do. |
| Q4 | B | Replicas may temporarily disagree but can later converge. |
| Q5 | C | During a partition, strong consistency and availability cannot both be guaranteed for all affected operations. |
| Q6 | B | Key-value databases optimize direct key-based access. |
| Q7 | C | A collection contains documents. |
| Q8 | B | `$gt` excludes the boundary. |
| Q9 | C | `updateMany()` updates every matching document. |
| Q10 | D | `$group` groups documents and computes aggregates. |
| Q11 | C | Cassandra is a wide-column database. |
| Q12 | B | RDF represents information using subject–predicate–object triples. |

**Marking:** 1 point for each correct answer.

## Section B — Model Answers (12 points)

### Q13. SQL and NoSQL comparison (3 points)

- **Data organization:** SQL databases traditionally organize data into tables; NoSQL databases support documents, key-value pairs, wide-column structures and graphs.
- **Schema:** Relational databases commonly use predefined schemas, while document databases and other NoSQL systems may permit more flexible structures.
- **Scaling:** Traditional relational deployments often scale vertically, while many NoSQL systems are designed for horizontal distribution.

These are tendencies, not absolute rules. SQL systems can scale horizontally, and NoSQL systems can enforce schemas and provide transactions.

**Marking:** 1 point per valid distinction.

### Q14. ACID, BASE and CAP (3 points)

- **ACID:** Transaction guarantees covering atomicity, consistency, isolation and durability.
- **BASE:** An approach associated with distributed systems that prioritize availability and tolerate temporary inconsistency.
- **CAP:** Describes the trade-off between strong consistency and availability when a network partition must be tolerated.

They address different questions: transaction correctness, distributed-system behavior, and partition-related guarantees, respectively.

**Marking:** 1 point for each concept accurately explained.

### Q15. Choosing a database model (3 points)

1. **Key-value:** Session IDs directly identify session data.
2. **Document:** Product specifications and attributes can be nested in flexible documents.
3. **Graph:** Relationships between users are central to traversal and connection analysis.

**Marking:** 1 point per correct choice with a reasonable justification.

### Q16. RDF and property graphs (3 points)

RDF represents facts as subject–predicate–object triples and is useful for semantic interoperability and linked data. Property graphs use nodes, directed relationships and properties attached to nodes or relationships.

Example:

```
(Alice:Person)-[WORKS_AT {since: 2024}]->(Acme:Company)
```

Here, `WORKS_AT` is the directed relationship and `since: 2024` is a relationship property.

**Marking:**

- 1 point: RDF triple structure.
- 1 point: property graph structure.
- 1 point: valid example and interpretation.

## Section C — Model Answers (18 points)

### Q17. Read and filter (4 points)

**1\. Movies released after 2000**

```
db.movies.find({
  year: { $gt: 2000 }
})
```

**2\. Movies released from 2000 through 2015**

```
db.movies.find({
  year: { $gte: 2000, $lte: 2015 }
})
```

**3\. Sci-Fi or Animation**

```
db.movies.find({
  genre: { $in: ["Sci-Fi", "Animation"] }
})
```

**4\. Movies whose rating field exists**

```
db.movies.find({
  rating: { $exists: true }
})
```

The fourth query also matches a document whose `rating` field exists but contains `null`.

**Marking:** 1 point per correct query.

### Q18. Arrays and logical operators (4 points)

**1\. At least one of Action or Drama**

```
db.movies.find({
  genres: { $in: ["Action", "Drama"] }
})
```

**2\. Both Action and Sci-Fi**

```
db.movies.find({
  genres: { $all: ["Action", "Sci-Fi"] }
})
```

**3\. Exactly two array elements**

```
db.movies.find({
  genres: { $size: 2 }
})
```

**4\. Before 2000 or after 2015**

```
db.movies.find({
  $or: [
    { year: { $lt: 2000 } },
    { year: { $gt: 2015 } }
  ]
})
```

**Marking:** 1 point per correct query.

### Q19. Update and delete (4 points)

**1\. Update The Matrix rating**

```
db.movies.updateOne(
  { title: "The Matrix" },
  { $set: { rating: 9.0 } }
)
```

`$set` changes the field or creates it if it is missing.

**2\. Update every Adventure movie**

```
db.movies.updateMany(
  { genre: "Adventure" },
  { $set: { genre: "Action" } }
)
```

**3\. Delete one Toy Story document**

```
db.movies.deleteOne({
  title: "Toy Story"
})
```

**4\. Difference**

`deleteOne()` removes at most one matching document. `deleteMany()` removes every matching document. If several documents match a `deleteOne()` filter, the operation does not promise which matching document is selected unless the query and application logic ensure uniqueness.

**Marking:** 1 point per part.

### Q20. Upsert and field existence (3 points)

1. If the student exists, MongoDB updates the student's `age` to `22`.
2. If no student matches, MongoDB inserts a new document using the equality filter and update information. The resulting document includes `student_id: 101` and `age: 22`.
3. `$exists: true` checks whether the field is present. A non-null check additionally excludes a field whose value is `null`.

Example non-null filter:

```
db.movies.find({
  rating: { $exists: true, $ne: null }
})
```

**Marking:** 1 point per part.

### Q21. Sorting (3 points)

```
db.movies.find({
  genre: "Sci-Fi"
}).sort({
  rating: -1,
  year: 1
})
```

- `-1` sorts descending, so higher ratings appear first.
- `1` sorts ascending, so older years appear first when ratings tie.

**Marking:**

- 1 point: correct filter.
- 1 point: correct sort fields and directions.
- 1 point: correct explanation.

## Section D — Model Answers (18 points)

### Q22. Select a data model (4 points)

1. **Key-value:** Efficient direct access using session keys.
2. **Wide-column:** Suitable for distributed, high-volume sensor data when partitioning and query patterns are designed appropriately.
3. **Document:** Supports nested addresses and preferences in flexible customer documents.
4. **Graph:** Supports traversals across accounts and transactions to identify suspicious connections.

**Marking:** 1 point per correct model and justification.

### Q23. Vertical and horizontal scaling (3 points)

1. **Vertical:** Upgrade the existing server's CPU, memory or storage.
2. **Horizontal:** Add nodes and distribute data or requests across them.
3. **Advantage:** More aggregate capacity and potentially better resilience. **Limitation:** Distribution introduces coordination, replication, network and operational complexity.

**Marking:** 1 point per part.

### Q24. CAP scenario (4 points)

1. During a network partition, a system cannot guarantee both strong consistency and availability for every affected request while continuing to tolerate the partition.
2. **CP-oriented:** May reject or delay inventory-changing operations when it cannot safely coordinate updates.
3. **AP-oriented:** May continue accepting operations on both sides, potentially producing temporary disagreement or conflicting inventory updates that need reconciliation.
4. CAP describes guarantees during a partition. Systems may behave differently when communication is healthy, and different operations or configurations can have different guarantees.

An AP design does not automatically resolve conflicts correctly, and a CP design does not necessarily mean the entire database becomes unavailable.

**Marking:** 1 point per part.

### Q25. Aggregation fundamentals (4 points)

1. `$match` filters the input documents to those whose `genre` is `Sci-Fi`.
2. `_id: "$genre"` groups the remaining documents by their `genre` value.
3. `$sum: 1` counts the documents in each group by adding one for each document.
4. `$avg` ignores missing and `null` values for the field being averaged.

**Marking:** 1 point per explanation.

### Q26. Row-oriented versus column-oriented storage (3 points)

Column-oriented analytical storage can read only the `balance` values rather than retrieving unrelated fields for millions of customers. This can reduce unnecessary data reads and improve analytical scans and aggregations.

Row-oriented storage is often effective when an application retrieves a complete customer record by ID because the attributes of each record are stored together.

**Marking:**

- 1 point: reduced unnecessary column reads.
- 1 point: analytical workload benefit.
- 1 point: appropriate contrast with complete-record retrieval.

---

# Mock Exam 2 — Applied Problems

**Duration:** 60 minutes\
**Total marks:** 60 points\
**Difficulty:** Intermediate to advanced

## Instructions

- Write MongoDB shell commands where appropriate.
- Pay attention to inclusive and exclusive boundaries.
- Distinguish document filtering from group filtering.
- Explain the order in which aggregation stages operate.
- Assume all required fields exist unless a question explicitly mentions missing or null values.

## Section A — Applied Multiple Choice (8 points)

**8 questions × 1 point**

### Q1. Comparison boundaries

Which query returns documents with `year` equal to 2010 or later?

A. `{ year: { $gt: 2010 } }`\
B. `{ year: { $gte: 2010 } }`\
C. `{ year: { $lt: 2010 } }`\
D. `{ year: { $lte: 2010 } }`

### Q2. Array conditions

A movie must contain both `Action` and `Thriller` in its `genres` array. Which operator is most appropriate?

A. `$in`\
B. `$exists`\
C. `$all`\
D. `$size`

### Q3. Update behavior

What does the following command do?

```
db.movies.updateOne(
  { genre: "Comedy" },
  { $set: { genre: "Drama" } }
)
```

A. Updates every Comedy movie.\
B. Updates at most one matching document.\
C. Inserts a new document in every case.\
D. Deletes one matching document.

### Q4. Aggregation stage order

What is the result of placing `$limit: 3` before `$sort` in a pipeline?

A. MongoDB always sorts every document before limiting.\
B. The pipeline limits the input to three documents before sorting those documents.\
C. `$limit` becomes equivalent to `$match`.\
D. The pipeline groups documents automatically.

### Q5. Counting grouped documents

Inside `$group`, what does `{ $sum: 1 }` usually calculate?

A. The sum of a field called `1`.\
B. The number of documents in each group.\
C. The average of every numeric field.\
D. The number of distinct groups.

### Q6. Missing fields

Which statement about `{ rating: { $exists: true } }` is correct?

A. It excludes every document where `rating` is `null`.\
B. It matches only numeric ratings.\
C. It matches documents where the field exists, even if its value is `null`.\
D. It creates the `rating` field.

### Q7. Wide-column databases

Which statement about Cassandra is most appropriate?

A. It is a property graph database.\
B. It is a wide-column database designed for distributed data workloads.\
C. It is exclusively an in-memory key-value store.\
D. It stores data only in traditional relational tables with mandatory foreign keys.

### Q8. CAP and partitions

A service continues answering requests during a partition but may return stale data. Which design priority does this illustrate?

A. Availability over strong consistency for affected operations\
B. Durability over atomicity\
C. Vertical over horizontal scaling\
D. Schema enforcement over flexible data

## Section B — MongoDB CRUD and Query Design (18 points)

Use this `products` collection:

```
[
  {
    product_id: 1,
    name: "Laptop",
    category: "Electronics",
    price: 1200,
    tags: ["work", "portable"],
    stock: 12
  },
  {
    product_id: 2,
    name: "Phone",
    category: "Electronics",
    price: 800,
    tags: ["portable", "mobile"],
    stock: 25
  },
  {
    product_id: 3,
    name: "Desk",
    category: "Furniture",
    price: 300,
    tags: ["office", "wood"],
    stock: 8
  },
  {
    product_id: 4,
    name: "Chair",
    category: "Furniture",
    price: 150,
    tags: ["office", "ergonomic"],
    stock: 0
  },
  {
    product_id: 5,
    name: "Monitor",
    category: "Electronics",
    price: 400,
    tags: ["work", "display"],
    stock: 15
  }
]
```

### Q9. Compound filtering (4 points)

Write queries to:

1. Find Electronics products costing at least 500.
2. Find products costing between 200 and 1000 inclusive.
3. Find products whose tags contain either `work` or `office`.
4. Find products whose tags contain both `office` and `wood`.

### Q10. Sorting and result selection (3 points)

Write a query that:

- Selects products in the Electronics category.
- Sorts by price descending.
- Uses `product_id` ascending to break price ties.
- Returns only the first two results.

Explain why the order of sorting and limiting matters.

### Q11. Updates and deletion (4 points)

Write commands to:

1. Increase the price of product 3 to 350.
2. Set `stock` to zero for every product in the Furniture category.
3. Insert a product with `product_id: 6`, name `Keyboard`, category `Electronics`, price `75`, tags `["work", "peripheral"]`, and stock `40`.
4. Delete all products whose stock is zero.

### Q12. Upsert and safe modification (3 points)

Write an upsert that sets the stock of product 10 to 50.

Then explain:

- What happens if product 10 already exists.
- What happens if it does not exist.
- Why a broad update filter can be dangerous.

### Q13. Field existence and arrays (4 points)

Assume some products have no `discount` field, some have `discount: null`, and others have numeric discounts.

Write queries to:

1. Find products where `discount` exists.
2. Find products where `discount` is present and non-null.
3. Find products whose tags array contains exactly two elements.
4. Explain why the first two queries return different results.

## Section C — Aggregation Pipelines (20 points)

Use the following `sales` documents:

```
[
  {
    order_id: 1,
    region: "North",
    category: "Electronics",
    amount: 1200,
    status: "completed"
  },
  {
    order_id: 2,
    region: "North",
    category: "Furniture",
    amount: 400,
    status: "completed"
  },
  {
    order_id: 3,
    region: "South",
    category: "Electronics",
    amount: 800,
    status: "completed"
  },
  {
    order_id: 4,
    region: "South",
    category: "Furniture",
    amount: 200,
    status: "cancelled"
  },
  {
    order_id: 5,
    region: "North",
    category: "Electronics",
    amount: 600,
    status: "completed"
  },
  {
    order_id: 6,
    region: "South",
    category: "Electronics",
    amount: 1000,
    status: "completed"
  }
]
```

### Q14. Group sales by region (4 points)

Write an aggregation pipeline that calculates the number of completed sales and total completed sales amount for each region.

### Q15. Category-level averages (4 points)

Write a pipeline that:

- Includes only completed sales.
- Groups by category.
- Calculates the number of sales and average amount.
- Returns only categories whose average amount exceeds 700.

### Q16. Top three sales (4 points)

Write a pipeline returning the three highest-value completed sales.

Include `order_id`, `region`, and `amount`, excluding `_id`.

Explain the purpose of each stage.

### Q17. Debug an aggregation (4 points)

A student writes:

```
db.sales.aggregate([
  {
    $group: {
      _id: "$region",
      total: { $sum: "$amount" }
    }
  },
  {
    $match: {
      status: "completed"
    }
  }
])
```

Explain why the pipeline does not correctly calculate completed sales by region. Rewrite it.

### Q18. Multi-dimensional aggregation (4 points)

Write a pipeline that groups completed sales by both region and category and calculates total sales amount for each combination.

Use a compound `_id` in the `$group` stage.

## Section D — NoSQL Architecture and Data Modeling (14 points)

### Q19. Database selection under constraints (4 points)

A company has the following requirements:

- User sessions must be retrieved quickly by session ID.
- Customer profiles contain nested preferences and optional attributes.
- Sensor events arrive at high volume and must be distributed across many nodes.
- Fraud analysts need to traverse relationships between accounts and transactions.

Recommend a database model for each requirement. Explain one important design consideration for each.

### Q20. ACID and BASE in an e-commerce system (4 points)

An e-commerce system has two operations:

- Operation A transfers money between two accounts.
- Operation B updates a replicated product description.

Explain which guarantees or approach would be particularly important for each operation. Discuss whether using NoSQL automatically rules out ACID transactions.

### Q21. CAP trade-off in inventory management (3 points)

Two warehouse replicas lose network connectivity.

Replica A records a sale of the last item. Replica B also accepts a sale because it cannot see A's update.

1. Explain the consistency problem.
2. Explain how a CP-oriented design could reduce the risk.
3. Explain the availability trade-off.

### Q22. Wide-column versus column-oriented storage (3 points)

Explain the difference between a wide-column NoSQL database and a column-oriented analytical database. Give one suitable use case for each.

---

# Mock Exam 2 — Complete Solutions and Marking Guide

## Section A — Answers (8 points)

| Question | Answer | Explanation |
| --- | --- | --- |
| Q1 | B | `$gte` includes 2010. |
| Q2 | C | `$all` requires every specified array value. |
| Q3 | B | `updateOne()` changes at most one matching document. |
| Q4 | B | `$limit` passes only three documents to the following stage. |
| Q5 | B | `$sum: 1` counts documents within each group. |
| Q6 | C | Field existence does not imply a non-null value. |
| Q7 | B | Cassandra is a distributed wide-column database. |
| Q8 | A | Availability is prioritized even when some reads may be stale. |

## Section B — Model Answers (18 points)

### Q9. Compound filtering (4 points)

**1\. Electronics costing at least 500**

```
db.products.find({
  category: "Electronics",
  price: { $gte: 500 }
})
```

**2\. Prices between 200 and 1000 inclusive**

```
db.products.find({
  price: { $gte: 200, $lte: 1000 }
})
```

**3\. Tags contain either work or office**

```
db.products.find({
  tags: { $in: ["work", "office"] }
})
```

**4\. Tags contain both office and wood**

```
db.products.find({
  tags: { $all: ["office", "wood"] }
})
```

**Marking:** 1 point per query.

### Q10. Sorting and result selection (3 points)

```
db.products.find({
  category: "Electronics"
}).sort({
  price: -1,
  product_id: 1
}).limit(2)
```

The sort establishes the desired order before the limit selects the first two documents. If limiting occurs first, the result may omit products that should rank highest.

**Marking:**

- 1 point: filter.
- 1 point: correct multi-field sort.
- 1 point: limit and explanation.

### Q11. Updates and deletion (4 points)

**1\. Set product 3's price**

```
db.products.updateOne(
  { product_id: 3 },
  { $set: { price: 350 } }
)
```

**2\. Set Furniture stock to zero**

```
db.products.updateMany(
  { category: "Furniture" },
  { $set: { stock: 0 } }
)
```

**3\. Insert Keyboard**

```
db.products.insertOne({
  product_id: 6,
  name: "Keyboard",
  category: "Electronics",
  price: 75,
  tags: ["work", "peripheral"],
  stock: 40
})
```

**4\. Delete products with zero stock**

```
db.products.deleteMany({
  stock: 0
})
```

**Marking:** 1 point per command.

### Q12. Upsert and safe modification (3 points)

```
db.products.updateOne(
  { product_id: 10 },
  { $set: { stock: 50 } },
  { upsert: true }
)
```

If product 10 exists, its stock is set to 50. Otherwise, MongoDB inserts a new document using the equality filter and update information.

A broad filter such as `{}` can match every document in an `updateMany()` operation. A missing or incorrect filter can therefore modify many unintended records.

**Marking:**

- 1 point: correct upsert.
- 1 point: behavior for an existing or missing document.
- 1 point: explanation of update-filter safety.

### Q13. Field existence and arrays (4 points)

**1\. Discount field exists**

```
db.products.find({
  discount: { $exists: true }
})
```

**2\. Discount exists and is non-null**

```
db.products.find({
  discount: { $exists: true, $ne: null }
})
```

**3\. Exactly two tags**

```
db.products.find({
  tags: { $size: 2 }
})
```

**4\. Explanation**

The first query checks only field presence. It includes documents with `discount: null`. The second excludes those null values.

**Marking:** 1 point per part.

## Section C — Model Answers (20 points)

### Q14. Group sales by region (4 points)

```
db.sales.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: "$region",
      salesCount: { $sum: 1 },
      totalAmount: { $sum: "$amount" }
    }
  }
])
```

`$match` excludes cancelled sales. `$group` creates one result per region. `$sum: 1` counts completed sales, while `$sum: "$amount"` totals their values.

Expected totals for the supplied documents:

| Region | Sales count | Total amount |
| --- | --- | --- |
| North | 3 | 2200 |
| South | 2 | 1800 |

**Marking:**

- 1 point: completed-sales filter.
- 1 point: grouping by region.
- 1 point: count.
- 1 point: total amount.

### Q15. Category-level averages (4 points)

```
db.sales.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: "$category",
      salesCount: { $sum: 1 },
      averageAmount: { $avg: "$amount" }
    }
  },
  {
    $match: {
      averageAmount: { $gt: 700 }
    }
  }
])
```

The first `$match` filters input sales. `$group` calculates category-level aggregates. The final `$match` filters the grouped results by average amount.

Expected results:

| Category | Sales count | Average amount |
| --- | --- | --- |
| Electronics | 4 | 900 |

Furniture has only one completed sale, worth 400, so it does not qualify.

**Marking:** 1 point per correct stage or aggregate requirement.

### Q16. Top three sales (4 points)

```
db.sales.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $sort: {
      amount: -1,
      order_id: 1
    }
  },
  {
    $limit: 3
  },
  {
    $project: {
      _id: 0,
      order_id: 1,
      region: 1,
      amount: 1
    }
  }
])
```

Explanation:

1. `$match` keeps completed sales.
2. `$sort` orders sales by amount descending, with order ID as a tie-breaker.
3. `$limit` retains the first three documents.
4. `$project` selects the requested fields and excludes `_id`.

Expected order IDs are 1, 6 and 3, with amounts 1200, 1000 and 800.

**Marking:** 1 point per correctly implemented requirement.

### Q17. Debug an aggregation (4 points)

The original pipeline groups documents before filtering by `status`. After `$group`, the documents contain `_id` and `total`, not the original `status` field. Therefore, the later `$match` cannot correctly filter the original sales.

Correct version:

```
db.sales.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: "$region",
      total: { $sum: "$amount" }
    }
  }
])
```

The filter must operate on the original documents before grouping.

**Marking:**

- 2 points: correctly identifies the stage-order and field-availability problem.
- 2 points: correct pipeline.

### Q18. Multi-dimensional aggregation (4 points)

```
db.sales.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: {
        region: "$region",
        category: "$category"
      },
      totalAmount: { $sum: "$amount" },
      salesCount: { $sum: 1 }
    }
  }
])
```

The compound `_id` creates a separate group for each region-category combination.

Expected totals:

| Region | Category | Sales count | Total amount |
| --- | --- | --- | --- |
| North | Electronics | 2 | 1800 |
| North | Furniture | 1 | 400 |
| South | Electronics | 2 | 1800 |

**Marking:**

- 1 point: completed-sales filter.
- 1 point: compound grouping key.
- 1 point: correct total.
- 1 point: correct count.

## Section D — Model Answers (14 points)

### Q19. Database selection under constraints (4 points)

- **Sessions:** Key-value database. Consider key uniqueness, expiration and session invalidation.
- **Customer profiles:** Document database. Consider validation, nested structures and whether frequently accessed data should be embedded.
- **Sensor events:** Wide-column database. Consider partition keys, data distribution, write volume and query patterns.
- **Fraud analysis:** Graph database. Consider relationship direction, traversal depth and the properties needed on nodes and edges.

**Marking:** 1 point per appropriate model and consideration.

### Q20. ACID and BASE in e-commerce (4 points)

**Operation A — Money transfer:** Atomicity is critical because the debit and credit must succeed together. Consistency, isolation and durability also matter because the transfer must preserve integrity, handle concurrent transactions correctly and survive failures after commit.

**Operation B — Product description replication:** Eventual consistency may be acceptable if replicas temporarily display different descriptions, provided updates eventually propagate. The application must decide whether stale descriptions are tolerable.

NoSQL does not automatically rule out ACID transactions. Capabilities depend on the database, its transaction model and configuration.

**Marking:**

- 2 points: appropriate transaction guarantees for the transfer.
- 1 point: reasonable consistency choice for replicated descriptions.
- 1 point: explains that NoSQL can support ACID.

### Q21. CAP trade-off in inventory management (3 points)

1. Both replicas may sell the same final item because neither observes the other's update. The result is conflicting inventory state or overselling.
2. A CP-oriented design can reject or delay operations that cannot be coordinated safely during the partition.
3. The cost is reduced availability for affected inventory operations. An AP-oriented approach could accept sales but would need conflict resolution and reconciliation.

**Marking:** 1 point per part.

### Q22. Wide-column versus column-oriented storage (3 points)

A **wide-column NoSQL database**, such as Cassandra, organizes distributed data around partition or row keys and flexible groups of columns. It is useful for high-volume operational workloads such as sensor events.

A **column-oriented analytical database** stores values by column to support scans and aggregations over selected fields. It is useful for calculating statistics over millions of records while reading only a few columns.

They are related concepts but are not interchangeable.

**Marking:**

- 1 point: wide-column explanation.
- 1 point: analytical column-oriented explanation.
- 1 point: suitable examples or use cases.

---

# Mock Exam 3 — Comprehensive Challenge

**Duration:** 60 minutes\
**Total marks:** 60 points\
**Difficulty:** Advanced

## Instructions

- Read every scenario carefully.
- Show your reasoning for database selection and distributed-system questions.
- Use valid MongoDB shell syntax.
- Distinguish filters on individual documents from filters on aggregated results.
- Consider missing values, duplicate matches and consistency requirements.
- Equivalent correct solutions receive credit.

## Section A — Integrated Conceptual Challenge (10 points)

### Q1. CAP and business requirements (3 points)

A ticketing platform sells the final seat for an event. Two data-center replicas lose connectivity.

The business requires that no two customers successfully purchase the same final seat.

1. Which guarantee is more important: strong consistency or accepting every request?
2. Explain how a CP-oriented design might respond.
3. Explain the consequence for availability during the partition.

### Q2. Model selection and trade-offs (3 points)

A company operates:

- A product catalog containing nested product specifications.
- A recommendation service that explores connections among customers and products.
- A session cache accessed directly by session key.

Choose a suitable model for each and explain why using a single data model for all three may be inefficient.

### Q3. ACID and BASE (2 points)

A replicated application updates a user's profile. One replica shows the new value while another temporarily shows the old value.

Explain:

1. Which distributed-system concept this illustrates.
2. Why the behavior does not automatically mean that the database violates ACID.

### Q4. Scaling and architecture (2 points)

A database is approaching the limits of one server.

Explain the difference between scaling vertically and horizontally. Identify one additional complexity introduced by horizontal scaling.

## Section B — Advanced MongoDB Queries (16 points)

Use the following `movies` collection:

```
[
  {
    title: "Film A",
    year: 2005,
    genre: "Sci-Fi",
    rating: 8.2,
    genres: ["Action", "Sci-Fi"]
  },
  {
    title: "Film B",
    year: 2010,
    genre: "Drama",
    rating: 7.5,
    genres: ["Drama", "Thriller"]
  },
  {
    title: "Film C",
    year: 2015,
    genre: "Sci-Fi",
    rating: 9.1,
    genres: ["Action", "Sci-Fi", "Adventure"]
  },
  {
    title: "Film D",
    year: 1995,
    genre: "Comedy",
    genres: ["Comedy"]
  },
  {
    title: "Film E",
    year: 2020,
    genre: "Sci-Fi",
    rating: 8.7,
    genres: ["Sci-Fi", "Drama"]
  },
  {
    title: "Film F",
    year: 2010,
    genre: "Drama",
    rating: null,
    genres: ["Drama", "Action"]
  }
]
```

### Q5. Combined filters (4 points)

Write queries to:

1. Find movies released from 2010 through 2020 inclusive with a rating greater than 8.
2. Find movies that are either Sci-Fi or released before 2000.
3. Find movies whose genres contain both `Drama` and `Action`.
4. Find movies where the rating field exists and is non-null.

### Q6. Sorting and boundary reasoning (3 points)

Write a query that selects movies released after 2000, sorts by rating descending and year ascending, and returns the first four results.

Explain why Film D is excluded and why a movie with a missing rating may affect how results should be interpreted.

### Q7. Update correctness (3 points)

An administrator wants to change the rating to 8.0 for all movies whose genre is Drama.

Write the correct update. Explain why `updateOne()` would not meet the stated requirement.

### Q8. Upsert and deletion safety (3 points)

1. Write an upsert that sets `rating` to 7.0 for the movie titled `Film G`.
2. Write a command to delete all movies whose year is earlier than 2000.
3. Identify one precaution before executing a bulk deletion.

### Q9. Array operator distinction (3 points)

Explain the difference between `$in`, `$all` and `$size`. Give one short example of each.

## Section C — Comprehensive Aggregation (18 points)

Use this `orders` collection:

```
[
  {
    order_id: 1,
    region: "North",
    category: "Electronics",
    amount: 1200,
    status: "completed"
  },
  {
    order_id: 2,
    region: "North",
    category: "Furniture",
    amount: 500,
    status: "completed"
  },
  {
    order_id: 3,
    region: "South",
    category: "Electronics",
    amount: 900,
    status: "completed"
  },
  {
    order_id: 4,
    region: "South",
    category: "Furniture",
    amount: 300,
    status: "cancelled"
  },
  {
    order_id: 5,
    region: "North",
    category: "Electronics",
    amount: 700,
    status: "completed"
  },
  {
    order_id: 6,
    region: "South",
    category: "Electronics",
    amount: 1100,
    status: "completed"
  },
  {
    order_id: 7,
    region: "North",
    category: "Furniture",
    amount: 600,
    status: "completed"
  }
]
```

### Q10. Multi-stage regional analysis (5 points)

Write a pipeline that:

1. Includes completed orders only.
2. Groups by region.
3. Calculates the number of completed orders, total revenue and average order amount.
4. Sorts by total revenue descending.
5. Returns the two highest-revenue regions.

### Q11. Filter after grouping (4 points)

Write a pipeline that calculates total completed revenue by category and returns only categories whose total revenue is at least 1500.

Explain why the final threshold belongs in a `$match` stage after `$group`.

### Q12. Compound grouping (4 points)

Write a pipeline that groups completed orders by both region and category, calculating the order count and total revenue for every combination.

Explain the role of the compound `_id`.

### Q13. Debugging aggregation logic (5 points)

A student writes:

```
db.orders.aggregate([
  {
    $group: {
      _id: "$region",
      totalRevenue: { $sum: "$amount" },
      orderCount: { $sum: 1 }
    }
  },
  {
    $match: {
      status: "completed"
    }
  },
  {
    $sort: {
      totalRevenue: -1
    }
  }
])
```

Answer the following:

1. Identify the main logical error.
2. Explain which information is lost after grouping.
3. Rewrite the pipeline to calculate completed revenue by region.
4. Explain why the corrected pipeline produces the intended result.

## Section D — Data Modeling and System Design (16 points)

### Q14. Design a NoSQL architecture (4 points)

A global online platform has four workloads:

- Product documents with optional and nested specifications.
- Session retrieval using unique session keys.
- High-volume telemetry distributed across many nodes.
- Fraud analysis involving multi-hop relationships among accounts.

Recommend an appropriate model for each workload. State one important design consideration for each.

### Q15. CAP and eventual consistency (4 points)

A shopping application continues accepting product updates while two replicas are disconnected. During the partition, different users see different product prices.

1. Explain why this can occur.
2. Describe the likely availability–consistency priority.
3. Explain what eventual consistency requires after communication is restored and updates stop.
4. Explain why eventual consistency alone does not guarantee that every intermediate read is correct.

### Q16. RDF versus property graph (4 points)

A knowledge platform integrates information from multiple organizations. Each organization may describe people, companies and relationships using different identifiers and vocabularies.

1. Explain why RDF may be appropriate.
2. Describe the RDF triple structure.
3. Explain how URIs help.
4. Contrast this with a property graph that stores directed relationships and key-value properties.

### Q17. NoSQL architecture decision (4 points)

A team proposes migrating its entire application from SQL to NoSQL because “NoSQL is always faster, more scalable and more available.”

Evaluate this proposal. Identify four considerations that should influence the database choice.

---

# Mock Exam 3 — Complete Solutions and Marking Guide

## Section A — Model Answers (10 points)

### Q1. CAP and business requirements (3 points)

1. Strong consistency is critical because the same final seat must not be sold twice.
2. A CP-oriented design may reject or delay a purchase when the replica cannot safely coordinate with the authoritative state.
3. Some customers may temporarily be unable to purchase seats during the partition. This sacrifices availability for the affected operation to protect correctness.

**Marking:** 1 point per part.

### Q2. Model selection and trade-offs (3 points)

- **Product catalog:** Document database, because product attributes can be nested and vary between products.
- **Recommendations:** Graph database, because the application must traverse relationships between customers and products.
- **Session cache:** Key-value database, because sessions are retrieved directly using unique keys.

A single model may require inefficient workarounds or provide less natural query support for some workloads.

**Marking:** 1 point per model with a valid explanation.

### Q3. ACID and BASE (2 points)

The scenario illustrates temporary inconsistency associated with asynchronous replication and eventual consistency.

It does not automatically indicate an ACID violation. The database's transaction guarantees, replication configuration and consistency model determine whether the observed behavior is permitted.

**Marking:** 1 point per part.

### Q4. Scaling and architecture (2 points)

Vertical scaling increases the resources of one server. Horizontal scaling adds servers or nodes and distributes the workload.

Horizontal scaling introduces complexity such as replication coordination, partitioning, distributed transactions, network failures or conflict resolution.

**Marking:** 1 point per part.

## Section B — Model Answers (16 points)

### Q5. Combined filters (4 points)

**1\. Years 2010–2020 inclusive, rating greater than 8**

```
db.movies.find({
  year: { $gte: 2010, $lte: 2020 },
  rating: { $gt: 8 }
})
```

**2\. Sci-Fi or released before 2000**

```
db.movies.find({
  $or: [
    { genre: "Sci-Fi" },
    { year: { $lt: 2000 } }
  ]
})
```

**3\. Both Drama and Action**

```
db.movies.find({
  genres: { $all: ["Drama", "Action"] }
})
```

**4\. Rating exists and is non-null**

```
db.movies.find({
  rating: { $exists: true, $ne: null }
})
```

**Marking:** 1 point per query.

### Q6. Sorting and boundary reasoning (3 points)

```
db.movies.find({
  year: { $gt: 2000 }
}).sort({
  rating: -1,
  year: 1
}).limit(4)
```

Film D is excluded because its year, 1995, does not satisfy the strict greater-than-2000 condition.

A missing or null rating does not represent a normal numeric rating. MongoDB sorting includes documents with missing or null sort fields, so applications that require rated movies should filter for a valid rating explicitly.

**Marking:**

- 1 point: filter.
- 1 point: sorting and limit.
- 1 point: boundary and missing-value explanation.

### Q7. Update correctness (3 points)

```
db.movies.updateMany(
  { genre: "Drama" },
  { $set: { rating: 8.0 } }
)
```

`updateOne()` modifies at most one matching document. Since the requirement applies to every Drama movie, `updateMany()` is appropriate.

**Marking:**

- 2 points: correct command.
- 1 point: explanation.

### Q8. Upsert and deletion safety (3 points)

**1\. Upsert Film G**

```
db.movies.updateOne(
  { title: "Film G" },
  { $set: { rating: 7.0 } },
  { upsert: true }
)
```

**2\. Delete movies released before 2000**

```
db.movies.deleteMany({
  year: { $lt: 2000 }
})
```

**3\. Precaution**

Run a matching `find()` query first to inspect the affected documents, verify the filter and confirm that deletion is intended. A backup or transaction may also be appropriate depending on the system and operation.

**Marking:** 1 point per part.

### Q9. Array operator distinction (3 points)

- **`$in`****:** Matches if at least one specified value is present in the array.```
  db.movies.find({
    genres: { $in: ["Action", "Drama"] }
  })
  ```
- **`$all`****:** Requires every specified value to be present.```
  db.movies.find({
    genres: { $all: ["Action", "Drama"] }
  })
  ```
- **`$size`****:** Matches arrays with exactly the specified number of elements.```
  db.movies.find({
    genres: { $size: 2 }
  })
  ```

**Marking:** 1 point per operator and correct explanation/example.

## Section C — Model Answers (18 points)

### Q10. Multi-stage regional analysis (5 points)

```
db.orders.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: "$region",
      orderCount: { $sum: 1 },
      totalRevenue: { $sum: "$amount" },
      averageOrderAmount: { $avg: "$amount" }
    }
  },
  {
    $sort: {
      totalRevenue: -1
    }
  },
  {
    $limit: 2
  }
])
```

Expected results:

| Region | Orders | Total revenue | Average order amount |
| --- | --- | --- | --- |
| North | 4 | 3000 | 750 |
| South | 2 | 2000 | 1000 |

The cancelled order is excluded before aggregation.

**Marking:**

- 1 point: completed-order filter.
- 1 point: regional grouping.
- 1 point: correct aggregates.
- 1 point: descending revenue sort.
- 1 point: limit of two.

### Q11. Filter after grouping (4 points)

```
db.orders.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: "$category",
      totalRevenue: { $sum: "$amount" }
    }
  },
  {
    $match: {
      totalRevenue: { $gte: 1500 }
    }
  }
])
```

Expected results:

| Category | Total revenue |
| --- | --- |
| Electronics | 3900 |
| Furniture | 1100 |

Only Electronics qualifies.

The threshold belongs after `$group` because it concerns the calculated total for each category, not the amount of an individual order. The `$match` stage before grouping filters individual source documents; the `$match` after grouping filters aggregate results.

**Marking:**

- 2 points: correct pipeline.
- 1 point: correct threshold placement.
- 1 point: explanation.

### Q12. Compound grouping (4 points)

```
db.orders.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: {
        region: "$region",
        category: "$category"
      },
      orderCount: { $sum: 1 },
      totalRevenue: { $sum: "$amount" }
    }
  }
])
```

Expected results:

| Region | Category | Orders | Total revenue |
| --- | --- | --- | --- |
| North | Electronics | 2 | 1900 |
| North | Furniture | 2 | 1100 |
| South | Electronics | 2 | 2000 |

The compound `_id` defines a distinct group for each combination of region and category.

**Marking:**

- 1 point: filter.
- 1 point: compound key.
- 1 point: count.
- 1 point: total revenue.

### Q13. Debugging aggregation logic (5 points)

**1\. Main error**

The pipeline groups documents before filtering by status.

**2\. Lost information**

After grouping, the output documents contain the group key and calculated aggregates. The original `status` field is no longer available unless it was explicitly included in the grouping output or another retained field.

**3\. Correct pipeline**

```
db.orders.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: "$region",
      totalRevenue: { $sum: "$amount" },
      orderCount: { $sum: 1 }
    }
  },
  {
    $sort: {
      totalRevenue: -1
    }
  }
])
```

**4\. Why it works**

The initial `$match` removes cancelled orders while their status is still available. `$group` then calculates revenue and order counts using only completed orders. `$sort` orders the regional totals from highest to lowest.

Expected results:

| Region | Total revenue | Orders |
| --- | --- | --- |
| North | 3000 | 4 |
| South | 2000 | 2 |

**Marking:**

- 1 point: identifies the incorrect stage order.
- 1 point: explains loss of the original field.
- 2 points: correct pipeline.
- 1 point: explains why it works.

## Section D — Model Answers (16 points)

### Q14. Design a NoSQL architecture (4 points)

- **Product specifications:** Document database. Consider document validation, nested attributes and whether embedding is appropriate.
- **Session retrieval:** Key-value database. Consider key uniqueness, expiration and lookup latency.
- **Telemetry:** Wide-column database. Consider partition-key design, write throughput and the distribution of time-series data.
- **Fraud analysis:** Graph database. Consider relationship types, traversal depth and how account-to-account connections are represented.

**Marking:** 1 point per model and design consideration.

### Q15. CAP and eventual consistency (4 points)

1. Replicas may accept updates independently while communication is unavailable, so they cannot immediately exchange changes.
2. The system appears to prioritize availability for these operations over strong consistency during the partition.
3. After communication is restored and updates stop, successful replication should bring the replicas to a converged state.
4. Eventual consistency permits temporary stale reads and does not guarantee that an arbitrary read during convergence returns the latest globally accepted value. Conflict-resolution rules are also needed if concurrent updates disagree.

**Marking:** 1 point per part.

### Q16. RDF versus property graph (4 points)

1. RDF is appropriate when information must be integrated across organizations using shared semantic descriptions and interoperable identifiers.
2. An RDF triple has the form `(Subject, Predicate, Object)`, for example `(Alice, worksFor, Acme)`.
3. URIs provide identifiers that allow systems to refer to resources consistently, reducing ambiguity between different names or representations.
4. A property graph represents nodes, directed relationships and key-value properties. For example:```
   (Alice:Person)-[WORKS_AT {since: 2024}]->(Acme:Company)
   ```
   This relationship has a type, direction and a `since` property.

**Marking:** 1 point per part.

### Q17. NoSQL architecture decision (4 points)

The proposal is too broad. NoSQL is not universally faster, more scalable or more available than SQL. The correct choice depends on workload and implementation.

Four important considerations are:

1. **Data structure and relationships:** Determine whether the application needs relational joins, nested documents, direct key lookups or graph traversals.
2. **Consistency and transactions:** Establish whether operations require strong consistency, multi-record transactions, or tolerance for temporarily stale replicas.
3. **Workload and performance:** Evaluate read/write patterns, query selectivity, latency targets, throughput and indexing requirements.
4. **Scale and operational constraints:** Consider data volume, distribution, replication, availability, partitioning, cost, backup and recovery.

A hybrid architecture may be appropriate when different workloads have different requirements.

**Marking:** 1 point per distinct, correctly explained consideration.

---

# Final Revision Checklist — T3

Use this checklist after completing the three exams.

- [ ]Explain SQL versus NoSQL without claiming that NoSQL has no schema or transactions.
- [ ]Distinguish vertical scaling from horizontal scaling.
- [ ]Explain all four ACID properties.
- [ ]Distinguish ACID, BASE and CAP.
- [ ]Explain the CAP trade-off specifically during a network partition.
- [ ]Select document, key-value, wide-column and graph databases for suitable workloads.
- [ ]Explain MongoDB databases, collections, documents and fields.
- [ ]Write `insertOne()`, `insertMany()`, `find()`, `updateOne()`, `updateMany()`, `deleteOne()` and `deleteMany()` commands.
- [ ]Explain upsert behavior.
- [ ]Use `$gt`, `$gte`, `$lt`, `$lte`, `$in`, `$nin`, `$and`, `$or`, `$not`, `$nor` and `$exists`.
- [ ]Distinguish field existence from non-null values.
- [ ]Use `$in`, `$all` and `$size` correctly with arrays.
- [ ]Sort ascending with `1` and descending with `-1`.
- [ ]Build aggregation pipelines using `$match`, `$group`, `$project`, `$sort` and `$limit`.
- [ ]Explain why filtering individual documents differs from filtering grouped results.
- [ ]Use `$sum: 1`, `$sum: "$field"` and `$avg: "$field"` correctly.
- [ ]Explain wide-column NoSQL databases versus column-oriented analytical storage.
- [ ]Distinguish RDF triples from property graphs.
- [ ]Explain why database choice must follow application requirements rather than blanket claims about performance.

## Score Interpretation

Each exam is worth 60 points.

| Score | Interpretation |
| --- | --- |
| 54–60 | Excellent understanding |
| 48–53 | Strong preparation |
| 42–47 | Good foundation; review weaker areas |
| 36–41 | Partial understanding; more practice recommended |
| Below 36 | Revisit the T3 compression and repeat the exercises |

These bands are study guidelines, not official university grade boundaries.

**Recommended study sequence:** Complete Mock Exam 1 under timed conditions, review the explanations, attempt Mock Exam 2, and finish with Mock Exam 3 without consulting the solutions.
