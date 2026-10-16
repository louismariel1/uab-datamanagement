# EXAM COMPRESSION — T2. SQL

**Data Management 2026–27 · Complete Condensed Exam Revision Notes**

## 1\. Database Fundamentals

### 1.1 Data models

Data models abstract and organize information at three levels:

- **Conceptual:** High-level view of entities and relationships.
- **Logical:** Defines tables, attributes, keys and relationships.
- **Physical:** Describes how data is stored and accessed.

Core concepts:

- **Entity:** An object or concept represented in a database.
- **Attribute:** A property of an entity.
- **Relationship:** An association between entities.
- **Primary Key (PK):** Uniquely identifies each record.
- **Foreign Key (FK):** References a key in another table to establish a relationship and maintain referential integrity.
- **Normalization:** Reduces unnecessary data redundancy and improves consistency.
- **ER diagram:** Visually represents entities, attributes and relationships before implementation.

ER models can be translated into relational tables.

### 1.2 Evolution of databases

| Period | Database type | Main characteristics |
| --- | --- | --- |
| 1960s | Navigational | Pointers, physical addresses and batch processing; e.g., IBM IMS |
| 1970s | Relational | Tables, keys, relationships, SQL and flexible querying |
| 1980s | Desktop | Databases became accessible to individual users; e.g., dBase |
| 1990s | Object-oriented | Data organized around objects |
| 2000s | NoSQL | Alternative data models for diverse application requirements |

**Key distinction:** Navigational databases rely on pointers and record-access paths; relational databases organize data into tables and support flexible queries.

### 1.3 What is a database?

A database stores organized information so it can be structured, retrieved, updated and protected.

Examples include library records, customer information, invoices, Formula 1 telemetry, astronomical observations, internet user profiles and online-game status.

| Database type | Main characteristic |
| --- | --- |
| Relational | Structured data organized into related tables |
| NoSQL | Flexible models, such as documents or key-value data |
| Time-series | Measurements collected over time |
| In-memory | Data held primarily in memory for fast access |

**Exam principle:** Select a database based on requirements such as read/write speed, query patterns, data structure, concurrency and fault tolerance.

## 2\. Relational Databases, SQL and DBMS

### 2.1 Relational databases

A relational database organizes data into tables and manages relationships between them.

| Term | Meaning |
| --- | --- |
| Table | Collection of rows and columns |
| Column / attribute | Property of the data |
| Row / tuple / record | One individual entry |
| Schema | Table definition, including its columns and data types |
| Primary key | Uniquely identifies each row |
| Foreign key | References a key in another table |

Example schema:

`Students(id, name, age, city)`

A primary key must be unique and cannot be `NULL`. It can contain one column or multiple columns, forming a **composite primary key**.

### 2.2 SQL

**SQL = Structured Query Language.**

SQL is the standard language used to define, retrieve and manipulate data in relational databases. Its syntax can vary slightly between database management systems (DBMSs).

Main capabilities:

- Create tables and define keys.
- Insert, retrieve, update and delete records.
- Filter, group and aggregate results.
- Combine related tables using `JOIN`.
- Define indexes to improve data retrieval.

### 2.3 Database vs. DBMS vs. SQL

- **Database:** The organized data.
- **DBMS (Database Management System):** Software that manages storage, queries, transactions, security and access to the database.
- **SQL:** The language used to communicate database operations to the DBMS.

Typical architecture:

`Application → Database driver / SQL interface → DBMS → Stored data`

Applications may be written in Java, C#, C/C++ or other programming languages. Multiple applications can use the same DBMS.

### 2.4 DBMS responsibilities

1. Database creation and storage management.
2. Query processing and data modification.
3. Concurrency management for multiple users and applications.
4. Audit trails and change records.
5. Security, users and permissions.
6. Referential integrity and constraint enforcement.

### 2.5 SQL command categories

| Category | Full name | Purpose | Main commands |
| --- | --- | --- | --- |
| DDL | Data Definition Language | Defines or changes structures | `CREATE`, `DROP`, `RENAME` |
| DQL | Data Query Language | Retrieves data | `SELECT` |
| DML | Data Manipulation Language | Changes stored data | `INSERT`, `UPDATE`, `DELETE` |
| DCL | Data Control Language | Controls access permissions | `GRANT`, `REVOKE` |

**Important:** Transaction commands such as `COMMIT` and `ROLLBACK` are commonly classified as TCL (Transaction Control Language).

**Exam trap:**

- `DELETE` removes rows.
- `DROP TABLE` removes the table structure and its data.
- `CREATE TABLE` creates the structure, not the records.

## 3\. ACID Properties

A transaction is a group of operations treated as one logical unit.

| Property | Meaning | Memory aid |
| --- | --- | --- |
| Atomicity | All operations succeed, or the transaction fails as a whole | All or nothing |
| Consistency | Database rules and constraints remain satisfied | Valid data |
| Isolation | Concurrent transactions behave as if executed separately in a valid serial order | Independent execution |
| Durability | Committed changes survive subsequent failures | Permanent commit |

**Common exam distinctions:**

- Atomicity ≠ durability: all-or-nothing execution vs. persistence after commit.
- Isolation ≠ consistency: concurrent execution vs. valid database states.

## 4\. SQL Data Types and Table Creation

### 4.1 Common data types

| Category | Examples | Purpose |
| --- | --- | --- |
| Numeric | `INT`, `DECIMAL`, `FLOAT` | Numbers |
| String | `CHAR`, `VARCHAR`, `TEXT` | Text |
| Date/time | `DATE`, `TIME`, `DATETIME` | Dates and times |

Exact supported types depend on the DBMS.

### 4.2 Creating a table

```
CREATE TABLE Students (
    id INT,
    name VARCHAR(50),
    age INT
);
```

`CREATE TABLE` defines the table structure. It does not insert records.

### 4.3 Defining a primary key

```
CREATE TABLE Students (
    id INT,
    name VARCHAR(50),
    age INT,
    PRIMARY KEY (id)
);
```

A primary key must be unique and non-null. It may consist of one column or several columns.

### 4.4 Auto-increment

Example in MySQL:

```
CREATE TABLE Students (
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(50),
    PRIMARY KEY (id)
);
```

- Generates identifiers automatically for new rows.
- Useful for primary keys.
- `AUTO_INCREMENT` is MySQL-specific; other systems may use `IDENTITY` or sequences.

## 5\. CRUD Operations

**CRUD = Create, Read, Update, Delete.**

| Operation | SQL command | Purpose |
| --- | --- | --- |
| Create records | `INSERT` | Adds rows |
| Read | `SELECT` | Retrieves data |
| Update | `UPDATE` | Modifies existing rows |
| Delete | `DELETE` | Removes rows |

### 5.1 INSERT — add records

```
INSERT INTO Students (id, name, age)
VALUES (1, 'Joan', 20);
```

Prefer specifying the column names because this explicitly maps values to columns.

Without a column list:

```
INSERT INTO Students
VALUES (1, 'Joan', 20);
```

Values must match the table's column order and satisfy its constraints.

For an auto-generated ID:

```
INSERT INTO Students (name, age)
VALUES ('Joan', 20);
```

### 5.2 SELECT — retrieve data

Retrieve all columns:

```
SELECT *
FROM Students;
```

Retrieve selected columns:

```
SELECT name, age
FROM Students;
```

Filter records:

```
SELECT *
FROM Students
WHERE age >= 18;
```

`SELECT *` retrieves all columns, while `WHERE` filters rows.

### 5.3 UPDATE — modify records

```
UPDATE Students
SET name = 'Manuel'
WHERE id = 1;
```

Update multiple columns:

```
UPDATE Students
SET name = 'Manuel', age = 21
WHERE id = 1;
```

- `UPDATE`: Specifies the table.
- `SET`: Specifies the new values.
- `WHERE`: Specifies which rows are modified.

**Critical exam trap:** Omitting `WHERE` updates every row.

### 5.4 DELETE — remove records

```
DELETE FROM Students
WHERE id = 1;
```

**Critical exam trap:** Omitting `WHERE` deletes every row in the table, but the table structure remains.

### 5.5 CREATE vs. INSERT

- `CREATE TABLE` creates the table structure.
- `INSERT INTO` creates individual records.

## 6\. SQL Filtering, Sorting and Aggregation

### 6.1 WHERE

Filters individual rows before grouping or aggregation.

```
SELECT *
FROM Movie
WHERE genre = 'drama';
```

Common comparison operators include `=`, `>`, `<`, `>=`, `<=` and `<>`.

Pattern matching uses `LIKE`:

```
SELECT *
FROM Movie
WHERE description LIKE '%drama%';
```

- `%` matches zero or more characters.
- `_` matches a single character in standard SQL `LIKE` patterns.
- Exact equality is preferable when searching for an exact value.
- If genre is stored in its own column, filter by `genre` rather than searching the description.

### 6.2 ORDER BY

Sorts the output:

```
SELECT *
FROM Students
ORDER BY age ASC;
```

- `ASC`: Ascending order; commonly the default.
- `DESC`: Descending order.

### 6.3 Aggregate functions

| Function | Purpose |
| --- | --- |
| `COUNT(*)` | Counts rows |
| `COUNT(column)` | Counts non-`NULL` values in a column |
| `SUM(column)` | Calculates a total |
| `AVG(column)` | Calculates an average |
| `MIN(column)` | Finds the minimum |
| `MAX(column)` | Finds the maximum |

### 6.4 GROUP BY

Groups rows with the same values, typically to calculate an aggregate for each group.

Example: Count students by city.

```
SELECT city, COUNT(*) AS student_count
FROM Students
GROUP BY city;
```

Example: Calculate average marks by city.

```
SELECT city, AVG(mark) AS average_mark
FROM Students
GROUP BY city;
```

Without `GROUP BY`, an aggregate generally calculates one result for all selected rows.

### 6.5 HAVING

Filters groups after grouping and aggregation.

```
SELECT city, COUNT(*) AS student_count
FROM Students
GROUP BY city
HAVING COUNT(*) > 2;
```

**Essential distinction:**

- `WHERE` filters individual rows before grouping.
- `HAVING` filters groups after aggregation.

### 6.6 SQL clause order

Typical query structure:

```
SELECT column_name, COUNT(*)
FROM table_name
WHERE condition
GROUP BY column_name
HAVING COUNT(*) > 2
ORDER BY column_name;
```

Memorize the roles:

- `SELECT`: Chooses output columns or expressions.
- `FROM`: Specifies the source table(s).
- `WHERE`: Filters rows.
- `GROUP BY`: Forms groups.
- `HAVING`: Filters groups.
- `ORDER BY`: Sorts results.

## 7\. SQL JOINs

A `JOIN` combines related rows from two or more tables using a matching condition, usually specified with `ON`.

### 7.1 JOIN types

| JOIN type | What it returns |
| --- | --- |
| `INNER JOIN` | Only rows with matches in both tables |
| `LEFT JOIN` | All rows from the left table and matching rows from the right |
| `RIGHT JOIN` | All rows from the right table and matching rows from the left |
| `FULL OUTER JOIN` | All rows from both tables, matched where possible |

For outer joins, unmatched columns from the missing side contain `NULL`.

### 7.2 INNER JOIN example

```
SELECT Products.ProductID,
       Products.ProductName,
       Categories.CategoryName
FROM Products
INNER JOIN Categories
ON Products.CategoryID = Categories.CategoryID;
```

The query combines product details with category names.

- `ON` specifies the matching condition.
- A primary key may be referenced by a foreign key in another table.
- Table aliases can make queries shorter and clearer.

### 7.3 LEFT JOIN example

```
SELECT Students.name, Enrolments.course_id
FROM Students
LEFT JOIN Enrolments
ON Students.id = Enrolments.student_id;
```

Every student appears, including students without an enrolment. For those students, `course_id` is `NULL`.

### 7.4 Choosing a JOIN

- Need only matching records? Use `INNER JOIN`.
- Need every record from the left table? Use `LEFT JOIN`.
- Need every record from the right table? Use `RIGHT JOIN`.
- Need every record from both tables? Use `FULL OUTER JOIN`.

**Exam trap:** A `JOIN` does not automatically match records by similar-looking column names. The `ON` condition defines how the records relate.

## 8\. SQL Practice: Movies and Ratings

Assume the database contains `Movie(id, title, description, genre)` and `Rating(movie_id, rating)`.

### Exercise 1: Find drama movies

```
SELECT *
FROM Movie
WHERE genre = 'drama';
```

### Exercise 2: Find movies with ratings greater than 5

```
SELECT Movie.title,
       Movie.description,
       Rating.rating
FROM Movie
JOIN Rating
ON Movie.id = Rating.movie_id
WHERE Rating.rating > 5;
```

This returns matching movie-rating records whose rating exceeds 5.

### Exercise 3: Calculate average ratings per movie

```
SELECT Movie.title,
       AVG(Rating.rating) AS average_rating
FROM Movie
JOIN Rating
ON Movie.id = Rating.movie_id
GROUP BY Movie.id, Movie.title;
```

- `AVG()` calculates the average.
- `GROUP BY` produces a separate average for each movie.
- Grouping by both the movie ID and title helps identify each movie correctly.
- `AS average_rating` names the calculated output column.

To calculate the average of all ratings greater than 5:

```
SELECT AVG(rating) AS average_rating
FROM Rating
WHERE rating > 5;
```

**Important:** Filtering individual ratings before averaging is different from filtering movies according to their average rating. To filter the calculated averages, use `HAVING`.

## 9\. Database Indexes

### 9.1 Definition and purpose

An **index** is a data structure that helps the DBMS locate records efficiently without scanning the entire table in many cases.

Think of a book index: it helps you find a relevant page without reading every page.

Indexes can improve:

- Equality searches and lookups.
- Range queries.
- Filtering with `WHERE`.
- Joins on indexed columns.
- Sorting when the index order suits the query.

### 9.2 Advantages and disadvantages

**Advantages:**

- Faster retrieval for suitable queries.
- Reduced need for full table scans.
- Efficient ordered and range-based access with B-tree indexes.

**Disadvantages:**

- Additional storage requirements.
- More maintenance during `INSERT`, `UPDATE` and `DELETE`.
- Not every query benefits from an index.

**Exam principle:** Indexes improve performance selectively; indexing every column is not necessarily beneficial.

### 9.3 Types of indexes

#### Single-column index

Uses one column.

```
CREATE INDEX idx_students_mark
ON Students (mark);
```

#### Unique index

Prevents duplicate indexed values.

```
CREATE UNIQUE INDEX idx_students_email
ON Students (email);
```

A unique index enforces uniqueness for its indexed key, subject to the DBMS's treatment of `NULL`.

#### Composite index

Uses multiple columns.

```
CREATE INDEX idx_students_city_age
ON Students (city, age);
```

The column order matters. An index on `(city, age)` is generally most useful for queries filtering by `city` or by both `city` and `age`, and may be less useful for queries filtering only by `age`.

### 9.4 Other index structures

| Index type | Main characteristic |
| --- | --- |
| B-tree / B+ tree | Ordered keys; supports equality and range queries |
| Hash index | Particularly suitable for equality lookups; generally unsuitable for range scans |
| LSM tree | Optimized for write-intensive workloads using memory and disk-based structures |
| Sparse index | Stores entries for selected keys or blocks rather than every record, depending on the design |

The appropriate index depends on the DBMS, data distribution and query workload.

## 10\. B-Tree Indexes

### 10.1 Core properties

A B-tree is a balanced, multiway search tree designed for efficient searching, insertion and deletion.

Key properties:

- Keys are maintained in sorted order.
- All leaf nodes are at the same depth.
- Nodes contain multiple keys and child pointers.
- High branching factors reduce tree height.
- Database index pages hold multiple keys, reducing disk I/O.

Database page sizes vary by implementation; some systems use pages around 4 KB, while others use different sizes.

### 10.2 Searching

1. Start at the root page.
2. Compare the target key with the keys in that page.
3. Follow the child pointer corresponding to the target's key range.
4. Repeat until the target is found or the relevant leaf is reached.

Search complexity is approximately **O(log n)** for a balanced tree, where `n` is the number of indexed entries.

### 10.3 Insertion

1. Find the appropriate leaf page.
2. Insert the key in sorted order.
3. If the page is full, split it.
4. Update the parent to represent the resulting key ranges.
5. Splits may propagate upward and can create a new root.

**Page split:** A full page is divided to make room for additional entries.

### 10.4 Deletion

1. Find the page containing the key.
2. Remove the entry.
3. If the node becomes underfull, redistribute entries with a sibling or merge nodes as needed.
4. Update parent pages and preserve the tree's balance.

Exact procedures depend on the B-tree variant and implementation.

### 10.5 Updating

- Locate the entry.
- Modify the relevant value.
- Persist the modified page.
- If an indexed key changes, additional index changes may be required because the key's position can change.

### 10.6 B-tree vs. B+ tree

| Feature | B-tree | B+ tree |
| --- | --- | --- |
| Key/record storage | Records or references may appear in internal and leaf nodes | Records or record pointers are generally stored at leaves |
| Search | May finish at an internal node | Typically reaches a leaf |
| Linked leaves | Not required | Commonly linked |
| Range scans | Supported | Particularly efficient with linked leaves |
| Database use | Used in some systems | Widely used for database indexes |

**Exam trap:** Not every database index is a B-tree. Many relational databases use B+ tree variants, while other index structures are also available.

### 10.7 B-tree and range-query example

Suppose an index on `Mark` contains sorted entries:

`5, 20, 24, 32, 37, 87, 89, 90`

A query such as:

```
SELECT *
FROM Students
WHERE Mark >= 80;
```

can use an appropriate index to locate qualifying entries efficiently.

**Important:** Sorted index entries do not necessarily mean the physical rows in the table are stored in that order.

## 11\. Essential Exam Comparisons

| Concepts | Difference |
| --- | --- |
| Database vs. DBMS | Organized data vs. software managing that data |
| SQL vs. DBMS | Query language vs. database management software |
| DDL vs. DML | Changes structure vs. changes stored records |
| DQL vs. DCL | Retrieves data vs. controls privileges |
| PK vs. FK | Uniquely identifies a row vs. references a key |
| `CREATE TABLE` vs. `INSERT` | Creates structure vs. adds records |
| `DELETE` vs. `DROP TABLE` | Removes rows vs. removes the table structure and data |
| `WHERE` vs. `HAVING` | Filters rows before grouping vs. filters groups after aggregation |
| `GROUP BY` vs. `ORDER BY` | Groups rows vs. sorts results |
| `COUNT(*)` vs. `COUNT(column)` | Counts rows vs. counts non-`NULL` values |
| `INNER JOIN` vs. `LEFT JOIN` | Matching rows only vs. every left-side row plus matches |
| B-tree vs. hash index | Ordered lookups and ranges vs. primarily equality lookups |
| Index advantage vs. cost | Faster suitable reads vs. extra storage and write overhead |
| Atomicity vs. durability | All-or-nothing transaction vs. persistence after commit |
| Isolation vs. consistency | Concurrent execution vs. valid database states |

## 12\. Common Exam Traps

1. `CREATE TABLE` does not insert records.
2. `INSERT` adds rows; `UPDATE` modifies existing rows.
3. `SELECT` retrieves data without modifying it.
4. `UPDATE` or `DELETE` without `WHERE` can affect every row.
5. A primary key must be unique and non-null.
6. An `INSERT` without a column list must follow the table's column order.
7. `WHERE` filters rows; `HAVING` filters aggregated groups.
8. `GROUP BY` produces separate aggregates for each group.
9. `COUNT(column)` ignores `NULL`; `COUNT(*)` counts rows.
10. `JOIN` combines records according to the `ON` condition.
11. `LEFT JOIN` preserves every left-table row, even without a match.
12. Indexes can improve retrieval but increase storage and write costs.
13. Composite-index column order affects which queries can use the index efficiently.
14. B-trees are balanced and ordered; insertion may cause page splits.
15. B+ trees commonly link leaf nodes for efficient range scans.
16. Sorted index keys do not necessarily determine the physical order of table rows.
17. SQL syntax such as auto-increment features can vary by DBMS.

## 13\. Last-Minute SQL Syntax Sheet

### Create a table

```
CREATE TABLE Students (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT
);
```

### Insert a record

```
INSERT INTO Students (id, name, age)
VALUES (1, 'Joan', 20);
```

### Select and filter

```
SELECT *
FROM Students
WHERE age >= 18
ORDER BY age DESC;
```

### Update a record

```
UPDATE Students
SET age = 21
WHERE id = 1;
```

### Delete a record

```
DELETE FROM Students
WHERE id = 1;
```

### Group and count

```
SELECT city, COUNT(*) AS total
FROM Students
GROUP BY city;
```

### Filter groups

```
SELECT city, COUNT(*) AS total
FROM Students
GROUP BY city
HAVING COUNT(*) > 2;
```

### Inner join

```
SELECT T1.column1, T2.column2
FROM T1
INNER JOIN T2
ON T1.id = T2.id;
```

### Left join

```
SELECT T1.column1, T2.column2
FROM T1
LEFT JOIN T2
ON T1.id = T2.id;
```

### Create an index

```
CREATE INDEX idx_students_age
ON Students (age);
```

## 14\. Quick Exam Self-Test

Try answering before checking the answer key.

**1\. Which category contains****`CREATE`****and****`DROP`****?**

- A. DQL
- B. DDL
- C. DML
- D. DCL

**2\. Which ACID property ensures committed changes survive failures?**

- A. Atomicity
- B. Consistency
- C. Isolation
- D. Durability

**3\. What is a tuple?**

- A. A column
- B. A table name
- C. A row or record
- D. A permission

**4\. What happens when****`UPDATE`****has no****`WHERE`****clause?**

- A. It updates no rows
- B. It updates the first row
- C. It updates all rows
- D. It creates a table

**5\. Which clause filters individual rows before grouping?**

- A. `HAVING`
- B. `WHERE`
- C. `ORDER BY`
- D. `GROUP BY`

**6\. Which clause filters aggregated groups?**

- A. `WHERE`
- B. `SELECT`
- C. `HAVING`
- D. `JOIN`

**7\. Which JOIN returns only matching rows from both tables?**

- A. `LEFT JOIN`
- B. `INNER JOIN`
- C. `FULL OUTER JOIN`
- D. `RIGHT JOIN`

**8\. Which JOIN preserves every row from the left table?**

- A. `RIGHT JOIN`
- B. `INNER JOIN`
- C. `LEFT JOIN`
- D. `CROSS JOIN`

**9\. What does****`AVG()`****calculate?**

- A. Total
- B. Minimum
- C. Average
- D. Number of rows

**10\. What is the main purpose of an index?**

- A. Delete duplicate tables
- B. Improve query performance
- C. Replace SQL
- D. Guarantee every query is faster

**11\. Which index uses multiple columns?**

- A. Unique index
- B. Single-column index
- C. Composite index
- D. Primary query

**12\. What is a key property of a B-tree?**

- A. It stores only text
- B. It keeps keys ordered and balanced
- C. It eliminates the need for tables
- D. It always sorts physical table rows

### Answer key

1. **B — DDL**
2. **D — Durability**
3. **C — A row or record**
4. **C — It updates all rows**
5. **B — WHERE**
6. **C — HAVING**
7. **B — INNER JOIN**
8. **C — LEFT JOIN**
9. **C — Average**
10. **B — Improve query performance**
11. **C — Composite index**
12. **B — It keeps keys ordered and balanced**

## Final 60-Second Revision

Memorize these core rules:

- **CRUD:** `INSERT`, `SELECT`, `UPDATE`, `DELETE`.
- **DDL:** `CREATE`, `DROP`, `RENAME`.
- **ACID:** Atomicity = all or nothing; Consistency = valid state; Isolation = concurrent transactions; Durability = committed changes persist.
- **Keys:** PK uniquely identifies a row; FK establishes a reference to another table.
- **Filtering:** `WHERE` filters rows; `HAVING` filters groups.
- **Aggregation:** `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.
- **Grouping:** `GROUP BY` calculates aggregates per group.
- **Sorting:** `ORDER BY`.
- **Combining tables:** `JOIN ... ON`.
- **Safety:** Check `WHERE` before executing `UPDATE` or `DELETE`.
- **Indexes:** Faster suitable reads, but additional storage and write overhead.
- **Composite indexes:** Column order matters.
- **B-trees:** Balanced, ordered, multiway trees supporting efficient searches and range queries.
- **B+ trees:** Commonly store records or references at linked leaf nodes, making range scans efficient.
- **Page splits:** Occur when a B-tree page cannot accommodate another entry.

**Highest-priority exam skill:** Be able to write and explain `CREATE TABLE`, `INSERT`, `SELECT ... WHERE`, `UPDATE ... WHERE`, `DELETE ... WHERE`, `GROUP BY`, `HAVING`, `JOIN`, and basic index-creation syntax. Also understand the trade-offs of indexes and how B-tree searches and page splits work.
