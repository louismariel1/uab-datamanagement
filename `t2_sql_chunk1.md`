# Exam Compression — T3. SQL (Chunk 1)

Data Management 2026–27 · Condensed exam revision notes

## 1. Recap: Data Models

- Data models: Abstract and organize information.
- Three abstraction levels: Conceptual, Logical, Physical.
- Core components: Entities, attributes, relationships.
- Primary Key (PK): Uniquely identifies each record.
- Foreign Key (FK): Links tables and helps maintain referential integrity.
- Normalization: Reduces data redundancy and improves consistency.
- Different schemas suit different business needs.

## 2. Evolution of Databases

| Period | Database type | Key characteristics |
| --- | --- | --- |
| 1960s | Navigational databases | Pointers linked records; physical disk addresses; batch processing. Example: IBM IMS. |
| 1970s | Relational databases | Tables, keys, relationships, searchable data, SQL and query optimization. |
| 1980s | Desktop databases | Databases became accessible to individual users. Example: dBase. |
| 1990s | Object-oriented databases | Data organized around objects. |
| 2000s | NoSQL databases | Alternative data models for diverse data and application needs. |

Key evolution: Navigational databases relied on pointers and physical addresses; relational databases introduced table-based organization and flexible querying.

## 3. Database: Definition and Examples

A database is a system of tools for structuring, storing, retrieving, organizing and protecting information.

Examples:

- Library records and image collections.
- Customer data, invoices, consumption and preferences.
- Formula 1 telemetry.
- Astronomical objects: stars, planets, satellites and debris.
- Internet user profiles and online-game status.

### Types of databases

Different applications require different database designs and performance characteristics.

| Type | Typical purpose or characteristic |
| --- | --- |
| Relational | Structured data organized into related tables. |
| NoSQL | Flexible data models, such as documents or other non-tabular structures. |
| Numerical series / time-series | Numerical measurements collected over time, e.g. telemetry. |
| In-memory | Data held primarily in memory for fast access. |

Exam principle: Choose the database according to the application's requirements, such as fast writes, specific searches, rapid reads, fault tolerance, floating-point data or concurrent writes.

## 4. Relational Databases and SQL

- A relational database organizes data into tables and manages relationships between them.
- SQL (Structured Query Language) is the standard language used to access and manipulate relational databases.
- SQL syntax may vary slightly between database implementations.
- SQL uses queries to ask questions of the database and retrieve specific information.

### Main SQL capabilities

- Create tables and define primary keys.
- Insert, retrieve, update and delete data.
- Filter, group and aggregate results.
- Combine data from multiple tables using `JOIN`.
- Use indexes, including B-tree indexes, to improve data retrieval.

## 5. Database System Architecture

Application

Written in Java, C#, C/C++ or another language

Database driver / SQL interface

Connects the application to the DBMS

DBMS — Database Management System

Processes queries, updates and database operations

Stored data

Persisted on storage devices

The DBMS is the software engine that manages access to stored data. It is separated from the application and can be reused by different applications.

### DBMS responsibilities

1. Database creation: Manages low-level file and storage operations.
2. Query and update facilities: Supports retrieval and modification of data.
3. Multitasking / concurrency: Allows multiple applications to access data.
4. Audit trail: Records database changes.
5. Security: Manages users and permissions.
6. Referential integrity: Maintains valid relationships between tables.

### Languages provided by a DBMS

- DDL — Data Definition Language: Defines database structures.
- DQL — Data Query Language: Retrieves data.
- DML — Data Manipulation Language: Inserts, updates and deletes data.
- DCL — Data Control Language: Controls permissions and access.

## 6. ACID Model

ACID is a set of transaction properties that supports database reliability, consistency and integrity.

A transaction is a group of operations treated as one logical unit.

# A

Atomicity — All or nothing

All operations in a transaction succeed, or the transaction fails as a whole.

# C

Consistency — Valid states

A transaction preserves database rules and constraints, leaving the data valid.

# I

Isolation — Concurrent transactions

Concurrent transactions behave as if executed separately in a valid serial order.

# D

Durability — Permanent commit

Once committed, changes survive subsequent failures or errors.

Memory aid: Atomicity = all or nothing; Consistency = valid data; Isolation = independent execution; Durability = committed changes persist.

## 7. Types of SQL Commands

There are four main categories in this chapter.

| Category | Full name | Purpose | Commands |
| --- | --- | --- | --- |
| DDL | Data Definition Language | Defines or changes database structure | `CREATE`, `DROP`, `RENAME` |
| DQL | Data Query Language | Retrieves data | `SELECT` |
| DML | Data Manipulation Language | Changes table data | `INSERT`, `UPDATE`, `DELETE` |
| DCL | Data Control Language | Manages access permissions | `GRANT`, `REVOKE` |

### Important distinctions

- `CREATE TABLE` → creates a table.
- `DROP TABLE` → removes a table.
- `SELECT` → retrieves data.
- `INSERT` → adds records.
- `UPDATE` → modifies existing records.
- `DELETE` → removes records.
- `GRANT` → grants privileges.
- `REVOKE` → withdraws privileges.

Common exam trap: `DELETE` removes rows, whereas `DROP TABLE` removes the table structure itself as well as its data.

## 8. Tables in SQL

A relational table consists of the following elements:

| Term | Meaning | Example |
| --- | --- | --- |
| Table name | Identifies the table | `Students` |
| Attribute / column | Defines a property of the data | `Surname`, `Age`, `City` |
| Tuple / row / record | Represents one entry | `Joan, Martí, 22, Barcelona` |
| Primary key | Uniquely identifies a row | A unique student ID |
| Foreign key | References a key in another table | A student record referencing a course |

Example:

Students

| SName | Surname | Age | City |
| --- | --- | --- | --- |
| Joan | Martí | 22 | Barcelona |
| Maria | Andreu | 24 | Lleida |
| Paula | Pujol | 22 | Manresa |
| Santi | Martínez | 25 | Cerdanyola |

Remember:

- Columns describe what kind of information is stored.
- Rows describe individual records.
- A table is identified by its name.
- A primary key must uniquely identify each record; a foreign key maintains a relationship with another table.

## 9. High-priority exam facts

Learn these distinctions precisely:

1. Database vs. DBMS: A database stores organized data; a DBMS is the software that manages it.
2. SQL vs. DBMS: SQL is the query language; the DBMS executes and manages database operations.
3. DDL vs. DML: DDL changes structure; DML changes the data stored in that structure.
4. DQL vs. DCL: DQL retrieves data; DCL manages privileges.
5. Primary key vs. foreign key: A PK uniquely identifies a record; an FK establishes a reference to a key in another table.
6. Atomicity vs. durability: Atomicity guarantees all-or-nothing execution; durability guarantees persistence after commit.
7. Isolation vs. consistency: Isolation concerns concurrent transactions; consistency concerns valid database states.
8. Navigational vs. relational: Navigational databases follow pointers; relational databases use tables, keys and queries.

## 10. Quick self-test

Try answering before revealing each solution.

1. Which SQL category contains CREATE and DROP?

DQL

DDL

DML

DCL

2. Which ACID property ensures committed changes survive failures?

Atomicity

Consistency

Isolation

Durability

3. What is the DBMS?

A table column

A query language

Software that manages a database

A primary key

4. Which command retrieves data?

INSERT

SELECT

REVOKE

DROP

5. What is a tuple in a relational table?

A column

A table name

A row or record

A permission

6. Which property concerns concurrent transactions?

Isolation

Durability

Atomicity

Consistency

7. What distinguishes DELETE from DROP TABLE?

They are identical

DELETE removes rows; DROP removes the table

DELETE grants access

DROP only retrieves rows

0 of 7 answered

 Reset quiz

Chunk 1 complete. This compression covers the introduction to SQL, database evolution, database types, DBMS architecture, ACID, SQL command categories and table terminology. Send Chunk 2 when ready; I’ll compress it in the same format so we can merge the material into one consistent chapter revision sheet at the end.
