# SQL & Relational Databases — Exam Questions with Answers

 ## Section A — Fundamentals

 ### 1\. What is SQL?

 **Answer:**\
 SQL (Structured Query Language) is the standard language used to create, access, manipulate, and manage data stored in relational databases.

---

 ### 2\. What is a relational database?

 **Answer:**\
 A relational database stores data in structured tables consisting of rows and columns. Tables can be related to one another using common fields or keys.

---

 ### 3\. What is a table?

 **Answer:**\
 A table is a collection of related data organized into rows and columns.

 For example:

 | Name | Mark | Age |
| --- | --- | --- |
| John | 24 | 32 |
| Alex | 32 | 45 |

Here, `students` could be the name of the table.

---

 ### 4\. What is a row in a relational database?

 **Answer:**\
 A row represents one complete record or entity in a table.

 For example:

```
John | 24 | 32
```

 represents one student record.

---

 ### 5\. What is a column?

 **Answer:**\
 A column represents a particular attribute or field of the records in a table.

 For example:

```
Name
Mark
Age
```

 are columns in a student table.

---

 ### 6\. What does CRUD mean?

 **Answer:**

 CRUD stands for:

 - **C — Create:** Add new data.
- **R — Read:** Retrieve data.
- **U — Update:** Modify existing data.
- **D — Delete:** Remove data.

 In SQL, these are commonly performed using `INSERT`, `SELECT`, `UPDATE`, and `DELETE`.

---

 ## Section B — SQL CRUD Operations

 Assume the following table:

```
students(Name, Mark, Age)
```

 with the following data:

 | Name | Mark | Age |
| --- | --- | --- |
| Jones | 5 | 28 |
| Max | 20 | 48 |
| John | 24 | 32 |
| Alex | 32 | 45 |
| Tom | 37 | 23 |
| Ron | 87 | 33 |
| Bob | 89 | 32 |
| Zack | 90 | 34 |

---

 ### 7\. Write an SQL statement to retrieve all students.

 **Answer:**

```
SELECT * FROM students;
```

 `*` means that all columns should be returned.

---

 ### 8\. Write an SQL statement to retrieve only the names of all students.

 **Answer:**

```
SELECT Name
FROM students;
```

---

 ### 9\. Write an SQL statement to retrieve the name and mark of every student.

 **Answer:**

```
SELECT Name, Mark
FROM students;
```

---

 ### 10\. Write an SQL statement to insert a new student named Sarah, with a mark of 75 and age 27.

 **Answer:**

```
INSERT INTO students (Name, Mark, Age)
VALUES ('Sarah', 75, 27);
```

---

 ### 11. Write an SQL statement to change John's mark from 24 to 30.

 **Answer:**

```
UPDATE students
SET Mark = 30
WHERE Name = 'John';
```

 The `WHERE` clause ensures that only John is updated.

---

 ### 12\. What could happen if you execute the following statement?

```
UPDATE students
SET Mark = 30;
```

 **Answer:**\
 The mark of **every student** in the table would be changed to 30 because there is no `WHERE` clause restricting the update.

---

 ### 13\. Write an SQL statement to delete Max from the table.

 **Answer:**

```
DELETE FROM students
WHERE Name = 'Max';
```

---

 ### 14\. What could happen if you execute this statement?

```
DELETE FROM students;
```

 **Answer:**\
 All records in the `students` table would be deleted because there is no `WHERE` condition.

---

 ## Section C — Filtering Data

 ### 15\. What is the purpose of the `WHERE` clause?

 **Answer:**\
 The `WHERE` clause filters records and returns or modifies only those records that satisfy a specified condition.

 Example:

```
SELECT *
FROM students
WHERE Mark > 50;
```

 This returns only students whose mark is greater than 50.

---

 ### 16\. Write an SQL query to find students with marks greater than 80.

 **Answer:**

```
SELECT *
FROM students
WHERE Mark > 80;
```

 The original data returns:

 | Name | Mark | Age |
| --- | --- | --- |
| Ron | 87 | 33 |
| Bob | 89 | 32 |
| Zack | 90 | 34 |

---

 ### 17\. Write an SQL query to find students whose age is 32.

 **Answer:**

```
SELECT *
FROM students
WHERE Age = 32;
```

 **Result:**

 | Name | Mark | Age |
| --- | --- | --- |
| John | 24 | 32 |
| Bob | 89 | 32 |

---

 ### 18\. Write an SQL query to find students whose mark is between 20 and 50.

 **Answer:**

```
SELECT *
FROM students
WHERE Mark BETWEEN 20 AND 50;
```

 The result includes:

 - Max — 20
- John — 24
- Alex — 32
- Tom — 37

---

 ### 19\. Write an SQL query to find students whose mark is less than 30.

 **Answer:**

```
SELECT *
FROM students
WHERE Mark < 30;
```

---

 ## Section D — Sorting and Aggregate Functions

 ### 20\. Write an SQL query to display students from highest mark to lowest mark.

 **Answer:**

```
SELECT *
FROM students
ORDER BY Mark DESC;
```

 `DESC` means descending order.

---

 ### 21\. Write an SQL query to display students from lowest mark to highest mark.

 **Answer:**

```
SELECT *
FROM students
ORDER BY Mark ASC;
```

---

 ### 22\. What is an aggregate function?

 **Answer:**\
 An aggregate function performs a calculation on multiple rows and returns a single result.

 Common aggregate functions include:

 - `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`

---

 ### 23\. Write an SQL query to find the highest mark.

 **Answer:**

```
SELECT MAX(Mark)
FROM students;
```

 **Answer from the given data:** `90`

---

 ### 24\. Write an SQL query to find the lowest mark.

 **Answer:**

```
SELECT MIN(Mark)
FROM students;
```

 **Answer:** `5`

---

 ### 25\. Write an SQL query to calculate the average mark.

 **Answer:**

```
SELECT AVG(Mark)
FROM students;
```

 For the eight students:

```
5 + 20 + 24 + 32 + 37 + 87 + 89 + 90 = 384
```

 Therefore:

```
384 / 8 = 48
```

 **Average mark = 48**

---

 ### 26\. Write an SQL query to count the number of students.

 **Answer:**

```
SELECT COUNT(*)
FROM students;
```

 **Answer:** `8`

---

 ### 27\. Write an SQL query to calculate the average age.

 **Answer:**

```
SELECT AVG(Age)
FROM students;
```

 The ages are:

```
28 + 48 + 32 + 45 + 23 + 33 + 32 + 34 = 275
```

 Therefore:

```
275 / 8 = 34.375
```

 **Average age = 34.375**

---

 ## Section E — GROUP BY and HAVING

 ### 28\. What is the purpose of `GROUP BY`?

 **Answer:**\
 `GROUP BY` groups rows that have the same value in one or more columns. It is commonly used with aggregate functions such as `COUNT()`, `AVG()`, `SUM()`, `MIN()`, and `MAX()`.

---

 ### 29\. Write an SQL query that groups students by age.

 **Answer:**

```
SELECT Age, COUNT(*)
FROM students
GROUP BY Age;
```

 This counts how many students belong to each age group.

---

 ### 30\. Based on the given data, which age occurs more than once?

 **Answer:**\
 Age **32** occurs twice:

 - John — 32
- Bob — 32

 All other ages occur once.

---

 ### 31\. Write an SQL query to calculate the average mark for each age.

 **Answer:**

```
SELECT Age, AVG(Mark)
FROM students
GROUP BY Age;
```

---

 ### 32\. What is the purpose of the `HAVING` clause?

 **Answer:**\
 `HAVING` filters groups produced by `GROUP BY`.

 For example:

```
SELECT Age, COUNT(*)
FROM students
GROUP BY Age
HAVING COUNT(*) > 1;
```

 This returns only age groups containing more than one student.

---

 ### 33\. Explain the difference between `WHERE` and `HAVING`.

 **Answer:**

 | `WHERE` | `HAVING` |
| --- | --- |
| Filters individual rows | Filters groups |
| Usually applied before grouping | Applied after grouping |
| Can be used without `GROUP BY` | Commonly used with `GROUP BY` |
| Example: `WHERE Mark > 50` | Example: `HAVING AVG(Mark) > 50` |

---

 ### 34\. Explain this query:

```
SELECT Age, AVG(Mark)
FROM students
GROUP BY Age
HAVING AVG(Mark) > 50;
```

 **Answer:**

 - `SELECT Age, AVG(Mark)` selects the age and average mark.
- `FROM students` specifies the source table.
- `GROUP BY Age` creates a group for each age.
- `AVG(Mark)` calculates the average mark within each age group.
- `HAVING AVG(Mark) > 50` keeps only groups whose average mark exceeds 50.

---

 ## Section F — JOINs

 ### 35\. What is a JOIN?

 **Answer:**\
 A JOIN combines related data from two or more tables using a related column.

---

 ### 36\. Why are JOINs useful?

 **Answer:**\
 JOINs allow information stored in separate relational tables to be combined when querying the database.

 For example, student information could be stored in one table and course information in another.

---

 ### 37\. Consider these tables:

```
students
----------------
student_id
name
course_id
```

```
courses
----------------
course_id
course_name
```

 Write an SQL query that displays the student's name and course name.

 **Answer:**

```
SELECT students.name, courses.course_name
FROM students
INNER JOIN courses
ON students.course_id = courses.course_id;
```

---

 ### 38\. What is the purpose of the `ON` clause in a JOIN?

 **Answer:**\
 The `ON` clause specifies the condition that determines how records from the two tables are related.

 For example:

```
ON students.course_id = courses.course_id
```

 means that a student's `course_id` is matched with the corresponding course's `course_id`.

---

 ### 39\. What is an INNER JOIN?

 **Answer:**\
 An `INNER JOIN` returns only records where a matching record exists in both tables.

---

 ### 40\. What happens if a student has no matching course when using an INNER JOIN?

 **Answer:**\
 That student's record will not appear in the result because an `INNER JOIN` returns only matching records.

---

 ## Section G — Database Indexes

 ### 41\. What is a database index?

 **Answer:**\
 An index is a database structure used to make searching and retrieving records more efficient.

 It can reduce the amount of data the database needs to examine when executing certain queries.

---

 ### 42\. What is a B-tree index?

 **Answer:**\
 A B-tree index is a tree-based indexing structure that organizes indexed values so that the database can efficiently search, locate, and retrieve records.

---

 ### 43\. What is a single-column index?

 **Answer:**\
 A single-column index is an index created using one column of a table.

 Example:

```
CREATE INDEX index_name
ON table_name (column_name);
```

---

 ### 44\. Create an index on the `Mark` column of the `students` table.

 **Answer:**

```
CREATE INDEX idx_students_mark
ON students(Mark);
```

---

 ### 45\. What is a unique index?

 **Answer:**\
 A unique index requires the indexed values to be unique. Duplicate values are not permitted for the indexed attribute.

 Example:

```
CREATE UNIQUE INDEX index_name
ON table_name(column_name);
```

---

 ### 46\. Create a unique index on the `Name` column.

 **Answer:**

```
CREATE UNIQUE INDEX idx_students_name
ON students(Name);
```

 This would prevent two records from having the same indexed name.

---

 ### 47\. What is a composite index?

 **Answer:**\
 A composite index is an index created using multiple columns.

 For example:

```
CREATE INDEX idx_students_mark_age
ON students(Mark, Age);
```

 Here, both `Mark` and `Age` form the index.

---

 ### 48\. Write the general syntax for creating a composite index.

 **Answer:**

```
CREATE INDEX index_name
ON table_name (column1, column2);
```

---

 ### 49\. Explain the difference between these two indexes.

```
CREATE INDEX idx1
ON students(Mark);
```

 and

```
CREATE INDEX idx2
ON students(Mark, Age);
```

 **Answer:**

 The first is a **single-column index** because it uses only `Mark`.

 The second is a **composite index** because it uses both `Mark` and `Age`.

---

 ### 50\. Why can indexes improve query performance?

 **Answer:**\
 Indexes provide an organized structure that allows the database to locate relevant records more efficiently instead of having to examine every row in the table.

 Therefore, indexes can reduce search time, particularly for frequently queried columns.

---

 ### 51\. Why should indexes not be created on every column?

 **Answer:**\
 Indexes require additional storage and must be maintained when data is inserted, updated, or deleted. Too many indexes can therefore increase storage requirements and slow down data modification operations.

---

 ## Section H — Practical SQL Exam

 ### 52\. Write SQL to create the `students` table.

 **Answer:**

 One possible implementation is:

```
CREATE TABLE students (
    Name VARCHAR(50),
    Mark INT,
    Age INT
);
```

---

 ### 53\. Write SQL to insert the eight students from the dataset.

 **Answer:**

```
INSERT INTO students (Name, Mark, Age)
VALUES
('Jones', 5, 28),
('Max', 20, 48),
('John', 24, 32),
('Alex', 32, 45),
('Tom', 37, 23),
('Ron', 87, 33),
('Bob', 89, 32),
('Zack', 90, 34);
```

---

 ### 54. Write a query to find the student with the highest mark.

 **Answer:**

```
SELECT *
FROM students
WHERE Mark = (SELECT MAX(Mark) FROM students);
```

 **Result:**

```
Zack | 90 | 34
```

---

 ### 55\. Write a query to find students whose mark is greater than the average mark.

 **Answer:**

```
SELECT *
FROM students
WHERE Mark > (SELECT AVG(Mark) FROM students);
```

 The average mark is `48`, so the students are:

 - Ron — 87
- Bob — 89
- Zack — 90

---

 ### 56\. Write a query to find the number of students whose mark is greater than 50.

 **Answer:**

```
SELECT COUNT(*)
FROM students
WHERE Mark > 50;
```

 **Result:** `3`

---

 ### 57\. Write a query to display the oldest student.

 **Answer:**

```
SELECT *
FROM students
WHERE Age = (SELECT MAX(Age) FROM students);
```

 **Result:**

```
Max | 20 | 48
```

---

 ### 58\. Write a query to display the youngest student.

 **Answer:**

```
SELECT *
FROM students
WHERE Age = (SELECT MIN(Age) FROM students);
```

 **Result:**

```
Tom | 37 | 23
```

---

 ## Section I — Applied/Scenario Questions

 ### 59\. A database contains millions of student records. Users frequently search for students based on their marks. What would you recommend?

 **Answer:**\
 Create an index on the `Mark` column:

```
CREATE INDEX idx_students_mark
ON students(Mark);
```

 This can improve the performance of queries that search or sort using `Mark`.

---

 ### 60\. A school wants to ensure that every student's name is unique. What type of index could be used?

 **Answer:**\
 A **unique index** could be used:

```
CREATE UNIQUE INDEX idx_students_name
ON students(Name);
```

 This prevents duplicate values in the indexed column.

---

 ### 61\. A school frequently searches students using both their mark and age. What type of index would be appropriate?

 **Answer:**\
 A **composite index** would be appropriate:

```
CREATE INDEX idx_students_mark_age
ON students(Mark, Age);
```

---

 ### 62\. A database stores student and course information in separate tables. Explain why this design can be useful.

 **Answer:**\
 Separating information into related tables reduces unnecessary duplication and organizes the database logically. JOINs can then be used to combine related information when it is needed.

---

 ### 63\. A teacher wants to know the average mark for each age group, but only wants to see groups whose average mark is above 50. Write the SQL query.

 **Answer:**

```
SELECT Age, AVG(Mark) AS AverageMark
FROM students
GROUP BY Age
HAVING AVG(Mark) > 50;
```

---

 ## Section J — Long-Answer Exam Questions

 ### 64\. Explain CRUD operations with SQL examples.

 **Answer:**

 CRUD represents the four fundamental operations performed on data.

 **Create:**

```
INSERT INTO students (Name, Mark, Age)
VALUES ('Sarah', 75, 27);
```

 This adds a new record.

 **Read:**

```
SELECT *
FROM students;
```

 This retrieves data.

 **Update:**

```
UPDATE students
SET Mark = 80
WHERE Name = 'Sarah';
```

 This modifies existing data.

 **Delete:**

```
DELETE FROM students
WHERE Name = 'Sarah';
```

 This removes a record.

 Together, these operations allow users and applications to manage the data stored in a relational database.

---

 ### 65\. Explain `GROUP BY` and `HAVING` with an example.

 **Answer:**

 `GROUP BY` combines rows with the same value into groups. It is particularly useful with aggregate functions.

 For example:

```
SELECT Age, AVG(Mark)
FROM students
GROUP BY Age;
```

 This calculates the average mark for every age group.

 `HAVING` is then used to filter the resulting groups:

```
SELECT Age, AVG(Mark)
FROM students
GROUP BY Age
HAVING AVG(Mark) > 50;
```

 The query returns only age groups whose average mark is greater than 50.

 The key distinction is that `WHERE` filters individual rows, while `HAVING` filters groups.

---

 ### 66\. Explain JOINs and their importance in relational databases.

 **Answer:**

 A relational database often divides information into multiple tables. For example, student information can be stored in a `students` table while course information can be stored in a `courses` table.

 A JOIN allows related records from these tables to be combined.

 For example:

```
SELECT students.Name, courses.course_name
FROM students
INNER JOIN courses
ON students.course_id = courses.course_id;
```

 The `ON` condition establishes the relationship between the tables. An `INNER JOIN` returns only records where matching values exist in both tables.

 JOINs therefore allow normalized, related data to be retrieved together when necessary.

---

 ### 67\. Explain database indexing and the three index types covered.

 **Answer:**

 An index is a database structure designed to make data retrieval more efficient. Instead of searching through every row, the database can use the index to locate relevant records more quickly.

 The material covers three main types:

 **1\. Single-column index**

 An index based on one column:

```
CREATE INDEX idx_students_mark
ON students(Mark);
```

 **2\. Unique index**

 An index where the significantly improve query performance, but they require additional storage and maintenance. Therefore, they should be created strategically rather than on every indexed values must be unique:

```
CREATE UNIQUE INDEX idx_students_name
ON students(Name);
```

 **3\. Composite index**

 An index involving multiple columns:

```
CREATE INDEX idx_students_mark_age
ON students(Mark, Age);
```

 Indexes can significantly improve query performance, but they require additional storage and maintenance. Therefore, they should be created strategically rather than on every column.

---

 ### 68\. Comprehensive Database Question

 **Question:**\
 Explain how SQL can be used to manage a student database from creation through optimization.

 **Answer:**

 First, a table can be created:

```
CREATE TABLE students (
    Name VARCHAR(50),
    Mark INT,
    Age INT
);
```

 Data can then be inserted:

```
INSERT INTO students (Name, Mark, Age)
VALUES ('John', 24, 32);
```

 Data can be retrieved using `SELECT`:

```
SELECT *
FROM students;
```

 Specific records can be filtered using `WHERE`:

```
SELECT *
FROM students
WHERE Mark > 50;
```

 Existing records can be modified using `UPDATE`:

```
UPDATE students
SET Mark = 30
WHERE Name = 'John';
```

 Records can be removed using `DELETE`:

```
DELETE FROM students
WHERE Name = 'John';
```

 Aggregate functions can analyze the data:

```
SELECT AVG(Mark)
FROM students;
```

 `GROUP BY` can organize records into groups:

```
SELECT Age, AVG(Mark)
FROM students
GROUP BY Age;
```

 `HAVING` can filter those groups:

```
SELECT Age, AVG(Mark)
FROM students
GROUP BY Age
HAVING AVG(Mark) > 50;
```

 JOINs can combine information from related tables.

 Finally, indexes can improve the performance of frequently executed queries:

```
CREATE INDEX idx_students_mark
ON students(Mark);
```

 Thus, SQL supports the complete lifecycle of relational data management: **storing, retrieving, modifying, analyzing, relating, and efficiently accessing data**.

---

 # High-Priority Questions to Study

 If this is for an exam, make sure you can confidently answer these without looking at your notes:

 1. **What is SQL?**
2. **What is a relational database?**
3. **Explain CRUD.**
4. **Write `SELECT`, `INSERT`, `UPDATE`, and `DELETE` statements.**
5. **Explain and use `WHERE`.**
6. **Explain aggregate functions.**
7. **Use `GROUP BY`.**
8. **Explain the difference between `WHERE` and `HAVING`.**
9. **Write queries using `GROUP BY` and `HAVING`.**
10. **Explain JOINs and write an `INNER JOIN`.**
11. **Explain what an index does.**
12. **Know single-column, unique, and composite indexes.**
13. **Write `CREATE INDEX` statements.**
14. **Explain B-tree indexes.**
15. **Understand why indexes improve search performance but also have costs.**
