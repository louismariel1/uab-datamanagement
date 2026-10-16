Here is the merged, consolidated T1 — Data Models & Data Storage: Exam Compression (Chunks 1–3). I’ve removed repetition, unified the terminology, preserved the high-yield definitions and examples, and combined the practice questions into one revision section. You can save this directly as `T1_Data_Models_Exam_Compression_2026-2027.md`.

Writing

# T1 — Data Models & Data Storage

## Exam Compression — Data Management 2026–2027

**Topics:** Data models, abstraction layers, conceptual/logical/physical models, entities, attributes, relationships, cardinalities, keys, normalization, database schemas, relational databases, and ACID transactions.

---

# 1\. Data System Foundations

A successful data-intensive system balances three essential qualities:

- **Reliability:** Operates correctly and recovers from failures.
- **Scalability:** Handles increasing data volumes, users, and workloads.
- **Maintainability:** Can be understood, modified, and operated efficiently.

### Performance metrics

- **Throughput:** Number of operations processed per unit of time.
- **Latency:** Time required for an operation or request.
- **Response time:** Total time between sending a request and receiving its response.

Data-intensive applications combine databases, caches, storage systems, and processing frameworks.

## What is a data model?

A **data model** is an abstract representation of data, its structure, and its relationships.

It helps define how information is organized, stored, accessed, and maintained.

**Why it matters:** Data models clarify requirements, improve communication, guide software design, and influence performance and scalability.

### Abstraction principle

Each layer presents a simplified view to the layer above it while hiding unnecessary implementation details.

**Exam rule:** Abstraction reduces complexity by hiding implementation details.

---

# 2\. The Three Levels of Data Modeling

Database design typically progresses through three levels.

| Model | Main question | Main contents | DBMS-specific? |
| --- | --- | --- | --- |
| **Conceptual** | What data is needed? | Business entities and relationships | No |
| **Logical** | How is data structured? | Attributes, keys, relationships, normalization | Generally no |
| **Physical** | How is data implemented? | Tables, column types, indexes, constraints, storage | Yes |

## A. Conceptual model

- High-level, business-oriented representation of information.
- Identifies important entities and relationships.
- Used by business stakeholders, analysts, and architects.
- Avoids implementation-specific details.

**Example:** A Customer places Orders.

## B. Logical model

- Defines entities, attributes, keys, and relationships in detail.
- Usually independent of a particular DBMS.
- Supports schema design and normalization.

**Example:** `Customer(CustomerID, Name, Email)`

Here, `CustomerID` can be the primary key.

## C. Physical model

- Specifies the database-specific implementation.
- Defines tables, columns, data types, indexes, constraints, and storage structures.
- Depends on the selected database technology.

**Example:** Define `CustomerID` as `INT PRIMARY KEY`, `Name` as `VARCHAR(100)`, and create an index where appropriate.

**Memory aid: Conceptual = WHAT; Logical = STRUCTURE; Physical = IMPLEMENTATION.**

---

# 3\. Core Data-Modeling Concepts

- **Entity:** A real-world object or concept about which data is stored (e.g., Student, Customer, Order).
- **Attribute:** A property describing an entity (e.g., StudentName, Email).
- **Relationship:** An association between entities (e.g., Customer places Order).
- **Record/tuple:** A row in a relational table.
- **Relation:** A table consisting of rows and columns.
- **Schema:** The structure that defines how data is organized.

## Keys and integrity

### Primary key (PK)

- Uniquely identifies each row.
- Must be unique and non-null.
- Can consist of one or multiple attributes.

### Foreign key (FK)

- References a primary key or another eligible candidate key.
- Establishes relationships between tables.
- Helps enforce referential integrity.

**Exam rule: PK identifies; FK connects.**

Example:

```
Customer(
    CustomerID PRIMARY KEY,
    Name
)

Orders(
    OrderID PRIMARY KEY,
    CustomerID FOREIGN KEY REFERENCES Customer
)
```

Together, PKs and FKs help maintain consistent relationships between records.

---

# 4\. Relationships and Cardinalities

Cardinality describes how many instances of one entity can be associated with instances of another.

## A. One-to-one (1:1)

Each instance of A corresponds to at most one instance of B, and vice versa.

**Example:** UserAccount ↔ CustomerProfile, if each account has exactly one profile and each profile belongs to exactly one account.

## B. One-to-many (1:M or 1:N)

One instance of A can be associated with many instances of B, while each B instance belongs to at most one A.

**Example:** Customer → Orders.

- One customer can place many orders.
- Each order belongs to one customer.

**Implementation:** Put the FK on the many side.

```
Customer(CustomerID PK, Name)
Orders(OrderID PK, CustomerID FK)
```

## C. Many-to-many (M:N)

Many instances of A can be associated with many instances of B.

**Example:** Students ↔ Courses.

- A student can enroll in multiple courses.
- A course can contain multiple students.

**Implementation:** Create a junction (associative) table.

```
Student(StudentID PK, Name)
Course(CourseID PK, Title)

Enrollment(
    StudentID PK, FK,
    CourseID PK, FK
)
```

The composite PK `(StudentID, CourseID)` prevents duplicate student-course pairs. If repeated enrollments must be represented, introduce a separate identifier or additional key attributes.

**Exam rules:**

- 1:1 = one on each side.
- 1:M = FK normally goes on the many side.
- M:N = use a junction table in a relational database.

---

# 5\. Database Normalization

**Normalization** organizes data into related tables to reduce redundancy, prevent update anomalies, and improve consistency and integrity.

### Benefits

- Reduces duplicated information.
- Avoids inconsistent updates.
- Improves data integrity and maintainability.
- Stores each fact in an appropriate place.

## First Normal Form (1NF)

**Rule:** Every attribute contains a single, atomic value; no repeating groups or multivalued cells.

**Problem eliminated:** Non-atomic and repeating data.

Bad example:

| StudentID | PhoneNumbers |
| --- | --- |
| 1 | 111, 222 |

Better: Store each phone number as a separate row in a related table.

## Second Normal Form (2NF)

**Rules:**

1. The relation is in 1NF.
2. Every non-prime attribute is fully functionally dependent on every candidate key; no partial dependency on a proper subset of a candidate key.

**Problem eliminated:** Partial dependencies.

Example: With composite key `(StudentID, CourseID)`, if `StudentName` depends only on `StudentID`, move student details to a separate Student table.

**Key point:** Especially important with composite keys.

## Third Normal Form (3NF)

**Rules:**

1. The relation is in 2NF.
2. Non-prime attributes do not depend improperly on candidate keys through transitive dependencies.

**Problem eliminated:** Inappropriate transitive dependencies.

Example:

```
EmployeeID → DepartmentID
DepartmentID → DepartmentName
```

Store department details in a separate Department table to avoid repeating the department name for every employee.

### Normalization memory aid

- **1NF:** One value per cell.
- **2NF:** Depend on the whole candidate key.
- **3NF:** Avoid inappropriate transitive dependencies.

Each normal form builds on the preceding one.

---

# 6\. Data Modeling Schemas

Traditional data schemas include:

- Flat
- Hierarchical
- Network
- Relational
- Star
- Snowflake

Schema selection occurs during logical design, once entities and relationships are understood. It depends on data structure, business requirements, query/report complexity, performance, scalability, and the target DBMS.

## A. Flat model

- Stores data in a single table or file, such as CSV or TXT.
- Rows represent records; columns represent attributes.
- Records follow a uniform format.
- Has no built-in relational structure for representing relationships between records.

**Advantages:** Simple and easy to use.

**Limitations:** Poor support for complex relationships, data integrity, and large-scale management.

**Memory aid:** Flat = one file/table.

## B. Hierarchical model

- Organizes records in a tree.
- Each child has one parent; a parent may have multiple children.
- Naturally supports one-to-many (1:N) relationships.
- Efficient when parent-child relationships and access patterns are predictable.
- Suitable for nested data.
- Historically one of the earliest database models; less flexible than models that support more complex relationships.

**Memory aid:** Tree → one parent per child → 1:N.

## C. Network model

- Records are connected through a network of links.
- A record can have multiple parent relationships.
- Supports more complex relationships, including many-to-many structures.

**Memory aid:** Network = multiple connections and potentially multiple parents.

## D. Relational model

- Organizes data into **relations (tables)**.
- Tables consist of **tuples (rows)** and **attributes (columns)**.
- Represents relationships using keys and constraints.
- Uses SQL to query and manipulate data.
- Supports normalization and transactions.

Example:

**Student**

| StudentID (PK) | Name |
| --- | --- |
| 1 | Ana |
| 2 | Ben |

**Enrollment**

| StudentID (FK) | CourseID (FK) |
| --- | --- |
| 1 | 101 |
| 2 | 102 |

The relational model is widely used in SQL database systems.

## E. Star schema

- A central **fact table** connects directly to dimension tables.
- Common in data warehouses and analytical workloads.
- Relatively simple structure, often requiring fewer joins.

**Memory aid:** Star = fact in the center, dimensions around it.

## F. Snowflake schema

- A central fact table connects to dimension structures.
- Dimension tables are further normalized into related tables.
- Reduces dimension-data redundancy but introduces additional joins and structural complexity.

**Memory aid:** Snowflake = normalized, branching dimensions.

### Star vs. snowflake

| Star | Snowflake |
| --- | --- |
| Simpler dimensions | More normalized dimensions |
| Fewer joins in typical queries | Potentially more joins |
| More dimension redundancy may occur | Less dimension redundancy |
| Simpler to understand | More complex structure |

---

# 7\. Relational Model: Benefits and Limitations

## Benefits

- **Simplicity:** Tables and SQL provide a consistent way to organize and manipulate data.
- **User-friendliness:** Queries enable flexible data retrieval.
- **Data accuracy:** Constraints and keys help enforce valid data.
- **Reduced redundancy:** Normalization minimizes unnecessary duplication.
- **Data integrity:** PKs, FKs, and other constraints enforce rules.
- **Reliability:** Transactions can provide ACID guarantees.

## Limitations and challenges

- **Maintenance:** Large, complex systems may require specialist administration.
- **Cost:** Infrastructure, software, and skilled personnel can be expensive.
- **Scalability:** Vertical scaling may become costly or limited under heavy workloads; horizontal scaling is possible but can add complexity.
- **Structural complexity:** Complex relationships may require many tables and joins.
- **Performance:** Large datasets and poorly optimized queries can increase latency.

**Important qualification:** These are potential limitations, not inevitable outcomes. Well-designed relational databases can support very large datasets and demanding workloads.

---

# 8\. ACID Transactions

ACID describes four properties that help relational database transactions remain reliable.

| Property | Definition | Exam keyword |
| --- | --- | --- |
| **A — Atomicity** | The transaction completes entirely or not at all. | All or nothing |
| **C — Consistency** | A transaction preserves database rules and validity. | Valid state |
| **I — Isolation** | Concurrent transactions do not improperly interfere. | Independent execution |
| **D — Durability** | Committed changes persist despite subsequent failures. | Permanent changes |

### Typical exam questions

- **Which property means all-or-nothing?** Atomicity.
- **Which property preserves valid database states?** Consistency.
- **Which property addresses interference between concurrent transactions?** Isolation.
- **Which property ensures committed changes persist?** Durability.

**Memory aid:** A = All or nothing; C = Correct state; I = Independent transactions; D = Durable changes.

---

# 9\. High-Yield Exam Rules

1. A data model represents data, structure, and relationships.
2. Conceptual = business view; logical = structured view; physical = implementation.
3. An entity is an object/concept; an attribute describes it; a relationship connects entities.
4. A PK uniquely identifies a row and cannot be null.
5. An FK references an eligible key and helps maintain referential integrity.
6. A 1:M relationship normally places the FK on the many side.
7. An M:N relationship requires a junction table in a relational implementation.
8. 1NF eliminates non-atomic and repeating data.
9. 2NF eliminates partial dependencies on a proper subset of a candidate key.
10. 3NF addresses inappropriate transitive dependencies.
11. Flat = one table/file; hierarchical = tree; network = complex connections.
12. Relational = tables, rows, columns, SQL, keys, and constraints.
13. Star = central fact table with directly connected dimensions.
14. Snowflake = more normalized, branching dimension tables.
15. ACID = Atomicity, Consistency, Isolation, Durability.
16. Schema choice depends on data, relationships, query needs, performance, and scalability.

---

# 10\. Practice Exam — 15 Questions

## Multiple-choice questions

**Q1.** Which model is most appropriate for discussing business requirements with nontechnical stakeholders?

A. Physical\
B. Conceptual\
C. Logical\
D. Network

**Q2.** Which model specifies database-specific data types and indexes?

A. Conceptual\
B. Logical\
C. Physical\
D. Hierarchical

**Q3.** In a hierarchical model, each child typically has:

A. Multiple parents\
B. Exactly one parent\
C. No parent\
D. A mandatory junction table

**Q4.** What is a tuple in the relational model?

A. A column\
B. A database file\
C. A row\
D. An index

**Q5.** A customer can place many orders, but each order belongs to one customer. What is the cardinality?

A. 1:1\
B. 1:M\
C. M:N\
D. No relationship

**Q6.** Students can enroll in multiple courses, and each course can contain multiple students. What is the correct relational design?

A. One flat file only\
B. A hierarchical model only\
C. A junction table connecting Student and Course\
D. Remove the relationship

**Q7.** Which key uniquely identifies a row?

A. Foreign key\
B. Primary key\
C. Attribute\
D. Tuple

**Q8.** What is the main purpose of normalization?

A. Increase duplication\
B. Reduce redundancy and update anomalies\
C. Remove all relationships\
D. Replace SQL

**Q9.** Which normal form requires atomic attribute values and no repeating groups?

A. 1NF\
B. 2NF\
C. 3NF\
D. ACID

**Q10.** Which normal form addresses partial dependencies on a proper subset of a candidate key?

A. 1NF\
B. 2NF\
C. 3NF\
D. Physical normalization only

**Q11.** Which normal form addresses inappropriate transitive dependencies?

A. 1NF\
B. 2NF\
C. 3NF\
D. None

**Q12.** Which schema uses a central fact table connected directly to dimensions?

A. Flat\
B. Hierarchical\
C. Star\
D. Network

**Q13.** Which ACID property means all-or-nothing?

A. Consistency\
B. Isolation\
C. Durability\
D. Atomicity

**Q14.** Which ACID property ensures committed changes persist?

A. Durability\
B. Atomicity\
C. Isolation\
D. Consistency

**Q15.** Which ACID property addresses interference between concurrent transactions?

A. Atomicity\
B. Consistency\
C. Isolation\
D. Durability

## Answer key

| Question | Answer | Reason |
| --- | --- | --- |
| 1 | B | Conceptual modeling represents business concepts. |
| 2 | C | Physical modeling specifies implementation details. |
| 3 | B | A hierarchical child has one parent. |
| 4 | C | A tuple is a row. |
| 5 | B | One customer can have many orders. |
| 6 | C | A junction table resolves an M:N relationship. |
| 7 | B | A PK uniquely identifies each row. |
| 8 | B | Normalization reduces redundancy and anomalies. |
| 9 | A | 1NF requires atomic values and no repeating groups. |
| 10 | B | 2NF addresses partial dependencies. |
| 11 | C | 3NF addresses inappropriate transitive dependencies. |
| 12 | C | A star schema centers on a fact table. |
| 13 | D | Atomicity means all or nothing. |
| 14 | A | Durability preserves committed changes. |
| 15 | C | Isolation addresses concurrent transaction interference. |

---

# 11\. Final 30-Second Memory Sheet

- **Data model:** Abstract representation of data and relationships.
- **Conceptual → Logical → Physical:** What → Structure → Implementation.
- **Entity:** Object/concept; **Attribute:** Property; **Relationship:** Association.
- **PK:** Uniquely identifies a row.
- **FK:** References another eligible key.
- **1:1:** One-to-one.
- **1:M:** FK normally on the many side.
- **M:N:** Junction table.
- **1NF:** Atomic values.
- **2NF:** No partial dependencies on part of a candidate key.
- **3NF:** No inappropriate transitive dependencies.
- **Flat:** One file/table.
- **Hierarchical:** Tree; one parent per child; 1:N.
- **Network:** Multiple connections and complex relationships.
- **Relational:** Tables, rows, columns, SQL, PKs/FKs.
- **Star:** Fact table + directly connected dimensions.
- **Snowflake:** More normalized, branching dimensions.
- **ACID:** Atomicity = all or nothing; Consistency = valid state; Isolation = no improper interference; Durability = committed changes persist.
- **Schema selection:** Based on data structure, business needs, queries, performance, and scalability.

## Final takeaway

Effective data modeling clarifies requirements and relationships before implementation. Correct keys and normalization improve integrity, while choosing an appropriate schema helps balance simplicity, query performance, maintainability, and scalability.
