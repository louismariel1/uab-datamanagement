# Chunk 2 — SQL Tables and Basic CRUD Operations

## 1. Tables, Tuples, and Schema

- Table: Data organized in rows and columns.
- Tuple (row): One record in a table.
- Attribute (column): A property of the data.
- Schema: Table name + its attributes and their data types.
- Primary key (PK): Attribute(s) that uniquely identify each row. Values must be unique and cannot be `NULL`.

Example:

`Students(SName, Surname, Age, City)`

If `SName` is the primary key, every row must have a unique `SName`.

## 2. SQL Data Types

Every column must have a data type specifying what kind of values it can store.

| Category | Examples | Used for |
| --- | --- | --- |
| Numeric | `INT`, `DECIMAL`, `FLOAT` | Numbers |
| String | `CHAR`, `VARCHAR`, `TEXT` | Text |
| Date and time | `DATE`, `TIME`, `DATETIME` | Dates and times |

Exam tip: Know the difference between numeric, string, and date/time types. Exact types and supported syntax may vary by DBMS.

## 3. Creating a Table — `CREATE TABLE`

Creates a new table with specified columns and data types.

sql
```
CREATE TABLE Students (
    id INT,
    name VARCHAR(50),
    age INT
);
```

Remember: `CREATE TABLE` defines the structure; it does not insert records.

## 4. Defining a Primary Key

sql
```
CREATE TABLE Students (
    id INT,
    name VARCHAR(50),
    age INT,
    PRIMARY KEY (id)
);
```

- `PRIMARY KEY (id)` makes `id` the table's primary key.
- A primary key must be unique and non-null.
- It can consist of one column or multiple columns (a composite key).

## 5. Auto-Increment

Generates a new identifier automatically when a record is inserted.

Example in MySQL:

sql
```
CREATE TABLE Students (
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(50),
    PRIMARY KEY (id)
);
```

- Commonly used for primary keys.
- New rows receive automatically generated IDs.
- `AUTO_INCREMENT` is MySQL-specific syntax; other DBMSs use alternatives such as `IDENTITY` or sequences.

## 6. Inserting Data — `INSERT INTO`

Adds new records to a table.

### Option 1: Insert values without naming columns

sql
```
INSERT INTO Students
VALUES (1, 'Joan', 20);
```

- Values must match the table's column order.
- Supply appropriate values for all required columns.
- Less flexible if the table structure changes.

### Option 2: Specify the columns

sql
```
INSERT INTO Students (id, name, age)
VALUES (1, 'Joan', 20);
```

- Explicitly maps values to columns.
- You can omit columns that have defaults or allow `NULL`.
- Usually preferred because it is clearer and safer.

For an auto-increment ID:

sql
```
INSERT INTO Students (name, age)
VALUES ('Joan', 20);
```

The database generates the ID automatically.

## 7. Updating Data — `UPDATE`

Modifies existing records.

sql
```
UPDATE Students
SET name = 'Manuel'
WHERE id = 1;
```

- `UPDATE`: Table to modify.
- `SET`: Column(s) and their new value(s).
- `WHERE`: Specifies which rows to modify.

Updating multiple columns:

sql
```
UPDATE Students
SET name = 'Manuel', age = 21
WHERE id = 1;
```

Critical exam trap: Omitting `WHERE` updates every row in the table.

## 8. Retrieving Data — `SELECT`

Retrieves data from a table.

sql
```
SELECT name, age
FROM Students;
```

Retrieve all columns:

sql
```
SELECT *
FROM Students;
```

Filter rows using `WHERE`:

sql
```
SELECT *
FROM Students
WHERE age >= 18;
```

Important: `SELECT` reads data; it does not modify it.

## 9. CRUD — Must Memorize

| Operation | SQL command | Purpose |
| --- | --- | --- |
| Create | `INSERT` | Add records |
| Read | `SELECT` | Retrieve records |
| Update | `UPDATE` | Modify records |
| Delete | `DELETE` | Remove records |

Note: `CREATE TABLE` creates the table structure; `INSERT` creates individual records.

## 10. Common Exam Traps

- `CREATE TABLE` creates a table, not its records.
- `INSERT INTO` adds new rows.
- `UPDATE` changes existing rows.
- `SELECT` retrieves data.
- `WHERE` filters rows for `SELECT`, `UPDATE`, and `DELETE`.
- `UPDATE` or `DELETE` without `WHERE` affects all rows.
- Primary keys must be unique and non-null.
- Auto-increment is useful for automatically generated identifiers.
- When using `INSERT` without a column list, values must follow the table's column order.
- SQL data types and auto-increment syntax can vary by DBMS.

## Quick exam practice

## 7 multiple-choice questions

0/7 answered

1. What is a tuple in a relational table?

A column

A row

A data type

A table schema

2. Which command creates a new table?

INSERT INTO

UPDATE

CREATE TABLE

SELECT

3. What is the main purpose of a primary key?

Allow duplicate IDs

Identify rows uniquely

Store only text

Sort rows automatically

4. What happens if UPDATE has no WHERE clause?

It updates no rows

It updates the first row

It updates all rows

It creates a table

5. Which INSERT form is generally more flexible?

INSERT INTO T VALUES (...)

INSERT INTO T (columns) VALUES (...)

UPDATE T VALUES (...)

SELECT INTO T VALUES (...)

6. Which command retrieves data?

SELECT

UPDATE

INSERT

DELETE

7. What does AUTO\_INCREMENT do in MySQL?

Automatically generates values for a column

Prevents all updates

Sorts the table

Creates a foreign key

Reset quiz  Harder practice

Last-minute revision: Memorize the syntax of `CREATE TABLE`, `PRIMARY KEY`, `INSERT INTO`, `UPDATE ... SET ... WHERE`, and `SELECT ... FROM ... WHERE`. Always check whether a `WHERE` clause is needed before modifying data.
