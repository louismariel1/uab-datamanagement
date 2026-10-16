# T1 — Data Models (Chunk 1)

## Exam Compression — Data Management 2026–2027

**Topics covered:** Data models, abstraction layers, conceptual models, logical models, storage models, and relationship cardinalities.

## 1\. Data System Foundations

A successful data system balances three essential qualities:

- **Reliability:** The system operates correctly and recovers from failures.
- **Scalability:** The system handles increasing data volumes and workloads.
- **Maintainability:** The system can be modified, understood, and operated efficiently.

### Performance Metrics

- **Throughput:** Number of requests processed per unit of time (e.g., requests/second).
- **Latency:** Time required for an operation or request to complete.
- **Response time:** Total time between a request and the corresponding response.

Data-intensive applications must manage growing volumes of data, users, and complexity. They combine components such as databases, caches, storage systems, and processing frameworks.

## 2\. What Is a Data Model?

A **data model** is an abstract representation that organizes data and describes how users and applications interact with it.

Data models influence:

- Software design and development.
- System performance.
- How information is organized and represented.
- Communication between technical teams and business stakeholders.

### Layered Data Models

Data-intensive applications use layers of abstraction to simplify system design.

- Each layer provides a clean model to the layer above it.
- Each layer hides unnecessary complexity from lower layers.
- Different teams can work on separate layers more effectively.
- Each layer describes how its information is represented by the next lower layer.

**Key principle:** Abstraction reduces complexity by hiding implementation details.

## 3\. Data Storage Models: Three Levels

The database design process typically progresses through three levels:

### A. Conceptual Model

**Purpose:** Describe what information the system needs to represent.

- High-level, graphical representation of the information in a database.
- Focuses on business concepts and relationships.
- Used by business stakeholders, analysts, and data architects.
- Does not include implementation-specific details.
- Helps define system scope and requirements.

**Main elements:**

- **Entity:** A real-world object or concept about which data is stored (e.g., Customer, Car, Student).
- **Relationship:** An association between entities (e.g., Customer places Order).

### B. Logical Model

**Purpose:** Specify the data structure in greater detail without committing to a particular database platform.

- Expands the conceptual model.
- Defines entity attributes (properties describing entities).
- Specifies relationships and their structure.
- Supports business requirements, flexibility, and portability.
- Helps determine the appropriate database schema.

**Example:**

Conceptual entity: `Customer`

Logical representation: `Customer(CustomerID, Name, Email)`

Here, `CustomerID` can serve as the primary key.

### C. Physical Model

**Purpose:** Describe how data is actually implemented and stored in a particular database system.

Typically includes:

- Database-specific data types.
- Tables, columns, and indexes.
- Storage structures and implementation details.
- Constraints and performance-related choices.

**Example:** Choosing `VARCHAR(100)` for a customer's name and creating an index on `CustomerID` in a specific DBMS.

### Comparison Table

| Feature | Conceptual | Logical | Physical |
| --- | --- | --- | --- |
| Main question | What data is needed? | How is data structured? | How is data implemented? |
| Level | High-level | Detailed structure | Implementation-specific |
| Attributes | Usually not detailed | Explicitly defined | Defined with data types |
| DBMS-specific | No | Generally no | Yes |
| Main audience | Business stakeholders | Analysts and architects | Database developers/administrators |

**Exam memory aid:** Conceptual = what; Logical = structure; Physical = implementation.

## 4\. Relationships and Cardinalities

Relationships describe associations between entities. Three common cardinalities are:

### A. One-to-One (1:1)

One instance of entity A is associated with at most one instance of entity B, and vice versa.

**Example:** One person has one passport, assuming the business rules permit only one passport per person.

### B. One-to-Many (1:M)

One instance of entity A can be associated with multiple instances of entity B, while each B instance belongs to at most one A instance.

**Example:** Customer → Orders.

- One customer can place many orders.
- Each order is associated with one customer.

**Relational implementation:** Place the foreign key of the one-side entity in the many-side table.

```
Customer(CustomerID PK, Name)
Orders(OrderID PK, CustomerID FK)
```

### C. Many-to-Many (M:N)

Multiple instances of entity A can be associated with multiple instances of entity B.

**Example:** Products ↔ Orders.

- One order can contain multiple products.
- One product can appear in multiple orders.

**Relational implementation:** Create a junction table containing foreign keys referencing both entities.

```
Product(ProductID PK, Name)
Orders(OrderID PK, OrderDate)

OrderItem(
    OrderID PK, FK,
    ProductID PK, FK,
    Quantity
)
```

The composite primary key `(OrderID, ProductID)` prevents the same product from appearing more than once per order in this simplified model. If repeated lines for the same product are allowed, use an additional line identifier.

## 5\. Essential Exam Rules

1. An **entity** represents a real-world object or concept.
2. An **attribute** describes an entity.
3. A **relationship** connects entities.
4. A **conceptual model** describes business concepts and their relationships.
5. A **logical model** defines attributes and data structure independently of a particular DBMS.
6. A **physical model** specifies implementation details.
7. A **1:M relationship** is normally implemented using a foreign key on the many side.
8. An **M:N relationship** is implemented using a junction table in a relational database.
9. A **primary key (PK)** uniquely identifies each row.
10. A **foreign key (FK)** references a key in another table, or a candidate key in the same table.

## 6\. Quick Self-Test

**Q1.** Which model is most appropriate for discussing business requirements with nontechnical stakeholders?

**Answer:** Conceptual model.

**Q2.** Which model defines attributes while remaining independent of a particular DBMS?

**Answer:** Logical model.

**Q3.** Where would you specify database-specific column types and indexes?

**Answer:** Physical model.

**Q4.** A customer can place many orders, but each order belongs to one customer. What is the cardinality?

**Answer:** One-to-many (1:M).

**Q5.** Students can enroll in multiple courses, and courses can have multiple students. How should this be represented relationally?

**Answer:** A many-to-many relationship resolved through an Enrollment junction table.

**Q6.** Why are data models important?

**Answer:** They clarify requirements, structure information, improve communication, and influence software design and performance.

---

**Chunk 1 takeaway:** Understand the three modeling levels, the distinction between entities, attributes, and relationships, and how 1:1, 1:M, and M:N relationships translate into relational structures.

When you're ready, paste Chunk 2. I'll continue the exam compression and suggest the corresponding `t2_data_models_chunk2.md` filename.
