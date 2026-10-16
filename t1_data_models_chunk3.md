# Chunk 3 — Exam Compression: Data Modeling & Schemas

## 1. Data modeling schemas — overview

Data modeling organizes data and defines how it is stored, managed, and related.

Traditional schemas:

- Flat
- Hierarchical
- Network
- Relational
- Star
- Snowflake

Schema selection occurs during logical data modeling, after entities and relationships are defined. It depends on data structure, query/report complexity, performance, DBMS, and business needs.

## 2. Main data models

### A. Flat model

- Stores data in a single table or file (e.g., CSV, TXT).
- Rows = records; columns = attributes.
- Records follow a uniform format.
- No built-in structure for representing relationships between records.
- Advantage: simple and easy to use.
- Limitation: poor support for complex relationships and large-scale data management.

### B. Hierarchical model

- Organizes data in a tree structure.
- Each child has one parent, while a parent can have multiple children.
- Supports one-to-many (1:N) relationships.
- Efficient when parent–child relationships and data access patterns are predictable.
- Well suited to nested data.
- Historically one of the earliest database models; now less common than more flexible approaches.

Remember: Tree → one parent per child → 1:N.

### C. Relational model

- Organizes data into relations (tables).
- A relation contains tuples (rows) and attributes (columns).
- Tables represent both data and relationships between data.
- Uses SQL for querying and manipulating data.
- Relationships are typically represented through keys.

Example:

| StudentID (PK) | Name |
| --- | --- |
| 1 | Ana |
| 2 | Ben |

| CourseID (PK) | StudentID (FK) |
| --- | --- |
| 101 | 1 |
| 102 | 2 |

- PK (Primary Key): uniquely identifies a record.
- FK (Foreign Key): references a key in another table, helping maintain referential integrity.

## 3. Relational model — benefits

- Simplicity: structured tables and SQL make data manipulation straightforward.
- User-friendliness: users can retrieve data through queries.
- Data accuracy and consistency: constraints and relationships help maintain valid data.
- Reduced duplication: normalization reduces redundant data.
- Data integrity: keys and constraints enforce rules.
- Reliability: supports transactions through ACID properties.

### ACID — memorize all four

| Property | Meaning | Exam keyword |
| --- | --- | --- |
| A — Atomicity | Transaction completes entirely or not at all. | All or nothing |
| C — Consistency | Transaction preserves database rules and validity. | Valid state |
| I — Isolation | Concurrent transactions do not improperly interfere. | Independent execution |
| D — Durability | Committed changes persist. | Permanent changes |

Typical exam question: What guarantees that a transaction is fully completed or fully rolled back?

Answer: Atomicity.

## 4. Relational model — limitations

- Maintenance challenges: administration becomes more demanding as systems and data volumes grow.
- Cost: infrastructure, software, and specialist personnel may be expensive.
- Scalability: scaling vertically can become costly or limited under heavy workloads; horizontal scaling is possible but may be more complex.
- Structural complexity: complex object relationships may require many tables and joins.
- Performance degradation: large datasets and complex queries can increase latency if indexing and query design are inadequate.

Exam tip: These are potential limitations, not inevitable outcomes. Well-designed relational databases can handle very large datasets and workloads.

## 5. Other schemas: star, snowflake, and network

Know the defining structure of each:

| Schema | Key characteristic |
| --- | --- |
| Network | Records can have multiple relationships/parents; supports many-to-many structures. |
| Star | Central fact table connected directly to dimension tables; common in data warehouses. |
| Snowflake | Normalized dimension tables branching into related tables; more complex than a star schema. |

Star vs. snowflake: Star = simpler, fewer joins. Snowflake = more normalized dimensions, potentially more joins.

## 6. Core data-modeling concepts

- Data model: abstract representation of data, structure, and relationships.
- Conceptual model: high-level business view of entities and relationships.
- Logical model: defines entities, attributes, keys, and relationships independently of a specific physical implementation.
- Physical model: specifies actual database implementation, such as tables, data types, indexes, and storage.
- Entity: object or concept about which data is stored.
- Attribute: property of an entity.
- Relationship: association between entities.
- Normalization: organizes tables to reduce redundancy and update anomalies.
- PK/FK: help identify records and maintain referential integrity.

## 7. High-yield exam revision

## 10 practice MCQs

0/10 answered

1. Which model organizes records into a tree?

A. Flat

B. Hierarchical

C. Relational

D. Star

2. In a hierarchical model, each child typically has:

A. Multiple parents

B. Exactly one parent

C. No parent

D. A foreign key only

3. What is a tuple in the relational model?

A. A column

B. A database file

C. A row

D. A relationship constraint

4. Which ACID property means all-or-nothing?

A. Consistency

B. Isolation

C. Durability

D. Atomicity

5. Which ACID property ensures committed changes persist?

A. Durability

B. Atomicity

C. Isolation

D. Consistency

6. What is the main purpose of normalization?

A. Increase duplication

B. Reduce redundancy

C. Remove all relationships

D. Replace SQL

7. Which schema has a central fact table connected to dimensions?

A. Flat

B. Hierarchical

C. Star

D. Network

8. Which key uniquely identifies a row?

A. Foreign key

B. Primary key

C. Attribute only

D. Tuple

9. Which model is especially suitable for predictable parent–child access?

A. Hierarchical

B. Star

C. Flat

D. Snowflake

10. What does isolation address?

A. Permanent storage

B. Valid database rules

C. Interference between concurrent transactions

D. Unique identifiers

 Check answers

## Final 30-second memory sheet

- Flat: one file/table; simple structure.
- Hierarchical: tree; one parent per child; 1:N.
- Relational: tables, rows, columns, SQL, PKs and FKs.
- Network: supports complex connections and multiple parent relationships.
- Star: fact table + directly connected dimensions.
- Snowflake: normalized dimensions branching into related tables.
- Normalization: reduce redundancy.
- ACID: Atomicity = all or nothing; Consistency = valid state; Isolation = no improper interference; Durability = permanent committed changes.
- Modeling levels: Conceptual → Logical → Physical.
- Schema choice: based on data, relationships, query needs, performance, and scalability.
