# Chunk 3 — SQL Exam Compression

## 1. `SELECT *`

- `SELECT *` retrieves all columns from a table.
- `WHERE` filters rows according to a condition.

sql
```
SELECT *
FROM table_name
WHERE condition;
```

## 2. `GROUP BY`

Definition: Groups rows with the same values into summary rows, usually with aggregate functions.

- Used with aggregate functions such as `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`.
- Can be combined with `ORDER BY` to sort results.

sql
```
SELECT column_name, COUNT(*)
FROM table_name
WHERE condition
GROUP BY column_name
ORDER BY column_name;
```

Example: Count customers in each country.

sql
```
SELECT Country, COUNT(*)
FROM Customers
GROUP BY Country;
```

## 3. Aggregate functions

| Function | Purpose |
| --- | --- |
| `COUNT()` | Counts rows or values |
| `SUM()` | Calculates the total |
| `AVG()` | Calculates the average |
| `MIN()` | Finds the minimum |
| `MAX()` | Finds the maximum |

## 4. `HAVING`

Definition: Filters grouped or aggregated results, typically used with `GROUP BY`.

- `WHERE` filters rows before grouping.
- `HAVING` filters groups after grouping.

sql
```
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 2;
```

Exam trap: Use `HAVING` to filter aggregate results, such as groups with a count greater than 2.

## 5. SQL JOINs

Definition: A `JOIN` combines rows from two or more tables using a related column.

Types to memorize:

- `INNER JOIN`
- `LEFT JOIN`
- `RIGHT JOIN`
- `FULL OUTER JOIN`

General syntax:

sql
```
SELECT T1.column1, T2.column2
FROM T1
INNER JOIN T2
ON T1.id = T2.id;
```

### JOIN types — most important exam concept

SQL JOIN - Logicmojo

INNER JOIN

Returns only rows with matching values in both tables.

Relational databases: types of joins | by Software Sisterz | Medium

LEFT JOIN

Returns all rows from the left table and matching rows from the right. No match → `NULL` for right-table columns.

SQL JOINS (INNER, LEFT, RIGHT, and FULL Join) - Scaler Topics

RIGHT JOIN

Returns all rows from the right table and matching rows from the left. No match → `NULL` for left-table columns.

Top 15 SQL Interview Questions and Answers (with Examples) - Exponent

FULL OUTER JOIN

Returns all rows from both tables, matching where possible. Unmatched columns contain `NULL`.

### JOIN example

sql
```
SELECT ProductID, ProductName, CategoryName
FROM Products
INNER JOIN Categories
ON Products.CategoryID = Categories.CategoryID;
```

- `Products.CategoryID` matches `Categories.CategoryID`.
- The result combines product details with category names.
- `ON` specifies the matching condition.
- `PK` = Primary Key; `FK` = Foreign Key.

## 6. Quick comparison table

| SQL clause | What it does |
| --- | --- |
| `SELECT` | Chooses columns |
| `*` | Selects all columns |
| `WHERE` | Filters individual rows |
| `GROUP BY` | Groups rows with equal values |
| `HAVING` | Filters groups after aggregation |
| `ORDER BY` | Sorts the output |
| `JOIN` | Combines tables |
| `ON` | Specifies the join condition |

## 7. Exam-ready syntax to memorize

sql
```
-- Select all columns
SELECT * FROM table_name;

-- Group and count
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name;

-- Filter groups
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 2;

-- Inner join
SELECT T1.column1, T2.column2
FROM T1
INNER JOIN T2 ON T1.id = T2.id;

-- Left join
SELECT T1.column1, T2.column2
FROM T1
LEFT JOIN T2 ON T1.id = T2.id;

-- Right join
SELECT T1.column1, T2.column2
FROM T1
RIGHT JOIN T2 ON T1.id = T2.id;
```

## 8. Last-minute revision — test yourself

1. Which clause filters rows before grouping?

HAVING

WHERE

ORDER BY

2. Which clause filters aggregated groups?

WHERE

SELECT

HAVING

3. Which JOIN returns only matching rows from both tables?

LEFT JOIN

INNER JOIN

FULL OUTER JOIN

4. Which JOIN returns every row from the left table?

RIGHT JOIN

INNER JOIN

LEFT JOIN

5. What does SELECT \* return?

Only the primary key

All columns

Only numeric columns

 Reset quiz

Final memory trick: `WHERE` = rows, `GROUP BY` = groups, `HAVING` = filter groups, `ORDER BY` = sorting. For JOINs, remember which table's rows must all remain in the result.
