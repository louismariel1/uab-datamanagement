# Data Storage Models — Exam Compression (Chunk 2)

## 1\. Relationships: One-to-One (1:1)

- Each **User Account** is associated with exactly one **Customer Profile**.
- Each Customer Profile belongs to exactly one User Account.

## 2\. Logical Data Model: Keys and Integrity

### Primary Key (PK)

- Attribute or combination of attributes that **uniquely identifies each record**.
- Prevents duplicate records and ambiguity.
- Must be unique and non-null.

### Foreign Key (FK)

- Attribute(s) in one table that reference a **primary key (or eligible candidate key) in another table**.
- Establishes relationships between tables and maintains referential integrity.

**Exam tip:** PK identifies a record; FK connects tables.

Together, PKs and FKs help preserve data consistency from the design stage through implementation.

## 3\. Database Normalization

**Definition:** Organizing data into related tables to reduce redundancy, prevent update anomalies, and improve consistency and integrity.

### Benefits

- Stores each piece of information only once where possible.
- Reduces repeated data (e.g., customer addresses).
- Simplifies updates and reduces errors.
- Improves maintainability and data integrity.

### Normal Forms: Must Memorize

| Normal form | Requirements | Main problem eliminated |
| --- | --- | --- |
| **1NF** | Every attribute contains atomic (indivisible) values; no repeating groups or multivalued attributes | Non-atomic and repeating data |
| **2NF** | In 1NF, and every non-prime attribute is fully functionally dependent on the whole candidate key | Partial dependencies |
| **3NF** | In 2NF, with no improper transitive dependencies of non-prime attributes on candidate keys | Transitive dependencies |

### First Normal Form (1NF)

A table is in 1NF when:

- Each field contains a single, atomic value.
- No lists, sets, arrays, or multiple values in one attribute.
- No repeating groups.

**Example:** Instead of storing `PhoneNumbers = 111, 222` in one cell, store individual phone numbers in separate rows or a related table.

### Second Normal Form (2NF)

A table is in 2NF when:

1. It satisfies 1NF.
2. Every non-prime attribute depends on the **entire candidate key**, not just part of a composite key.

- Eliminates **partial dependencies**.
- Particularly important when a key consists of multiple attributes.
- Simplifies updates and maintenance.

**Example:** Given a composite key `(StudentID, CourseID)`, if `StudentName` depends only on `StudentID`, move student information to a separate Student table.

### Third Normal Form (3NF)

A table is in 3NF when:

1. It satisfies 2NF.
2. Non-prime attributes do not depend transitively on candidate keys.

- Eliminates inappropriate **transitive dependencies**.
- Reduces duplication and update anomalies.
- Improves data integrity and maintainability.

**Example:** If `EmployeeID → DepartmentID` and `DepartmentID → DepartmentName`, store department details in a separate Department table.

### Normalization Memory Aid

- **1NF:** One value per cell.
- **2NF:** Depends on the whole key.
- **3NF:** Depends on nothing but the key (informal memory aid).

Remember: Each successive normal form builds on the previous one.

## 4\. Physical Data Model

**Definition:** The technology- and database-specific implementation of the data model. It specifies how data is physically stored and accessed.

### Key characteristics

- Final step in transforming a logical model into a working database.
- Includes implementation details needed to build the database.
- Uses database-specific concepts such as tables, columns, data types, indexes, and storage structures rather than only conceptual entities and attributes.
- Depends on the chosen database technology.

## 5\. Conceptual vs. Logical vs. Physical Models

| Model | Focus | Typical contents |
| --- | --- | --- |
| **Conceptual** | What data and business concepts exist? | High-level entities and relationships |
| **Logical** | How is data organized logically? | Entities/tables, attributes, keys, relationships, normalization |
| **Physical** | How will the database implement and store data? | Database-specific tables, columns, data types, indexes, constraints, storage details |

**Memory aid:**

- Conceptual = **What?**
- Logical = **How is the data structured?**
- Physical = **How is it implemented?**

## 6\. High-Yield Exam Questions

1. **What is a PK?**\
   A unique identifier for each record; it cannot contain duplicate or null values.
2. **What is an FK?**\
   An attribute that references a key in another (or the same) table to establish a relationship and maintain referential integrity.
3. **What is normalization?**\
   Structuring data into related tables to reduce redundancy and improve integrity.
4. **What does 1NF eliminate?**\
   Non-atomic values and repeating groups.
5. **What does 2NF eliminate?**\
   Partial dependencies on part of a composite candidate key.
6. **What does 3NF eliminate?**\
   Transitive dependencies of non-prime attributes on candidate keys.
7. **What is the difference between 2NF and 3NF?**\
   2NF addresses partial dependencies; 3NF addresses transitive dependencies.
8. **What is the physical data model?**\
   The database-specific implementation describing how data is stored and accessed.
9. **What is a 1:1 relationship?**\
   Each entity instance corresponds to exactly one instance of the other entity, in both directions.

## Last-Minute Revision

**PK → uniquely identifies.**\
**FK → references another key.**\
**Normalization → less redundancy, better integrity.**\
**1NF → atomic values.**\
**2NF → no partial dependency.**\
**3NF → no inappropriate transitive dependency.**\
**Conceptual → business view.**\
**Logical → structured data view.**\
**Physical → implementation view.**

I can provide the content in Markdown here, ready to save to GitHub as `t1........_chunk2.md`. I haven’t created a downloadable file in this response.
