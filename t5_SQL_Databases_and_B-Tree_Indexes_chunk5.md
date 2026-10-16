# Chunk 5 — Exam Compression: SQL & B-Tree Indexes

## 1. Relational databases

- Relational database: stores information in tables (e.g., MySQL).
- Table: consists of rows (records) and columns (attributes).
- SQL (Structured Query Language): standard language for managing relational databases.

Example table: `students`

| Name | Mark | Age |
| --- | --- | --- |
| Zack | 90 | 34 |
| John | 24 | 32 |
| Jones | 5 | 28 |
| Alex | 32 | 45 |
| Tom | 37 | 23 |
| Ron | 87 | 33 |
| Max | 20 | 48 |
| Bob | 89 | 32 |

## 2. SQL commands — must memorize

### CRUD operations

| Operation | SQL command | Purpose |
| --- | --- | --- |
| Create data | `INSERT` | Add records |
| Read data | `SELECT` | Retrieve records |
| Update data | `UPDATE` | Modify records |
| Delete data | `DELETE` | Remove records |

Example: Create a table

sql
```
CREATE TABLE students (
    Name VARCHAR(50),
    Mark INT,
    Age INT
);
```

Insert data:

sql
```
INSERT INTO students (Name, Mark, Age)
VALUES ('John', 24, 32);
```

Retrieve data:

sql
```
SELECT * FROM students;
```

Update data:

sql
```
UPDATE students
SET Mark = 30
WHERE Name = 'John';
```

Delete data:

sql
```
DELETE FROM students
WHERE Name = 'John';
```

Exam tip: `WHERE` specifies which rows to modify or delete. Without it, `UPDATE` or `DELETE` can affect every row.

### Other important SQL concepts

- `GROUP BY`: groups rows with the same value for aggregation.
- `HAVING`: filters groups after aggregation.
- `JOIN`: combines related data from multiple tables.

## 3. Indexes

Definition: An index is a data structure that improves the speed of searching and retrieving data from a table.

- Main purpose: reduce search time and improve query performance.
- Trade-off: indexes require additional storage and can slow down `INSERT`, `UPDATE`, and `DELETE` because indexes may need updating.

### Three index types

1. Single-column index

Created using one column.

sql
```
CREATE INDEX index_name
ON table_name (column_name);
```

Example:

sql
```
CREATE INDEX idx_students_mark
ON students (Mark);
```

2. Unique index

Ensures that indexed values are unique; duplicate values are rejected.

sql
```
CREATE UNIQUE INDEX index_name
ON table_name (column_name);
```

3. Composite index

Created using multiple columns.

sql
```
CREATE INDEX index_name
ON table_name (column1, column2);
```

The order of columns matters. An index on `(column1, column2)` is generally most useful for queries filtering by `column1` or by both columns, rather than `column2` alone.

## 4. B-tree indexes — exam essentials

- A B-tree is a balanced tree data structure used to organize indexed keys.
- It keeps keys ordered, enabling efficient searching, insertion, and deletion.
- It reduces the amount of data that must be examined compared with a full table scan.
- B-tree indexes support equality searches and range queries, such as `Mark >= 80`.
- A database can use the index to locate matching records rather than scanning every row.

Example: An index on `Mark` can organize values in ascending order:

`5, 20, 24, 32, 37, 87, 89, 90`

This makes it easier to locate marks within a range.

Important distinction: the sorted index entries are not necessarily the physical order of rows in the table.

## 5. Key takeaways — memorize these

- SQL manages relational databases.
- Tables contain rows and columns.
- CRUD = `INSERT`, `SELECT`, `UPDATE`, `DELETE`.
- `GROUP BY` groups rows; `HAVING` filters groups.
- `JOIN` combines related tables.
- Indexes speed up searches but require storage and maintenance.
- Single-column index = one column.
- Unique index = prevents duplicate indexed values.
- Composite index = multiple columns; column order matters.
- B-tree indexes maintain ordered keys and support efficient searches and range queries.

## Quick exam practice

## 6 multiple-choice questions

0/6 answered

1. Which SQL command retrieves data?

INSERT

SELECT

UPDATE

DELETE

2. Which clause filters aggregated groups?

WHERE

ORDER BY

HAVING

JOIN

3. Which index uses multiple columns?

Unique index

Single-column index

Composite index

Primary query

4. What is the main purpose of an index?

Delete duplicate tables

Improve query performance

Replace SQL

Guarantee every query is faster

5. What does CRUD stand for in SQL?

Create, Read, Update, Delete

Copy, Remove, Undo, Deploy

Create, Run, Use, Drop

Connect, Read, Upload, Delete

6. What is a key property of a B-tree index?

It stores only text

It keeps keys ordered and balanced

It removes the need for tables

It always sorts physical rows

 Check answers
