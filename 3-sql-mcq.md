# Comprehensive SQL & Relational Databases MCQ

 ## Section A — SQL and Relational Database Fundamentals

 ### 1\. What does SQL stand for?

 A. Structured Query Language\
 B. Simple Query Language\
 C. Structured Question Language\
 D. System Query Logic

 **Answer: A**

 **Explanation:** SQL stands for **Structured Query Language**. It is the standard language used to communicate with and manage relational databases.

---

 ### 2\. What is the primary purpose of a relational database?

 A. To store data only as images\
 B. To store data in related tables\
 C. To store data only in text files\
 D. To execute programs

 **Answer: B**

 **Explanation:** Relational databases organize information into **tables** consisting of rows and columns. Relationships can be established between tables using keys.

---

 ### 3\. A relational database table is primarily composed of:

 A. Files and folders\
 B. Objects and classes\
 C. Rows and columns\
 D. Programs and functions

 **Answer: C**

 **Explanation:** A table consists of **rows**, representing records, and **columns**, representing attributes or properties.

---

 ### 4\. In a relational table, what does a row normally represent?

 A. An attribute\
 B. A database\
 C. A record\
 D. A data type

 **Answer: C**

 **Explanation:** Each row normally represents one **record/entity instance**, such as one student.

---

 ### 5\. In a relational table, what does a column represent?

 A. An attribute\
 B. A complete record\
 C. A database\
 D. An index tree

 **Answer: A**

 **Explanation:** A column represents an **attribute**, such as `Name`, `Age`, or `Mark`.

---

 ### 6\. Which of the following is an example of a relational database management system?

 A. MySQL\
 B. HTML\
 C. CSS\
 D. JPEG

 **Answer: A**

 **Explanation:** **MySQL** is a relational database management system (RDBMS) that uses SQL.

---

 ## Section B — CRUD Operations

 ### 7\. What does CRUD stand for?

 A. Create, Read, Update, Delete\
 B. Create, Run, Undo, Display\
 C. Copy, Read, Update, Drop\
 D. Create, Retrieve, Use, Display

 **Answer: A**

 **Explanation:** CRUD represents the four fundamental data operations: **Create, Read, Update, and Delete**.

---

 ### 8\. Which SQL command is primarily used to add new records?

 A. `SELECT`\
 B. `INSERT`\
 C. `UPDATE`\
 D. `DELETE`

 **Answer: B**

 **Explanation:** `INSERT` adds new rows to a table.

---

 ### 9\. Which SQL command is used to retrieve data?

 A. `SELECT`\
 B. `INSERT`\
 C. `UPDATE`\
 D. `CREATE`

 **Answer: A**

 **Explanation:** `SELECT` is used to read/retrieve data from one or more tables.

---

 ### 10\. Which SQL command modifies existing records?

 A. `ALTER`\
 B. `UPDATE`\
 C. `INSERT`\
 D. `SELECT`

 **Answer: B**

 **Explanation:** `UPDATE` changes values in existing rows.

---

 ### 11\. Which SQL command removes records from a table?

 A. `REMOVE`\
 B. `DROP`\
 C. `DELETE`\
 D. `CLEAR`

 **Answer: C**

 **Explanation:** `DELETE` removes rows from a table.

---

 ### 12\. Which statement correctly inserts a student?

 A.

```
INSERT students VALUES ('Zack', 90, 34);
```

 B.

```
INSERT INTO students (Name, Mark, Age)
VALUES ('Zack', 90, 34);
```

 C.

```
ADD INTO students ('Zack', 90, 34);
```

 D.

```
CREATE students VALUES ('Zack', 90, 34);
```

 **Answer: B**

 **Explanation:** The standard SQL syntax is:

```
INSERT INTO table_name (columns)
VALUES (values);
```

---

 ### 13\. What is the purpose of the `UPDATE` statement?

 A. To create a table\
 B. To retrieve data\
 C. To modify existing data\
 D. To create an index

 **Answer: C**

 **Explanation:** `UPDATE` changes existing values in table rows.

---

 ### 14. Why is `WHERE` important when using `UPDATE`?

 A. It creates an index\
 B. It limits which rows are modified\
 C. It sorts the table\
 D. It creates a new database

 **Answer: B**

 **Explanation:** Without an appropriate `WHERE` condition, an `UPDATE` can modify **every row** in the table.

---

 ### 15\. What can happen if the following statement is executed?

```
DELETE FROM students;
```

 A. Only one student is deleted\
 B. The table structure is deleted\
 C. All rows in `students` are deleted\
 D. An index is created

 **Answer: C**

 **Explanation:** A `DELETE` statement without a `WHERE` clause removes **all records**, while the table itself remains.

---

 ## Section C — SELECT and Filtering

 Consider this table:

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

### 16\. Which query retrieves every column from `students`?

 A.

```
GET * FROM students;
```

 B.

```
SELECT ALL students;
```

 C.

```
SELECT * FROM students;
```

 D.

```
SHOW students;
```

 **Answer: C**

 **Explanation:** `SELECT *` retrieves all columns from the specified table.

---

 ### 17\. Which query retrieves only names?

 A.

```
SELECT Name FROM students;
```

 B.

```
GET Name students;
```

 C.

```
SELECT students.Name;
```

 D.

```
READ Name FROM students;
```

 **Answer: A**

 **Explanation:** `SELECT Name FROM students` returns only the `Name` column.

---

 ### 18\. Which query returns students with marks greater than 50?

 A.

```
SELECT * FROM students WHERE Mark > 50;
```

 B.

```
SELECT * FROM students HAVING Mark > 50;
```

 C.

```
SELECT * FROM students IF Mark > 50;
```

 D.

```
SELECT * FROM students GROUP Mark > 50;
```

 **Answer: A**

 **Explanation:** `WHERE` is used to filter individual rows.

---

 ### 19\. How many students have a mark greater than 50?

 A. 3\
 B. 4\
 C. 5\
 D. 6

 **Answer: C**

 **Explanation:** The marks greater than 50 are:

 - Zack — 90
- Ron — 87
- Bob — 89

 Actually, there are **3**, not 5.

 **Correct Answer: A — 3.**

 This is an important example of checking the actual data rather than guessing.

---

 ### 20\. Which students have a mark below 30?

 A. John, Jones, Max\
 B. Zack, John, Max\
 C. Jones, Alex, Tom\
 D. John, Ron, Bob

 **Answer: A**

 **Explanation:** Their marks are:

 - John = 24
- Jones = 5
- Max = 20

 All are below 30.

---

 ### 21\. Which operator means "greater than or equal to"?

 A. `=>`\
 B. `>=`\
 C. `=<`\
 D. `==`

 **Answer: B**

 **Explanation:** SQL uses `>=` for greater than or equal to.

---

 ### 22\. Which operator means "not equal to" in SQL?

 A. `<>`\
 B. `><`\
 C. `=!` only\
 D. `NOT=`

 **Answer: A**

 **Explanation:** `<>` is the standard SQL not-equal operator. Many SQL systems also support `!=`.

---

 ### 23\. Which logical operator requires both conditions to be true?

 A. `OR`\
 B. `AND`\
 C. `NOT`\
 D. `BOTH`

 **Answer: B**

 **Explanation:** `AND` requires all specified conditions to be true.

---

 ## Section D — Sorting

 ### 24\. Which clause is used to sort query results?

 A. `SORT BY`\
 B. `ORDER BY`\
 C. `GROUP BY`\
 D. `ARRANGE BY`

 **Answer: B**

 **Explanation:** `ORDER BY` sorts query results.

---

 ### 25\. What does `ASC` mean?

 A. Accurate\
 B. Ascending\
 C. Association\
 D. Access

 **Answer: B**

 **Explanation:** `ASC` sorts values in **ascending order**.

---

 ### 26\. What does `DESC` mean?

 A. Descending\
 B. Description\
 C. Decreasing SQL Command\
 D. Database Search Command

 **Answer: A**

 **Explanation:** `DESC` sorts values in **descending order**.

---

 ### 27\. Which query sorts students by mark from highest to lowest?

 A.

```
SELECT * FROM students ORDER BY Mark ASC;
```

 B.

```
SELECT * FROM students ORDER BY Mark DESC;
```

 C.

```
SELECT * FROM students SORT Mark;
```

 D.

```
SELECT * FROM students GROUP BY Mark DESC;
```

 **Answer: B**

 **Explanation:** `DESC` sorts from highest to lowest.

---

 ### 28\. What would be the first mark after sorting the given data in ascending order?

 A. 90\
 B. 89\
 C. 5\
 D. 20

 **Answer: C**

 **Explanation:** The marks in ascending order are:

 **5, 20, 24, 32, 37, 87, 89, 90**

---

 ## Section E — Aggregate Functions

 ### 29\. Which function counts rows?

 A. `SUM()`\
 B. `COUNT()`\
 C. `TOTAL()`\
 D. `NUMBER()`

 **Answer: B**

 **Explanation:** `COUNT()` counts rows or non-null values depending on how it is used.

---

 ### 30\. Which function calculates an average?

 A. `MEAN()`\
 B. `AVERAGE()`\
 C. `AVG()`\
 D. `MID()`

 **Answer: C**

 **Explanation:** SQL uses `AVG()` to calculate the average.

---

 ### 31\. Which function calculates the total of numeric values?

 A. `TOTAL()`\
 B. `SUM()`\
 C. `ADD()`\
 D. `COUNT()`

 **Answer: B**

 **Explanation:** `SUM()` calculates the total of numeric values.

---

 ### 32\. Which function finds the largest value?

 A. `HIGH()`\
 B. `MAX()`\
 C. `TOP()`\
 D. `LARGE()`

 **Answer: B**

 **Explanation:** `MAX()` returns the maximum value.

---

 ### 33\. Which function finds the smallest value?

 A. `MIN()`\
 B. `LOW()`\
 C. `SMALL()`\
 D. `BOTTOM()`

 **Answer: A**

 **Explanation:** `MIN()` returns the minimum value.

---

 ### 34\. What is the average mark of the eight students?

 A. 42.75\
 B. 48.75\
 C. 50.50\
 D. 52.25

 **Answer: B**

 **Explanation:**

 The marks are:

 90 + 24 \+ 5 + 32 + 37 \+ 87 + 20 + 89 = **384**

 There are 8 students.

 384 ÷ 8 = **48**

 So the correct mathematical average is **48**, meaning none of the listed options is correct.

 **Correct answer: 48.**

---

 ## Section F — GROUP BY and HAVING

 ### 35\. What is the main purpose of `GROUP BY`?

 A. To delete groups\
 B. To sort individual rows\
 C. To organize rows into groups for aggregation\
 D. To create indexes

 **Answer: C**

 **Explanation:** `GROUP BY` groups rows with the same value so aggregate functions can be applied to each group.

---

 ### 36\. Which clause is commonly used with aggregate functions to create groups?

 A. `GROUP BY`\
 B. `ORDER BY`\
 C. `WHERE`\
 D. `CREATE BY`

 **Answer: A**

 **Explanation:** `GROUP BY` is specifically designed for grouping records.

---

 ### 37\. What is the main purpose of `HAVING`?

 A. Filter individual rows before grouping\
 B. Filter groups after aggregation\
 C. Sort records\
 D. Create tables

 **Answer: B**

 **Explanation:** `HAVING` filters the results of grouped/aggregated data.

---

 ### 38\. What is the key difference between `WHERE` and `HAVING`?

 A. They are exactly the same\
 B. `WHERE` filters rows; `HAVING` filters groups\
 C. `WHERE` filters groups; `HAVING` filters rows\
 D. `WHERE` creates tables; `HAVING` deletes them

 **Answer: B**

 **Explanation:** This is one of the most important SQL distinctions:

 **WHERE → individual rows**

 **HAVING → groups**

---

 ### 39\. Which query correctly finds age groups containing more than one student?

 A.

```
SELECT Age, COUNT(*)
FROM students
GROUP BY Age
HAVING COUNT(*) > 1;
```

 B.

```
SELECT Age, COUNT(*)
FROM students
WHERE COUNT(*) > 1;
```

 C.

```
SELECT Age
FROM students
HAVING Age > 1;
```

 D.

```
GROUP students BY Age HAVING COUNT;
```

 **Answer: A**

 **Explanation:** Aggregate conditions such as `COUNT(*) > 1` are placed in `HAVING`.

---

 ### 40\. In the provided data, which age occurs more than once?

 A. 23\
 B. 28\
 C. 32\
 D. 45

 **Answer: C**

 **Explanation:** John and Bob are both age **32**.

---

 ## Section G — JOINs

 ### 41\. What is the main purpose of a JOIN?

 A. To delete tables\
 B. To combine related data from multiple tables\
 C. To create indexes\
 D. To sort one column

 **Answer: B**

 **Explanation:** JOINs allow related information stored in different tables to be combined.

---

 ### 42\. Which JOIN returns only records with matching values in both tables?

 A. `LEFT JOIN`\
 B. `RIGHT JOIN`\
 C. `INNER JOIN`\
 D. `OUTER JOIN`

 **Answer: C**

 **Explanation:** `INNER JOIN` returns rows where the join condition has a match in both tables.

---

 ### 43\. What does a LEFT JOIN generally return?

 A. Only matching rows from both tables\
 B. All rows from the left table plus matching rows from the right\
 C. All rows from the right table only\
 D. Only unmatched rows

 **Answer: B**

 **Explanation:** A `LEFT JOIN` preserves all records from the left table and includes matching records from the right table.

---

 ### 44\. Which clause normally specifies how two tables are related in a JOIN?

 A. `USING` only\
 B. `ON`\
 C. `WHERE` only\
 D. `MATCH`

 **Answer: B**

 **Explanation:** The `ON` clause specifies the condition used to match records.

 Example:

```
SELECT *
FROM students
INNER JOIN courses
ON students.course_id = courses.id;
```

---

 ## Section H — Keys and Relationships

 ### 45\. What is the primary purpose of a primary key?

 A. To sort all records\
 B. To uniquely identify each record\
 C. To delete duplicate tables\
 D. To create a backup

 **Answer: B**

 **Explanation:** A **primary key** uniquely identifies each record in a table.

---

 ### 46\. Which characteristic should a primary key have?

 A. Duplicate values\
 B. Unique values\
 C. Only text values\
 D. Only negative values

 **Answer: B**

 **Explanation:** A primary key must uniquely identify records, so duplicate key values are not allowed.

---

 ### 47\. Why are keys important in relational databases?

 A. They allow tables to be related\
 B. They replace SQL\
 C. They automatically delete data\
 D. They prevent all database errors

 **Answer: A**

 **Explanation:** Keys help uniquely identify records and establish relationships between tables.

---

 ## Section I — Indexes

 ### 48\. What is the main purpose of an index?

 A. To increase table size\
 B. To improve data retrieval performance\
 C. To replace the table\
 D. To delete duplicate records

 **Answer: B**

 **Explanation:** Indexes provide additional structures that allow the database to find data more efficiently.

---

 ### 49\. What type of index is specifically discussed in the material?

 A. Hash-only index\
 B. B-tree index\
 C. XML index\
 D. Stack index

 **Answer: B**

 **Explanation:** The material focuses on **B-tree indexes**.

---

 ### 50\. What is the main advantage of a B-tree index?

 A. It eliminates the need for tables\
 B. It organizes indexed values to support efficient searching\
 C. It prevents all updates\
 D. It stores only text

 **Answer: B**

 **Explanation:** B-tree indexes organize values in a structure that allows efficient searching and locating corresponding records.

---

 ### 51\. Which statement creates a basic index?

 A.

```
CREATE INDEX index_name
ON table_name (column_name);
```

 B.

```
MAKE INDEX index_name
FROM table_name;
```

 C.

```
INDEX CREATE column_name;
```

 D.

```
ADD INDEX TO table_name;
```

 **Answer: A**

 **Explanation:** This is the standard SQL syntax for creating a single-column index.

---

 ### 52\. Which statement creates an index on the `mark` column?

 A.

```
CREATE INDEX idx_mark
ON students(mark);
```

 B.

```
CREATE TABLE idx_mark
ON students(mark);
```

 C.

```
MAKE INDEX students.mark;
```

 D.

```
CREATE MARK INDEX students;
```

 **Answer: A**

 **Explanation:** The syntax is `CREATE INDEX index_name ON table_name(column_name)`.

---

 ### 53\. What is a single-column index?

 A. An index based on one column\
 B. An index containing only one record\
 C. An index containing one table only\
 D. An index that can never be unique

 **Answer: A**

 **Explanation:** A single-column index is created using one table column.

---

 ### 54. Which statement creates a unique index?

 A.

```
CREATE INDEX UNIQUE index_name
ON table_name(column_name);
```

 B.

```
CREATE UNIQUE INDEX index_name
ON table_name(column_name);
```

 C.

```
UNIQUE CREATE index_name;
```

 D.

```
CREATE INDEX index_name UNIQUE ONLY;
```

 **Answer: B**

 **Explanation:** The correct syntax places `UNIQUE` after `CREATE`:

```
CREATE UNIQUE INDEX ...
```

---

 ### 55\. What is the defining characteristic of a unique index?

 A. It contains only one row\
 B. The indexed values must be unique\
 C. It can contain unlimited duplicate values\
 D. It can only be used on primary keys

 **Answer: B**

 **Explanation:** A unique index prevents duplicate values in the indexed attribute.

---

 ### 56. What is a composite index?

 A. An index based on multiple columns\
 B. An index containing multiple databases\
 C. An index that has multiple tables\
 D. An index that contains only unique values

 **Answer: A**

 **Explanation:** A composite index uses **two or more columns**.

---

 ### 57\. Which statement creates a composite index?

 A.

```
CREATE INDEX idx
ON students(mark, Age);
```

 B.

```
CREATE MULTI INDEX idx;
```

 C.

```
CREATE INDEX idx
ON students(mark + Age);
```

 D.

```
COMPOSITE INDEX idx(mark, Age);
```

 **Answer: A**

 **Explanation:** Multiple columns are listed inside the parentheses.

---

 ### 58\. Which index is created by this statement?

```
CREATE INDEX idx_estudiantes_mark
ON estudiantes(mark);
```

 A. Composite index\
 B. Single-column index\
 C. Unique index\
 D. Primary key

 **Answer: B**

 **Explanation:** Only the `mark` column is specified, so this is a **single-column index**.

---

 ## Section J — Indexing and Performance

 ### 59\. Why can indexes improve SELECT performance?

 A. They reduce the amount of searching required\
 B. They eliminate SQL\
 C. They automatically remove records\
 D. They make every query instantaneous

 **Answer: A**

 **Explanation:** An index provides an organized lookup structure, reducing the need to scan every table row.

---

 ### 60\. Do indexes require additional storage?

 A. No\
 B. Yes\
 C. Only for `SELECT`\
 D. Only for empty tables

 **Answer: B**

 **Explanation:** An index is an additional data structure and therefore requires storage.

---

 ### 61\. What is a potential disadvantage of having too many indexes?

 A. They can increase storage and maintenance costs\
 B. They prevent all SELECT queries\
 C. They delete primary keys\
 D. They make tables impossible to create

 **Answer: A**

 **Explanation:** Indexes must be maintained when data changes. Too many indexes can increase storage requirements and make `INSERT`, `UPDATE`, and `DELETE` operations more expensive.

---

 ### 62\. Which operation generally benefits most directly from indexes?

 A. Data retrieval/search\
 B. Turning off the database\
 C. Creating a programming language\
 D. Formatting text

 **Answer: A**

 **Explanation:** Indexes are primarily designed to make **data retrieval and searching** more efficient.

---

 ## Section K — Scenario-Based Questions

 ### 63\. A query needs to find every student with `Mark = 89`. Which index would be most directly useful?

 A. Index on `Age`\
 B. Index on `Name`\
 C. Index on `Mark`\
 D. Index on an unrelated column

 **Answer: C**

 **Explanation:** Since the query searches by `Mark`, an index on `Mark` can help the database locate matching rows efficiently.

---

 ### 64\. A database frequently searches using both `Mark` and `Age`. Which index could be appropriate?

 A. Single-column index on Name\
 B. Composite index on `(Mark, Age)`\
 C. Unique index on Name\
 D. Index on an unrelated column

 **Answer: B**

 **Explanation:** A composite index can be useful when queries commonly use multiple columns together.

---

 ### 65\. A developer writes:

```
UPDATE students
SET Mark = 100;
```

 What is the likely result?

 A. Only Zack is updated\
 B. Only the first student is updated\
 C. Every student's mark becomes 100\
 D. Nothing happens

 **Answer: C**

 **Explanation:** There is no `WHERE` clause, so the `UPDATE` applies to **every row**.

---

 ### 66\. A developer writes:

```
DELETE FROM students
WHERE Age = 32;
```

 Which students are deleted from the supplied table?

 A. John and Bob\
 B. John and Zack\
 C. Alex and Tom\
 D. Jones and Max

 **Answer: A**

 **Explanation:** John and Bob both have an age of **32**.

---

 ### 67\. Which query returns the highest mark first?

 A.

```
SELECT * FROM students
ORDER BY Mark ASC;
```

 B.

```
SELECT * FROM students
ORDER BY Mark DESC;
```

 C.

```
SELECT * FROM students
GROUP BY Mark;
```

 D.

```
SELECT MAX(*) FROM students;
```

 **Answer: B**

 **Explanation:** `DESC` orders the results from highest to lowest.

---

 ### 68\. Which student has the highest mark?

 A. Zack\
 B. Ron\
 C. Bob\
 D. Alex

 **Answer: A**

 **Explanation:** The marks are:

 - Zack = 90
- Bob = 89
- Ron = 87
- Alex = 32

 Therefore, **Zack** has the highest mark.

---

 ### 69\. Which student has the lowest mark?

 A. Max\
 B. John\
 C. Jones\
 D. Tom

 **Answer: C**

 **Explanation:** Jones has a mark of **5**, the lowest value in the table.

---

 ### 70\. Which student is the oldest?

 A. Zack\
 B. Alex\
 C. Max\
 D. Bob

 **Answer: C**

 **Explanation:** Max is **48**, which is the highest age in the table.

---

 ### 71\. Which student is the youngest?

 A. Tom\
 B. Jones\
 C. John\
 D. Alex

 **Answer: A**

 **Explanation:** Tom is **23**, the lowest age in the table.

---

 ### 72\. If you want the number of students in each age group, which query concept should you use?

 A. `GROUP BY Age` with `COUNT(*)`\
 B. `ORDER BY Age` only\
 C. `DELETE Age`\
 D. `CREATE INDEX Age`

 **Answer: A**

 **Explanation:** `GROUP BY Age` creates age groups, while `COUNT(*)` counts students within each group.

---

 ### 73\. Which statement best describes the relationship between an index and the original table?

 A. The index completely replaces the table\
 B. The index is an additional structure that helps locate table records\
 C. The index contains only deleted records\
 D. The index is another name for a row

 **Answer: B**

 **Explanation:** An index provides an additional lookup structure containing indexed values and references to corresponding table records.

---

 ## Section L — Higher-Level Understanding

 ### 74\. Which sequence best represents a typical SQL data-analysis process?

 A. Delete → Drop → Create → Stop\
 B. Select → Filter/Group → Aggregate/Analyze → Return results\
 C. Index → Delete → Insert → Drop\
 D. Update → Shutdown → Join

 **Answer: B**

 **Explanation:** A typical analytical query retrieves data, filters or groups it, performs calculations, and returns the results.

---

 ### 75\. Which statement about `WHERE` is correct?

 A. It can only be used with `DELETE`\
 B. It filters rows according to conditions\
 C. It always creates groups\
 D. It creates an index

 **Answer: B**

 **Explanation:** `WHERE` specifies which individual records should participate in the query.

---

 ### 76\. Which statement about `HAVING` is correct?

 A. It is mainly used to filter groups created through aggregation\
 B. It always replaces `WHERE`\
 C. It creates a primary key\
 D. It creates a B-tree

 **Answer: A**

 **Explanation:** `HAVING` is designed to filter grouped/aggregated results.

---

 ### 77\. Which statement best describes JOINs?

 A. They combine data from related tables\
 B. They always delete duplicate records\
 C. They create indexes automatically\
 D. They replace primary keys

 **Answer: A**

 **Explanation:** JOINs allow information from different but related tables to be queried together.

---

 ### 78\. Which statement best describes a primary key?

 A. A performance-only structure\
 B. A unique identifier for records\
 C. A method of sorting records\
 D. A command used to retrieve data

 **Answer: B**

 **Explanation:** A primary key uniquely identifies each row in a table.

---

 ### 79\. Which statement best describes a B-tree index?

 A. It is a type of SQL query\
 B. It is a structured lookup mechanism for efficient searching\
 C. It is a database table containing all records\
 D. It is a replacement for SQL

 **Answer: B**

 **Explanation:** A B-tree index organizes indexed values into a tree structure that supports efficient lookup.

---

 ### 80\. Which statement summarizes the main purpose of database indexes?

 A. To make every database operation slower\
 B. To improve data retrieval performance\
 C. To eliminate tables\
 D. To replace relational database design

 **Answer: B**

 **Explanation:** Indexes primarily improve the efficiency of finding and retrieving records, although they require additional storage and maintenance.

---

 # Quick Answer Key

 | Q | Ans | Q | Ans | Q | Ans | Q | Ans |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | A | 21 | B | 41 | B | 61 | A |
| 2 | B | 22 | A | 42 | C | 62 | A |
| 3 | C | 23 | B | 43 | B | 63 | C |
| 4 | C | 24 | B | 44 | B | 64 | B |
| 5 | A | 25 | B | 45 | B | 65 | C |
| 6 | A | 26 | A | 46 | B | 66 | A |
| 7 | A | 27 | B | 47 | A | 67 | B |
| 8 | B | 28 | C | 48 | B | 68 | A |
| 9 | A | 29 | B | 49 | B | 69 | C |
| 10 | B | 30 | C | 50 | B | 70 | C |
| 11 | C | 31 | B | 51 | A | 71 | A |
| 12 | B | 32 | B | 52 | A | 72 | A |
| 13 | C | 33 | A | 53 | A | 73 | B |
| 14 | B | 34 | B\* | 54 | B | 74 | B |
| 15 | C | 35 | C | 55 | B | 75 | B |
| 16 | C | 36 | A | 56 | A | 76 | A |
| 17 | A | 37 | B | 57 | A | 77 | A |
| 18 | A | 38 | B | 58 | B | 78 | B |
| 19 | A | 39 | A | 59 | A | 79 | B |
| 20 | A | 40 | C | 60 | B | 80 | B |

\* **Q34 correction:** The listed options do not contain the correct average. The actual average mark is **48**.

 ## Highest-Priority Questions to Master

 If this is for an exam, make sure you can answer these without hesitation:

 1. **CRUD** → Create, Read, Update, Delete.
2. `SELECT` → retrieves data.
3. `INSERT` → adds data.
4. `UPDATE` → modifies existing data.
5. `DELETE` → removes rows.
6. `WHERE` → filters rows.
7. `ORDER BY` → sorts results.
8. `GROUP BY` → creates groups.
9. `HAVING` → filters groups.
10. `JOIN` → combines related tables.
11. **Primary key** → uniquely identifies a record.
12. **B-tree index** → organized lookup structure for faster searching.
13. **Single-column index** → one column.
14. **Composite index** → multiple columns.
15. **Unique index** → indexed values must be unique.
16. Indexes can improve `SELECT` performance but require **additional storage and maintenance**.
