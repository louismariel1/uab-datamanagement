# Exam Compression Notes: Solving a Mystery with SQL

Data Management · 2026–27 · High-yield revision sheet

This chapter focuses on SQL fundamentals, filtering, aggregate functions, sorting, and joins, applied to a murder-mystery investigation. For exams, prioritize query syntax, operator differences, interpreting results, and connecting tables correctly.

## 1. SQL essentials — memorize first

SQL (Structured Query Language) is used to retrieve, filter, summarize, sort, and combine data stored in relational databases.

| Term | Meaning |
| --- | --- |
| Database | Organized collection of data |
| Table | Data arranged in rows and columns |
| Row / record | One entry in a table |
| Column / attribute | One property of a record |
| Query | Instruction sent to a database |
| SQLite | A database system; this lab uses SQLite |

### Core query structure

sql
```
SELECT column_name
FROM table_name
WHERE condition;
```

- `SELECT` — specifies which columns to display.
- `FROM` — specifies the table to query.
- `WHERE` — filters rows according to a condition.
- `*` — selects all columns.

Example:

sql
```
SELECT *
FROM person;
```

Returns all columns and all rows in `person`.

sql
```
SELECT name
FROM person
WHERE address_street_name = 'Franklin Ave';
```

Returns names of people living on Franklin Ave.

Exam trap: `SELECT *` means all columns, not all tables.

## 2. Essential SQL commands and syntax

A. List tables in SQLite

sql
```
SELECT name
FROM sqlite_master
WHERE type = 'table';
```

Lists database tables.

B. Inspect table structure

sql
```
PRAGMA table_info(person);
```

Displays column names and other schema information in SQLite.

C. Count records

sql
```
SELECT COUNT(*)
FROM person;
```

Chapter result: 10,011 records.

D. Filter rows

sql
```
SELECT *
FROM crime_scene_report
WHERE city = 'SQL City'
  AND type = 'murder';
```

Both conditions must be true.

## 3. Filtering operators — common exam questions

| Operator | Meaning | Example |
| --- | --- | --- |
| `=` | Equal to | `city = 'SQL City'` |
| `>` | Greater than | `age > 60` |
| `<` | Less than | `age < 30` |
| `>=` | Greater than or equal | `age >= 18` |
| `<=` | Less than or equal | `age <= 65` |
| `AND` | Both conditions true | `city = 'SQL City' AND type = 'murder'` |
| `OR` | At least one condition true | `type = 'murder' OR type = 'robbery'` |
| `LIKE` | Pattern matching | `city LIKE '%City'` |
| `BETWEEN` | Within an inclusive range | `age BETWEEN 18 AND 65` |
| `DISTINCT` | Removes duplicate values | `SELECT DISTINCT city FROM crime_scene_report` |

### Wildcards: `LIKE`

- `%` — any sequence of characters, including zero characters.
- `_` — exactly one character.

sql
```
-- Ends with "City"
WHERE city LIKE '%City'

-- Starts with "SQL"
WHERE city LIKE 'SQL%'

-- Contains "City"
WHERE city LIKE '%City%'
```

Remember: Use `LIKE` for pattern matching and `=` for exact equality.

### `BETWEEN`

sql
```
SELECT *
FROM drivers_license
WHERE age BETWEEN 18 AND 65;
```

Includes both endpoints: 18 and 65.

sql
```
SELECT DISTINCT city
FROM crime_scene_report;
```

Returns unique city values.

Exam trap: `BETWEEN` is inclusive. `DISTINCT` removes duplicate results; it does not sort them.

## 4. Aggregate functions — learn all five

Aggregate functions calculate a summary over multiple rows.

| Function | Purpose | Example |
| --- | --- | --- |
| `COUNT()` | Counts records or non-NULL values, depending on argument | `COUNT(*)` |
| `SUM()` | Adds numeric values | `SUM(age)` |
| `AVG()` | Calculates average | `AVG(age)` |
| `MIN()` | Finds minimum | `MIN(age)` |
| `MAX()` | Finds maximum | `MAX(age)` |

Examples:

sql
```
SELECT COUNT(*) FROM person;

SELECT AVG(age) FROM drivers_license;

SELECT MIN(age) FROM drivers_license;

SELECT MAX(age) FROM drivers_license;

SELECT SUM(age) FROM drivers_license;
```

Important distinction:

- `COUNT(*)` counts rows, including rows containing NULL values.
- `COUNT(column_name)` counts non-NULL values in that column.
- `AVG()`, `MIN()`, `MAX()`, and `SUM()` generally ignore NULL inputs.

Exam trap: `MIN(age)` returns the smallest age, not the name of the youngest person. To retrieve the person's name, combine tables and sort or use a suitable query.

## 5. Sorting and limiting results

sql
```
SELECT *
FROM drivers_license
ORDER BY age ASC;
```

- `ASC` — ascending order; default.
- `DESC` — descending order.

sql
```
SELECT *
FROM drivers_license
ORDER BY age DESC
LIMIT 10;
```

Returns up to 10 records with the highest ages.

To find the youngest records:

sql
```
SELECT *
FROM drivers_license
ORDER BY age ASC
LIMIT 10;
```

Syntax to memorize:

sql
```
SELECT ...
FROM ...
WHERE ...
ORDER BY column ASC
LIMIT 10;
```

The logical processing order is approximately:

`FROM` → `WHERE` → `SELECT` → `ORDER BY` → `LIMIT`

This is the conceptual processing order, not the order in which the clauses are written.

## 6. JOINs — the most important concept

A JOIN combines related information from multiple tables using matching columns.

Example tables:

- `person` contains names and `license_id`.
- `drivers_license` contains `id`, age, and vehicle details.

The relationship is:

`person.license_id = drivers_license.id`

### How the join works

person

name

license\_id

drivers\_license

id

age

Match license\_id = id

Combined result: person's name + their age

### Query using an explicit JOIN

sql
```
SELECT person.name, drivers_license.age
FROM person
JOIN drivers_license
  ON person.license_id = drivers_license.id;
```

### Equivalent older syntax used in the chapter

sql
```
SELECT person.name, drivers_license.age
FROM person, drivers_license
WHERE person.license_id = drivers_license.id;
```

Both express an inner join when the matching condition is present.

Three-step JOIN method

1. Identify the required columns.
2. Identify the tables containing those columns.
3. Identify the matching key columns and write the join condition.

Critical exam trap: If you list two tables without a matching condition, you can produce a Cartesian product, combining every row from one table with every row from the other.

## 7. Murder mystery — facts to remember

These are the facts and expected results explicitly provided in the chapter.

| Item | Key fact |
| --- | --- |
| Database software | SQLite; Letos (formerly SQLiteStudio) |
| `person` table | Personal details and addresses |
| `person` attributes | `id`, `name`, `license_id`, `address_number`, `address_street_name`, `ssn` |
| Person records | 10,011 |
| `crime_scene_report` attributes | `date`, `type`, `description`, `city` |
| Murder report date | `20180115` (15 January 2018) |
| Crime location | SQL City |
| Reports in SQL City | 9 |
| Witness 1 | Lives at the last house on Northwestern Dr |
| Witness 2 | Annabel, living on Franklin Ave |
| `interview` table | Contains witness transcripts |
| Interview link | `person.id = interview.person_id` |
| Suspect investigation tables | `person`, `drivers_license`, `get_fit_now_member`, `get_fit_now_check_in` |

### Queries you should be able to write

Find residents of Franklin Ave

sql
```
SELECT *
FROM person
WHERE address_street_name = 'Franklin Ave';
```

Find the last house on Northwestern Dr

sql
```
SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1;
```

This assumes `address_number` is stored as a numeric value. If it is stored as text, numeric ordering may require a cast.

Retrieve witness names and interviews

sql
```
SELECT person.name, interview.transcript
FROM person
JOIN interview
  ON person.id = interview.person_id;
```

This retrieves all matching interview records. To obtain only the two witnesses, first identify their person IDs and then filter using those IDs.

Important: The supplied chapter does not contain enough completed query results to establish the murderer conclusively. Do not memorize an unsupported suspect name as the answer.

## 8. Common mistakes — quick correction sheet

| Mistake | Correct approach |
| --- | --- |
| Forgetting `FROM` | Specify the source table |
| Using `==` out of habit | Use SQL's standard equality operator `=` |
| Using `=` for partial matching | Use `LIKE` with `%` or `_` |
| Forgetting quotes around text | Use `'SQL City'` for string values |
| Counting with `SELECT COUNT` | Use `SELECT COUNT(*)` |
| Assuming `BETWEEN` excludes endpoints | Both endpoints are included |
| Sorting with `ORDER_BY` | Correct syntax is `ORDER BY` |
| Writing `LIMIT` before `ORDER BY` | Put `LIMIT` after `ORDER BY` |
| Joining without a key condition | Match related columns using `ON` or `WHERE` |
| Selecting a person's name from the wrong table | Qualify it, e.g. `person.name` |

## 9. Exam practice — test yourself

## 8-question mini mock

0/8 answered

1. Which SQL clause filters rows?

SELECT

FROM

WHERE

ORDER BY

2. Which query counts all rows in person?

SELECT SUM(\*) FROM person;

SELECT COUNT(\*) FROM person;

SELECT ALL FROM person;

SELECT NUMBER(\*) FROM person;

3. What does LIKE '%City' match?

Values beginning with City

Values containing only City

Values ending with City

Values exactly equal to City

4. What does BETWEEN 18 AND 65 include?

18 and 65

Only values greater than 18 and less than 65

18 but not 65

Neither endpoint

5. Which function returns the average?

SUM()

COUNT()

AVG()

MAX()

6. Which condition connects person to drivers\_license?

person.name = drivers\_license.age

person.id = drivers\_license.age

person.license\_id = drivers\_license.id

person.ssn = drivers\_license.model

7. Which clause sorts query results?

ORDER BY

GROUP BY

SORT

ARRANGE

8. What does SELECT DISTINCT city return?

All columns

Only the first city alphabetically

Unique city values

The number of cities

 Check answers

## Final 60-second revision

Memorize these five patterns before the exam:

sql
```
-- Retrieve
SELECT * FROM table_name;

-- Filter
SELECT * FROM table_name
WHERE condition;

-- Count
SELECT COUNT(*) FROM table_name;

-- Sort and limit
SELECT * FROM table_name
ORDER BY column_name DESC
LIMIT 10;

-- Join
SELECT a.column_name, b.column_name
FROM table_a AS a
JOIN table_b AS b
  ON a.key = b.key;
```

The central exam skill: Convert the question into three decisions — what information do I need (`SELECT`), where is it stored (`FROM`/`JOIN`), and which records qualify (`WHERE`)? Then add aggregation, sorting, or `LIMIT` when the question requires it.
