Below is a GitHub-ready Markdown version of the three exams, organized as a single `README.md` file. It preserves the questions, SQL examples, mark allocations, and answer keys, while correcting formatting issues such as broken code fences, mathematical notation, tables, and incomplete sections.

I’ve also completed the truncated final answer for Mock Exam 3, Question 14, and included the remaining marking guidance and a consolidated exam overview.

Suggested filename: `README.md`

Writing

# Data Management 2026–2027

## Mock Examination Collection: T0–T2

This document contains three university-style mock examinations covering foundational concepts, applied database problems, and comprehensive challenges.

## Table of Contents

- Mock Exam 1 — Core Concepts
- Mock Exam 1 — Answer Key
- Mock Exam 2 — Applied Problems
- Mock Exam 2 — Answer Key
- Mock Exam 3 — Comprehensive Challenge
- Mock Exam 3 — Answer Key

---

# Mock Exam 1 — Core Concepts

**Level:** Foundation\
**Duration:** 60 minutes\
**Total marks:** 50 points\
**Coverage:** T0 foundations, T1 data models and storage, T2 SQL

## Instructions

- Answer every question.
- Choose one answer for each multiple-choice question.
- Write SQL statements using standard SQL where possible. State any assumptions.
- Marks are awarded for correct reasoning as well as final answers.
- The answer key follows the entire exam. Attempt the paper before reading it.

## Section A — Multiple Choice (10 points)

_10 questions × 1 point._

### Q1. Reliability

Which property describes the ability of a system to continue operating correctly and recover from failures?

A. Maintainability\
B. Reliability\
C. Throughput\
D. Normalization

### Q2. Performance metrics

A service processes 8,000 requests per second. Which performance metric does this describe?

A. Latency\
B. Response time\
C. Throughput\
D. Availability alone

### Q3. Data modeling

Which model specifies database-specific column types, indexes, and storage implementation?

A. Conceptual\
B. Logical\
C. Physical\
D. Entity-relationship

### Q4. Keys and referential integrity

Which statement about primary and foreign keys is correct?

A. A foreign key must always be unique.\
B. A primary key may contain `NULL` values.\
C. A foreign key establishes a reference to an eligible key in another or the same table.\
D. A primary key is used only to sort rows.

### Q5. Many-to-many relationships

A relationship connects many students to many courses. What is the standard relational solution?

A. A single Student table with a text field containing every course\
B. A junction table linking Student and Course\
C. A foreign key only in Student, with no additional structure\
D. A separate database for every student

### Q6. Normalization

Which normal form directly addresses partial dependencies on a proper subset of a candidate key?

A. 1NF\
B. 2NF\
C. 3NF\
D. ACID

### Q7. SQL commands

Which SQL command removes the table structure as well as its stored data?

A. `DELETE`\
B. `UPDATE`\
C. `DROP TABLE`\
D. `SELECT`

### Q8. Aggregation

Which clause filters groups after aggregation?

A. `WHERE`\
B. `HAVING`\
C. `ORDER BY`\
D. `FROM`

### Q9. Joins

Which join returns every row from the left table, including rows without a match on the right?

A. `INNER JOIN`\
B. `RIGHT JOIN`\
C. `LEFT JOIN`\
D. `CROSS JOIN`

### Q10. Indexes

What is a principal trade-off of adding database indexes?

A. Indexes always improve every query.\
B. Indexes eliminate the need for primary keys.\
C. Indexes can improve suitable reads but consume storage and increase write overhead.\
D. Indexes prevent concurrent transactions.

## Section B — Short-Answer Questions (12 points)

Answer in clear, concise sentences.

### Q11. Reliability, scalability, and maintainability (3 points)

Define each of the following and give one example of how it matters in a data-intensive application:

1. Reliability
2. Scalability
3. Maintainability

### Q12. Modeling levels (3 points)

A university wants to build a database to manage students and course registrations.

Explain what would be specified at the conceptual, logical, and physical modeling levels. Give one example of a design decision at each level.

### Q13. ACID properties (3 points)

A transaction transfers €200 between two bank accounts. Explain how atomicity, consistency, and durability apply to this transaction.

### Q14. Normalization (3 points)

Consider the following functional dependencies:

```
EmployeeID → DepartmentID
DepartmentID → DepartmentName
```

Explain why storing `DepartmentName` in every employee record can cause a data anomaly. Describe a suitable decomposition.

## Section C — SQL Fundamentals (12 points)

Consider the following schema:

```
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(80) NOT NULL,
    city VARCHAR(60),
    age INT
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL
);

CREATE TABLE Enrolments (
    student_id INT,
    course_id INT,
    mark DECIMAL(5,2),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
```

### Q15. Basic retrieval (2 points)

Write a query to retrieve the names and ages of students who are at least 18 years old, ordered by age from oldest to youngest.

### Q16. Aggregation (3 points)

Write a query that returns each `course_id` and the average mark for that course. Only include courses whose average mark is at least 60.

### Q17. Joins (3 points)

Write a query to display every student's name and their course IDs. Students who have no enrolments must also appear.

### Q18. Modification and safety (2 points)

Write an SQL statement that changes the city of the student with `student_id = 12` to Madrid.

Then explain what would happen if the `WHERE` clause were omitted.

### Q19. SQL command categories (2 points)

Classify each command as DDL, DQL, DML, DCL, or TCL:

- `CREATE TABLE`
- `SELECT`
- `UPDATE`
- `COMMIT`

## Section D — Database Design and System Reasoning (16 points)

### Q20. Cardinality and schema design (4 points)

An online shop has customers, orders, and products.

- A customer can place multiple orders.
- Each order belongs to one customer.
- Each order can contain multiple products.
- A product can appear in multiple orders.

Identify the cardinality of each relationship and propose the required tables and foreign keys. Explain how you would represent the order–product relationship.

### Q21. Schema selection (3 points)

Choose the most appropriate model for each situation and justify your choices.

1. A business intelligence system analyzes sales by product, region, and month.
2. An organization stores a parent-child hierarchy in which each child has one parent.
3. A reporting system has a central sales fact table and dimension tables that are further normalized into related tables.

Choose from: **hierarchical, relational, star, snowflake**.

### Q22. Batch versus stream processing (3 points)

A company has two workloads:

- **Workload A:** Calculates the previous day's sales totals every night.
- **Workload B:** Detects suspicious payments as transactions arrive.

For each workload, recommend batch or stream processing and explain why.

### Q23. Indexes and performance (3 points)

A query frequently searches for students by city and then filters by age:

```
SELECT *
FROM Students
WHERE city = 'Madrid'
  AND age >= 20;
```

Recommend an index. Explain one potential benefit and one cost of maintaining it.

### Q24. Faults, errors, and failures (3 points)

A database server has a faulty disk. The disk causes a read operation to return corrupted data, and the application consequently returns an incorrect result to the user.

Identify the fault, the error, and the failure in this scenario.

---

# Mock Exam 1 — Answer Key and Marking Guide

> **Stop here if you have not yet attempted the exam.**

## Section A — Answers

| Question | Answer | Explanation |
| --- | --- | --- |
| Q1 | B | Reliability concerns correct operation and recovery from failures. |
| Q2 | C | Throughput measures operations processed per unit of time. |
| Q3 | C | Physical modeling defines implementation details. |
| Q4 | C | Foreign keys reference eligible keys and support referential integrity. |
| Q5 | B | A junction table represents many-to-many associations. |
| Q6 | B | 2NF addresses partial dependencies. |
| Q7 | C | `DROP TABLE` removes the table definition and its data. |
| Q8 | B | `HAVING` filters groups after aggregation. |
| Q9 | C | `LEFT JOIN` preserves all left-side rows. |
| Q10 | C | Indexes trade extra storage and write maintenance for potentially faster reads. |

## Section B — Model Answers

### Q11. Reliability, scalability, and maintainability (3 points)

- **Reliability:** The system operates correctly and recovers from failures. Example: recovering committed records after a server crash.
- **Scalability:** The system can handle increasing workloads or data volumes. Example: adding capacity when the number of users increases.
- **Maintainability:** The system can be understood, modified, and operated efficiently. Example: clear schemas and documented database procedures make changes safer.

**Marking:** Award 1 point for each correct definition with a relevant example.

### Q12. Modeling levels (3 points)

- **Conceptual:** Identifies business entities and relationships, such as Students enrolling in Courses.
- **Logical:** Specifies attributes, keys, and relationships, such as `Students(student_id, name)` and `Enrolments(student_id, course_id)`.
- **Physical:** Specifies DBMS-specific types, indexes, and implementation, such as an integer primary key and an index on a frequently searched field.

**Marking:** Award 1 point per level with an appropriate example.

### Q13. ACID properties (3 points)

- **Atomicity:** Both the debit and credit succeed, or neither is committed.
- **Consistency:** Database constraints and business rules remain satisfied.
- **Durability:** Once the transaction commits, its changes persist through subsequent failures.

**Marking:** Award 1 point per correctly explained property.

### Q14. Normalization (3 points)

`DepartmentName` depends on `DepartmentID`, which in turn is determined by `EmployeeID`. Storing the department name repeatedly creates a transitive dependency.

Changing a department name requires multiple employee records to be updated, potentially producing inconsistent values. This is an update anomaly.

A suitable decomposition is:

```
Employee(
    EmployeeID PK,
    DepartmentID FK
)

Department(
    DepartmentID PK,
    DepartmentName
)
```

**Marking:**

- 1 point for identifying the dependency.
- 1 point for explaining the anomaly.
- 1 point for the decomposition.

## Section C — Model Answers

### Q15. Basic retrieval (2 points)

```
SELECT name, age
FROM Students
WHERE age >= 18
ORDER BY age DESC;
```

**Marking:**

- 1 point for selecting and filtering the correct columns and rows.
- 1 point for descending age order.

### Q16. Aggregation (3 points)

```
SELECT course_id, AVG(mark) AS average_mark
FROM Enrolments
GROUP BY course_id
HAVING AVG(mark) >= 60;
```

- `AVG(mark)` calculates the mean.
- `GROUP BY course_id` calculates a separate mean for each course.
- `HAVING` filters the calculated averages.

**Marking:** Award 1 point each for correct grouping, aggregation, and group filtering.

### Q17. Joins (3 points)

```
SELECT s.name, e.course_id
FROM Students AS s
LEFT JOIN Enrolments AS e
    ON s.student_id = e.student_id;
```

`LEFT JOIN` preserves students without enrolments. Their `course_id` will be `NULL`.

**Marking:**

- 1 point for the join.
- 1 point for the matching condition.
- 1 point for preserving unmatched students.

### Q18. Modification and safety (2 points)

```
UPDATE Students
SET city = 'Madrid'
WHERE student_id = 12;
```

Without `WHERE`, every row in `Students` would be updated to have `city = 'Madrid'`.

**Marking:**

- 1 point for correct SQL.
- 1 point for explaining the missing-filter consequence.

### Q19. SQL command categories (2 points)

- `CREATE TABLE` — DDL.
- `SELECT` — DQL.
- `UPDATE` — DML.
- `COMMIT` — TCL.

**Marking:** 0.5 point per correct classification.

## Section D — Model Answers

### Q20. Cardinality and schema design (4 points)

- **Customer–Order:** One-to-many.
- **Order–Product:** Many-to-many.

Suggested tables:

```
Customer(
    CustomerID PK,
    ...
)

Order(
    OrderID PK,
    CustomerID FK,
    ...
)

Product(
    ProductID PK,
    ...
)

OrderItem(
    OrderID PK, FK,
    ProductID PK, FK,
    Quantity,
    UnitPrice
)
```

The `OrderItem` junction table represents the products within each order. A composite primary key `(OrderID, ProductID)` prevents duplicate product entries within the same order, assuming one line per product. If multiple lines for the same product are allowed, use a line identifier instead.

**Marking:**

- 1 point for each correct cardinality.
- 1 point for the customer foreign key.
- 1 point for the junction-table design.

### Q21. Schema selection (3 points)

1. **Star:** A fact table connects directly to sales dimensions, supporting analytical queries with a relatively simple structure.
2. **Hierarchical:** A tree naturally represents parent-child relationships where each child has one parent.
3. **Snowflake:** Dimensions are normalized into additional related tables, reducing dimension-data redundancy at the cost of more joins.

**Marking:** 1 point each.

### Q22. Batch versus stream processing (3 points)

- **Workload A — Batch:** The previous day's sales are aggregated periodically over a completed data set.
- **Workload B — Stream:** Incoming payments must be processed as events arrive to detect suspicious activity with low delay.

**Marking:** Award 1 point per correct choice and 0.5 point per suitable justification.

### Q23. Indexes and performance (3 points)

A reasonable index is:

```
CREATE INDEX idx_students_city_age
ON Students (city, age);
```

**Benefit:** The index can help locate rows matching the city and then the age range, potentially reducing scanning and sorting work.

**Cost:** It consumes additional storage and must be maintained when indexed values are inserted, updated, or deleted.

**Marking:**

- 1 point for the index definition or correct column order.
- 1 point for the benefit.
- 1 point for the cost.

### Q24. Faults, errors, and failures (3 points)

- **Fault:** The faulty disk.
- **Error:** The corrupted data read or resulting incorrect internal state.
- **Failure:** The application returns an incorrect result to the user.

**Marking:** 1 point per correct distinction.

---

# Mock Exam 2 — Applied Problems

**Level:** Application\
**Duration:** 75 minutes\
**Total marks:** 60 points\
**Focus:** Practical SQL, normalization, transaction reasoning, query behavior, and index analysis.

## Instructions

- Write executable SQL wherever requested.
- Explain important decisions instead of merely naming a technique.
- Assume standard SQL unless a DBMS is specified.
- If a question is ambiguous, state a reasonable assumption.

## Section A — Applied Multiple Choice (8 points)

_8 questions × 1 point._

### Q1. Latency

An application takes 1.8 seconds to return a query result. Which metric most directly describes this elapsed time?

A. Throughput\
B. Latency\
C. Availability\
D. Scalability

### Q2. Normalization

A table uses `(OrderID, ProductID)` as its composite primary key. Which statement best describes a violation of 2NF?

A. Quantity depends on the complete composite key.\
B. ProductName depends only on ProductID.\
C. Both columns are part of the primary key.\
D. Each row contains one quantity.

### Q3. Joins

A reporting query must include all departments, even those with no employees. Which approach is appropriate?

A. `INNER JOIN` Departments to Employees\
B. `LEFT JOIN` Departments to Employees\
C. `WHERE Employees.EmployeeID IS NOT NULL` after a left join\
D. `CROSS JOIN` Departments and Employees

### Q4. Aggregation

Which query returns departments with more than five employees?

A. `WHERE COUNT(*) > 5`\
B. `GROUP BY DepartmentID WHERE COUNT(*) > 5`\
C. `GROUP BY DepartmentID HAVING COUNT(*) > 5`\
D. `ORDER BY COUNT(*) > 5`

### Q5. Index maintenance

A frequently queried indexed column is updated thousands of times per second. What is a likely consequence of adding more indexes to the table?

A. Write operations may become more expensive.\
B. Writes will always become faster.\
C. Indexes require no maintenance.\
D. The table will no longer require storage.

### Q6. Processing models

A system must calculate the total sales for the previous month. Which processing approach is usually the simplest fit?

A. Stream processing is mandatory.\
B. Batch processing.\
C. A primary key alone.\
D. A B-tree page split.

### Q7. B+ trees

Which statement about a B+ tree index is generally correct?

A. It cannot support range queries.\
B. Its leaf nodes are commonly linked to support efficient range scans.\
C. It always stores table rows in the same physical order as its keys.\
D. It is a synonym for a foreign key.

### Q8. Transactions

A transaction runs two operations concurrently with another transaction. Which ACID property addresses how their concurrent execution is controlled?

A. Atomicity\
B. Consistency\
C. Isolation\
D. Durability

## Section B — SQL Problem Solving (20 points)

Use this schema:

```
CREATE TABLE Departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(80) NOT NULL
);

CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INT,
    salary DECIMAL(10,2),
    FOREIGN KEY (department_id)
        REFERENCES Departments(department_id)
);

CREATE TABLE Projects (
    project_id INT PRIMARY KEY,
    project_name VARCHAR(100) NOT NULL
);

CREATE TABLE Assignments (
    employee_id INT,
    project_id INT,
    hours_worked DECIMAL(8,2),
    PRIMARY KEY (employee_id, project_id),
    FOREIGN KEY (employee_id)
        REFERENCES Employees(employee_id),
    FOREIGN KEY (project_id)
        REFERENCES Projects(project_id)
);
```

### Q9. Retrieve employees (3 points)

Write a query to retrieve the employee names and salaries of employees earning more than 50,000, ordered from highest salary to lowest.

### Q10. Aggregate by department (4 points)

Write a query returning:

- Department name.
- Number of employees in that department.
- Average employee salary.

Include only departments with at least three employees and an average salary greater than 40,000.

Your query should include only departments that have employees.

### Q11. Preserve empty departments (3 points)

Write a query showing every department and the number of employees in it, including departments with zero employees.

### Q12. Find unassigned employees (3 points)

Write a query returning the IDs and names of employees who have not been assigned to any project.

### Q13. Multi-table query (4 points)

Write a query returning each project name and the total number of hours assigned to it. Include only projects whose total recorded hours exceed 100.

Assume hours are non-negative and the intended report includes projects with assignments only.

### Q14. Update and delete safety (3 points)

An administrator intends to increase the salary of employee 25 by 5%.

1. Write the SQL update.
2. Explain what could happen if the employee filter is omitted.
3. Give one additional precaution before executing the update.

## Section C — Data Modeling and Normalization (12 points)

### Q15. Design a course registration database (5 points)

A university records:

- Student ID and student name.
- Course ID and course title.
- Each student's registration date and final mark for each course.

A student can register for multiple courses, and each course can have multiple students.

Design the tables, specifying primary and foreign keys. Explain where `RegistrationDate` and `FinalMark` should be stored and why.

### Q16. Identify and remove dependencies (4 points)

Consider:

```
Enrollment(
    StudentID,
    CourseID,
    StudentName,
    CourseTitle,
    InstructorID,
    InstructorName,
    FinalMark
)
```

Assume the primary key is `(StudentID, CourseID)` and the following dependencies hold:

```
StudentID → StudentName
CourseID → CourseTitle, InstructorID
InstructorID → InstructorName
(StudentID, CourseID) → FinalMark
```

Identify:

1. A partial dependency.
2. A transitive dependency.
3. A suitable normalized decomposition.

### Q17. Choose a physical index (3 points)

A database frequently executes:

```
SELECT employee_name
FROM Employees
WHERE department_id = 4
ORDER BY salary DESC;
```

Recommend a composite index that could help this query. Explain why index column order matters, and state one reason the index might not eliminate all sorting or data-access costs.

## Section D — Reliability and System Design (12 points)

### Q18. Diagnose a data-system incident (4 points)

An online shop occasionally displays an old stock quantity after a purchase. The database server itself remains operational, but a cache has not been updated after a successful database write.

Explain:

1. The likely cause of the stale result.
2. One strategy to reduce this problem.
3. One reliability or consistency concern.
4. How you would test that the fix works.

### Q19. Availability and fault tolerance (4 points)

A database service runs on a single server. A hardware failure makes the service unavailable until the server is repaired.

Propose two architectural or operational improvements that could reduce downtime. Explain what each improves and mention one limitation.

### Q20. B-tree reasoning (4 points)

A B-tree index page becomes full when a new key is inserted.

Explain:

1. What a page split is.
2. How the parent node may be affected.
3. Why balanced tree height matters for search performance.
4. Why a B-tree index does not necessarily mean physical table rows are stored in key order.

---

# Mock Exam 2 — Answer Key and Marking Guide

## Section A — Answers

| Question | Answer | Reason |
| --- | --- | --- |
| Q1 | B | Latency measures the time taken by an operation. |
| Q2 | B | ProductName depends on only part of the composite key. |
| Q3 | B | A left join preserves all departments. |
| Q4 | C | `HAVING` filters aggregated groups. |
| Q5 | A | Every relevant index may need maintenance during writes. |
| Q6 | B | A completed month's data can be processed in a batch. |
| Q7 | B | Linked leaf nodes commonly improve range scans. |
| Q8 | C | Isolation governs concurrent transaction interference. |

## Section B — Model Answers

### Q9. Retrieve employees (3 points)

```
SELECT employee_name, salary
FROM Employees
WHERE salary > 50000
ORDER BY salary DESC;
```

**Marking:**

- 1 point: correct columns and table.
- 1 point: correct filter.
- 1 point: descending salary order.

### Q10. Aggregate by department (4 points)

```
SELECT d.department_name,
       COUNT(e.employee_id) AS employee_count,
       AVG(e.salary) AS average_salary
FROM Departments AS d
INNER JOIN Employees AS e
    ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name
HAVING COUNT(e.employee_id) >= 3
   AND AVG(e.salary) > 40000;
```

**Marking:**

- 1 point: correct join.
- 1 point: correct grouping and aggregate expressions.
- 1 point: correct `HAVING` conditions.
- 1 point: valid query structure and output columns.

### Q11. Preserve empty departments (3 points)

```
SELECT d.department_name,
       COUNT(e.employee_id) AS employee_count
FROM Departments AS d
LEFT JOIN Employees AS e
    ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name;
```

Use `COUNT(e.employee_id)`, not `COUNT(*)`, because an unmatched department still contributes one joined row with `NULL` employee columns. `COUNT(e.employee_id)` returns zero for that department.

**Marking:**

- 1 point: left join.
- 1 point: correct grouping.
- 1 point: correct counting behavior.

### Q12. Find unassigned employees (3 points)

One valid solution:

```
SELECT e.employee_id, e.employee_name
FROM Employees AS e
LEFT JOIN Assignments AS a
    ON e.employee_id = a.employee_id
WHERE a.employee_id IS NULL;
```

Alternative using `NOT EXISTS`:

```
SELECT e.employee_id, e.employee_name
FROM Employees AS e
WHERE NOT EXISTS (
    SELECT 1
    FROM Assignments AS a
    WHERE a.employee_id = e.employee_id
);
```

**Marking:** Full marks for either correct approach.

### Q13. Multi-table query (4 points)

```
SELECT p.project_name,
       SUM(a.hours_worked) AS total_hours
FROM Projects AS p
INNER JOIN Assignments AS a
    ON p.project_id = a.project_id
GROUP BY p.project_id, p.project_name
HAVING SUM(a.hours_worked) > 100;
```

**Marking:**

- 1 point: correct joins.
- 1 point: correct grouping.
- 1 point: correct `SUM`.
- 1 point: correct aggregate filter.

### Q14. Update and delete safety (3 points)

```
UPDATE Employees
SET salary = salary * 1.05
WHERE employee_id = 25;
```

Without the `WHERE` clause, every employee's salary could increase by 5%.

A precaution is to run a `SELECT` using the same condition first, verify the target employee, and execute the change in a transaction where supported.

**Marking:**

- 1 point: correct SQL.
- 1 point: consequence of missing `WHERE`.
- 1 point: sensible safety measure.

## Section C — Model Answers

### Q15. Design a course registration database (5 points)

```
Student(
    StudentID PK,
    StudentName
)

Course(
    CourseID PK,
    CourseTitle
)

Registration(
    StudentID PK, FK,
    CourseID PK, FK,
    RegistrationDate,
    FinalMark
)
```

The composite key `(StudentID, CourseID)` identifies each student-course registration, assuming each student can register for a given course only once.

The registration date and final mark belong in `Registration` because they describe the association between a particular student and a particular course.

**Marking:**

- 2 points: correct entity tables and primary keys.
- 1 point: correct junction table and foreign keys.
- 1 point: appropriate composite key.
- 1 point: correct placement and explanation of registration-specific attributes.

### Q16. Identify and remove dependencies (4 points)

**Partial dependency:**

`StudentID → StudentName`, because `StudentID` is only part of the composite key.

**Transitive dependency:**

`CourseID → InstructorID → InstructorName`.

Suitable decomposition:

```
Student(
    StudentID PK,
    StudentName
)

Instructor(
    InstructorID PK,
    InstructorName
)

Course(
    CourseID PK,
    CourseTitle,
    InstructorID FK
)

Enrollment(
    StudentID PK, FK,
    CourseID PK, FK,
    FinalMark
)
```

**Marking:**

- 1 point: partial dependency.
- 1 point: transitive dependency.
- 2 points: appropriate decomposition preserving relationships and `FinalMark`.

### Q17. Choose a physical index (3 points)

A reasonable candidate is:

```
CREATE INDEX idx_employee_department_salary
ON Employees (department_id, salary DESC);
```

The leading `department_id` supports filtering by department. The salary ordering within that department may support the descending sort.

However, the DBMS might still need extra work because of its index implementation, query plan, or data-access requirements. Returning `employee_name` may also require fetching table records unless the chosen index covers that column.

**Marking:**

- 1 point: suitable index.
- 1 point: correct explanation of column order.
- 1 point: valid limitation.

## Section D — Model Answers

### Q18. Diagnose a data-system incident (4 points)

1. **Cause:** The cache contains an older value because a successful database update was not propagated or invalidated.
2. **Mitigation:** Invalidate or update the relevant cache entry after the database transaction commits; use suitable ordering, retry, and recovery mechanisms.
3. **Concern:** A stale cache can expose outdated stock quantities and contribute to overselling if inventory validation relies on stale data.
4. **Testing:** Test a purchase followed by a stock read, including repeated requests, cache misses, failures during invalidation, and concurrent purchases.

**Marking:** Award 1 point per component.

### Q19. Availability and fault tolerance (4 points)

Two possible improvements:

**1\. Replication and failover**

Maintain a replica that can take over if the primary fails.

- Benefit: Reduces downtime when the primary server becomes unavailable.
- Limitation: Replication lag or failover errors can affect recovery, and configuration matters.

**2\. Backups and recovery testing**

Maintain reliable backups and regularly test restoration procedures.

- Benefit: Allows data recovery after corruption or a serious failure.
- Limitation: Restoring backups does not necessarily provide immediate service availability.

Other valid answers include redundant infrastructure, health monitoring, automated failover, and disaster recovery planning.

**Marking:** Award 2 points for each well-explained improvement, including a relevant limitation.

### Q20. B-tree reasoning (4 points)

- **Page split:** A split divides a full node into multiple nodes so another entry can be inserted.
- **Parent update:** The parent must be updated to represent the new key ranges. Splits may propagate upward and create a new root.
- **Balanced height:** A balanced tree keeps search paths relatively short, typically providing logarithmic search complexity and limiting page reads.
- **Physical row order:** The index organizes its own keys and references. Unless the storage design specifically clusters rows by that index, the physical table records need not follow the index order.

**Marking:** Award 1 point per component.

---

# Mock Exam 3 — Comprehensive Challenge

**Level:** Challenge\
**Duration:** 90 minutes\
**Total marks:** 70 points\
**Focus:** Integrated design, multi-step SQL, anomaly analysis, transaction correctness, and performance trade-offs.

## Instructions

- Read the assumptions and schema carefully.
- Show intermediate reasoning for multi-step questions.
- SQL answers should be logically correct; equivalent solutions receive credit.
- Explain why a proposed solution works and identify its limitations.

## Section A — Conceptual Challenge (10 points)

_5 questions × 2 points._

### Q1. Latency distributions (2 points)

A service has low average response time but occasionally takes 20 seconds to respond.

Explain why its average latency alone may not adequately describe user experience. Identify one additional metric or distribution statistic that would help.

### Q2. Redundancy and normalization (2 points)

A designer proposes putting a customer's department name directly into every employee row to avoid joins.

Explain the trade-off and identify the normalization problem if department names depend on department IDs.

### Q3. Durability and atomicity (2 points)

A database transaction successfully commits. Shortly afterward, the server loses power.

Which ACID property concerns whether the committed changes survive, and how does this differ from atomicity?

### Q4. Index trade-offs (2 points)

A query filters employees by department, orders them by salary, and returns their names.

Explain why an index can improve this query without guaranteeing that every operation becomes faster.

### Q5. Modeling sequence (2 points)

Explain why conceptual modeling should normally precede physical database design. Give one example of a costly mistake that could arise from choosing physical structures before understanding business requirements.

## Section B — Integrated SQL Case Study (24 points)

A university uses the following schema:

```
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    city VARCHAR(80)
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    department VARCHAR(80) NOT NULL
);

CREATE TABLE Enrolments (
    student_id INT,
    course_id INT,
    mark DECIMAL(5,2),
    registration_date DATE NOT NULL,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id)
        REFERENCES Students(student_id),
    FOREIGN KEY (course_id)
        REFERENCES Courses(course_id)
);
```

Assume that each student can register for a course at most once and that `mark` can be `NULL` until grading is complete.

### Q6. Student performance (3 points)

Write a query that returns the names of students who have at least one mark greater than or equal to 80. Each student should appear only once.

### Q7. Course averages (4 points)

Return each course ID, course name, number of enrolment records, and average non-`NULL` mark.

Only include courses with at least five non-`NULL` marks and an average mark of at least 70.

### Q8. Students with no registrations (3 points)

Write a query that finds students who have never registered for any course.

### Q9. Department-level report (5 points)

Write a query returning each department and its average mark across all enrolments in that department.

Include only departments with an average mark greater than 65. Courses with no non-`NULL` marks should not qualify.

### Q10. Interpret a SQL query (4 points)

Consider:

```
SELECT c.department,
       COUNT(*) AS enrolment_count,
       AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
WHERE e.mark >= 50
GROUP BY c.department
HAVING COUNT(*) >= 10
ORDER BY average_mark DESC;
```

Explain:

1. What rows are removed by `WHERE`.
2. What is counted by `COUNT(*)`.
3. What the `HAVING` condition means.
4. Why this result does not necessarily represent the average mark across all enrolments.

### Q11. Repair a query (3 points)

A student wants to list departments with an average mark of at least 60, but writes:

```
SELECT c.department, AVG(e.mark)
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
WHERE AVG(e.mark) >= 60
GROUP BY c.department;
```

Explain the error and rewrite the query correctly.

### Q12. Update safely (2 points)

An administrator wants to set the mark to 75 for student 14 in course 22.

Write the update statement using both identifying columns.

## Section C — Database Design and Normalization (12 points)

### Q13. Normalize a sales record (6 points)

A company initially stores sales in this table:

```
Sales(
    OrderID,
    OrderDate,
    CustomerID,
    CustomerName,
    ProductID,
    ProductName,
    UnitPrice,
    Quantity
)
```

Assume:

- Each order belongs to one customer.
- An order can contain multiple products.
- Each product has one current product name.
- `UnitPrice` records the price agreed for that order line.
- An order contains at most one line for each product.

Dependencies include:

```
OrderID → OrderDate, CustomerID
CustomerID → CustomerName
ProductID → ProductName
(OrderID, ProductID) → UnitPrice, Quantity
```

Propose a normalized schema. Identify one partial dependency and explain why the order-line price should not automatically be replaced by the current product price.

### Q14. Relationship design under changing requirements (3 points)

The university later allows students to retake a course, with a separate registration date and mark for each attempt.

Explain why `(StudentID, CourseID)` is no longer sufficient as the primary key and propose a better design.

### Q15. Choose a schema for analytics (3 points)

A retail company must produce reports by day, product, store, and region.

Compare a star schema and a snowflake schema for this workload. Recommend one, justifying the recommendation with at least two considerations.

## Section D — Performance and Systems Reasoning (12 points)

### Q16. Composite index selection (4 points)

A large employee table is queried frequently with:

```
SELECT employee_id, employee_name, salary
FROM Employees
WHERE department_id = 7
  AND salary >= 60000
ORDER BY salary DESC;
```

Recommend an index and explain:

1. Why the column order matters.
2. How the index may help filtering and sorting.
3. Why it may not eliminate every table read.

### Q17. B-tree insertion and deletion (4 points)

Explain how a B-tree maintains its balance when:

1. A full leaf page receives a new key.
2. A deletion causes a node to become underfull.

Then explain why these operations may require changes to parent pages.

### Q18. Diagnose a performance problem (4 points)

A database table has 50 million rows. A query filters on a frequently used column but remains slow. The administrator creates indexes on 15 additional columns, after which inserts become slower.

Explain:

1. Why the original query may have been slow even with an index.
2. Why adding many indexes can slow inserts.
3. Two steps you would take to investigate or improve performance.

## Section E — Transactions and System Reliability (12 points)

### Q19. Transaction failure analysis (4 points)

An application transfers €500 from Account A to Account B:

1. Deduct €500 from Account A.
2. Add €500 to Account B.

The server fails between these operations.

Explain the potential inconsistency and how a transaction with appropriate ACID guarantees addresses it. Identify which ACID properties are most directly relevant.

### Q20. Design for fault tolerance (4 points)

An online booking system must continue serving users during a single server failure and must recover confirmed bookings after a crash.

Propose an architecture and operational approach that addresses both availability and data durability. Explain at least one trade-off or limitation.

### Q21. Batch versus stream integration (4 points)

A delivery company has two requirements:

- Detect a potentially fraudulent payment within seconds.
- Produce a reconciled monthly revenue report.

Recommend a processing approach for each. Explain how the systems might work together and why a single processing approach is not necessarily ideal for both.

---

# Mock Exam 3 — Answer Key and Marking Guide

## Section A — Model Answers

### Q1. Latency distributions (2 points)

An average can conceal a long tail of slow requests. For example, most requests might complete quickly while a small percentage take 20 seconds.

A useful additional measure is the 95th or 99th percentile latency, or a latency distribution.

**Marking:**

- 1 point: identifies the limitation of the average.
- 1 point: identifies a suitable metric.

### Q2. Redundancy and normalization (2 points)

Duplicating department names may reduce some joins, but it creates redundancy. If a department name changes, many employee records must be updated.

When:

```
EmployeeID → DepartmentID
DepartmentID → DepartmentName
```

storing the department name in the employee table introduces a transitive dependency.

**Marking:**

- 1 point: trade-off or update anomaly.
- 1 point: dependency explanation.

### Q3. Durability and atomicity (2 points)

Durability concerns whether committed changes survive a crash.

Atomicity means a transaction's operations succeed together or are rolled back together. Atomicity does not, by itself, guarantee that a successful commit survives a later failure.

**Marking:** 1 point per distinction.

### Q4. Index trade-offs (2 points)

An appropriate index may reduce scanning and help retrieve qualifying rows in salary order.

However, indexes consume storage, require maintenance, and may still require table lookups for `employee_name` or other columns. The optimizer may also choose another access plan depending on data distribution and selectivity.

**Marking:**

- 1 point: benefit.
- 1 point: limitation.

### Q5. Modeling sequence (2 points)

Conceptual modeling clarifies business entities, relationships, and requirements before implementation decisions are made.

For example, designing a database around an incorrect assumption that each order has only one product could require expensive schema changes when multi-product orders are later needed.

**Marking:**

- 1 point: reason for the sequence.
- 1 point: valid example.

## Section B — Model Answers

### Q6. Student performance (3 points)

```
SELECT DISTINCT s.student_name
FROM Students AS s
JOIN Enrolments AS e
    ON s.student_id = e.student_id
WHERE e.mark >= 80;
```

`DISTINCT` ensures each student name appears only once.

**Marking:**

- 1 point: correct join.
- 1 point: correct filter.
- 1 point: prevents duplicate output names.

_Note:_ If students with different IDs may share the same name and must be distinguished, include `student_id` in the output and apply `DISTINCT` to both columns.

### Q7. Course averages (4 points)

```
SELECT c.course_id,
       c.course_name,
       COUNT(e.mark) AS marked_enrolments,
       AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
GROUP BY c.course_id, c.course_name
HAVING COUNT(e.mark) >= 5
   AND AVG(e.mark) >= 70;
```

`COUNT(e.mark)` ignores `NULL` marks, so the threshold applies to graded enrolments. `AVG(e.mark)` also ignores `NULL` marks.

**Marking:**

- 1 point: join and output columns.
- 1 point: grouping.
- 1 point: correct count of non-`NULL` marks.
- 1 point: both `HAVING` conditions.

### Q8. Students with no registrations (3 points)

```
SELECT s.student_id, s.student_name
FROM Students AS s
WHERE NOT EXISTS (
    SELECT 1
    FROM Enrolments AS e
    WHERE e.student_id = s.student_id
);
```

A valid `LEFT JOIN ... WHERE e.student_id IS NULL` solution also receives full credit.

**Marking:**

- 1 point: correct relationship check.
- 1 point: correctly identifies missing registrations.
- 1 point: valid query structure.

### Q9. Department-level report (5 points)

```
SELECT c.department,
       AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
GROUP BY c.department
HAVING AVG(e.mark) > 65;
```

`AVG` ignores `NULL` marks. Departments whose enrolments have no non-`NULL` marks produce a `NULL` average and do not satisfy `AVG(e.mark) > 65`.

**Marking:**

- 1 point: join.
- 1 point: grouping by department.
- 1 point: correct aggregate.
- 1 point: correct threshold.
- 1 point: correctly handles departments with no graded marks.

### Q10. Interpret a SQL query (4 points)

1. `WHERE e.mark >= 50` removes enrolment rows with marks below 50 and rows whose mark is `NULL`.
2. `COUNT(*)` counts the remaining joined enrolment rows in each department group.
3. `HAVING COUNT(*) >= 10` keeps departments with at least ten qualifying rows.
4. The average is calculated only over rows that passed the `WHERE` filter. It is therefore not the average of all enrolments, but the average of qualifying marks.

**Marking:** Award 1 point per explanation.

### Q11. Repair a query (3 points)

The error is that an aggregate condition belongs in `HAVING`, not `WHERE`, because grouping and aggregation occur after row filtering.

Correct query:

```
SELECT c.department,
       AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
GROUP BY c.department
HAVING AVG(e.mark) >= 60;
```

**Marking:**

- 1 point: explains the error.
- 1 point: correct use of `GROUP BY`.
- 1 point: correct `HAVING` condition.

### Q12. Update safely (2 points)

```
UPDATE Enrolments
SET mark = 75
WHERE student_id = 14
  AND course_id = 22;
```

Both conditions are needed to identify the intended composite-key row.

**Marking:**

- 1 point: correct `SET`.
- 1 point: both key columns in the filter.

## Section C — Model Answers

### Q13. Normalize a sales record (6 points)

One suitable design is:

```
Customer(
    CustomerID PK,
    CustomerName
)

Product(
    ProductID PK,
    ProductName
)

Orders(
    OrderID PK,
    OrderDate,
    CustomerID FK
)

OrderLine(
    OrderID PK, FK,
    ProductID PK, FK,
    UnitPrice,
    Quantity
)
```

**Partial dependency:**

`OrderID → OrderDate, CustomerID`.

These attributes depend on only part of the composite key `(OrderID, ProductID)`.

**Transitive dependencies:**

`CustomerID → CustomerName` means customer names belong in the Customer table. Similarly, product names depend on `ProductID` and belong in the Product table.

**Historical price:**

`UnitPrice` should remain on `OrderLine` because it represents the historical agreed price for that transaction. The current product price may change after the order was placed.

**Marking:**

- 3 points: appropriate schema and keys.
- 1 point: partial dependency.
- 1 point: customer/product redundancy addressed.
- 1 point: historical order price explained.

### Q14. Relationship design under changing requirements (3 points)

A student can now have multiple registrations for the same course. Therefore, `(StudentID, CourseID)` would no longer uniquely identify each attempt.

One suitable design is:

```
Registration(
    RegistrationID PK,
    StudentID FK,
    CourseID FK,
    RegistrationDate,
    FinalMark
)
```

Alternatively, use a composite key such as:

```
Registration(
    StudentID PK, FK,
    CourseID PK, FK,
    AttemptNumber PK,
    RegistrationDate,
    FinalMark
)
```

The first design uses a surrogate key. The second uses the student, course, and attempt number to identify each registration.

**Marking:**

- 1 point: explains why the original key is insufficient.
- 1 point: proposes a valid replacement key.
- 1 point: correctly preserves the registration date and mark for each attempt.

### Q15. Choose a schema for analytics (3 points)

**Star schema:**

- A central fact table connects directly to dimension tables.
- Usually offers simpler queries and fewer joins.
- Often suits analytical workloads and business intelligence reporting.

**Snowflake schema:**

- Dimension data is normalized into additional related tables.
- Can reduce duplicated dimension attributes.
- Usually requires more joins and can make reporting queries more complex.

**Recommendation:** A star schema is generally a good default for this workload because reports frequently aggregate sales by day, product, store, and region. Direct dimension relationships simplify query writing and often improve analytical usability. A snowflake schema may be preferable when dimension data is large, shared extensively, or benefits significantly from normalization.

**Marking:**

- 1 point: correct comparison.
- 1 point: appropriate recommendation.
- 1 point: at least two relevant considerations.

## Section D — Model Answers

### Q16. Composite index selection (4 points)

A reasonable candidate is:

```
CREATE INDEX idx_employees_department_salary
ON Employees (department_id, salary DESC);
```

**Column order:** `department_id` is first because the query uses an equality condition on that column. `salary` follows because the query applies a range condition and requests descending salary order.

**Filtering and sorting:** The index may help locate the department's employees, restrict entries to salaries of at least 60,000, and retrieve qualifying index entries in the required salary order.

**Remaining table reads:** The query also returns `employee_name`. Unless that column is available through a covering index or included-column feature supported by the DBMS, the engine may need to fetch table records. The actual benefit depends on selectivity, data distribution, and the optimizer's chosen plan.

**Marking:**

- 1 point: suitable index.
- 1 point: column order.
- 1 point: filtering and sorting benefits.
- 1 point: explains remaining data-access costs.

### Q17. B-tree insertion and deletion (4 points)

**Insertion into a full leaf:**

A full leaf page may split into two pages. The index redistributes entries and updates the parent with a separator key or equivalent routing information.

**Deletion causing an underfull node:**

The tree may redistribute entries from a sibling or merge nodes, depending on the implementation and occupancy rules. Parent separator keys and child references may need updating. A merge can propagate structural changes upward.

**Parent-page changes:**

Splits, redistributions, and merges alter the ranges of keys represented by child nodes. Parent pages must reflect those changes so searches continue to follow the correct paths. Root changes may also occur.

Maintaining balance limits the length of search paths and supports efficient lookup, insertion, and deletion.

**Marking:**

- 1 point: insertion and split.
- 1 point: deletion and redistribution or merging.
- 1 point: parent updates.
- 1 point: explains the role of balance.

### Q18. Diagnose a performance problem (4 points)

**Why the original query may be slow even with an index:**

- The filter may match a large proportion of the 50 million rows, making an index less selective.
- The query may require additional table lookups, sorting, or other expensive operations.
- The optimizer may choose a table scan because it estimates that scanning is cheaper.
- The index may be unsuitable for the filter's column order or query conditions.

**Why many indexes slow inserts:**

Each applicable index must be maintained when a row is inserted. Adding 15 indexes can increase write work, storage use, logging, page splits, and cache pressure.

**Two investigation or improvement steps:**

1. Inspect the execution plan and query statistics to determine whether the query scans the table, uses the index efficiently, performs extra lookups, or sorts a large result.
2. Review selectivity and existing indexes, then design a targeted index for the actual query. Remove redundant or unused indexes where justified by workload measurements.

Other valid approaches include updating statistics, rewriting the query, checking data skew, and benchmarking representative workloads.

**Marking:**

- 1 point: plausible explanation for slow query performance.
- 1 point: explains insert overhead.
- 2 points: two relevant investigative or optimization steps.

## Section E — Model Answers

### Q19. Transaction failure analysis (4 points)

If the server fails after Account A has been debited but before Account B has been credited, the transfer may be left partially completed. The total balance across the two accounts may decrease incorrectly.

A database transaction groups both operations so that either both succeed or neither is committed:

```
BEGIN TRANSACTION;

UPDATE Accounts
SET Balance = Balance - 500
WHERE AccountID = 'A';

UPDATE Accounts
SET Balance = Balance + 500
WHERE AccountID = 'B';

COMMIT;
```

If an error occurs before commit, the application should roll back the transaction. The exact syntax for transaction control varies by DBMS.

The most directly relevant ACID properties are:

- **Atomicity:** Both account updates succeed together or are rolled back together.
- **Consistency:** Business rules, such as valid balances, remain satisfied.
- **Durability:** After a successful commit, the transfer survives a subsequent server failure.
- **Isolation:** Concurrent transactions should not interfere in a way that produces incorrect account balances.

**Marking:**

- 1 point: explains the partial-transfer problem.
- 1 point: explains atomicity and rollback.
- 1 point: identifies consistency or isolation as relevant.
- 1 point: explains durability or the role of commit.

### Q20. Design for fault tolerance (4 points)

A suitable design could include:

1. **Redundant application servers behind a load balancer:** Requests can be routed to a healthy server when another fails.
2. **Database replication and automated failover:** A standby database can take over when the primary becomes unavailable, subject to replication and failover guarantees.
3. **Durable transaction commits:** The database should use appropriate logging and persistence mechanisms to recover confirmed bookings after a crash.
4. **Backups and recovery testing:** Regular backups and tested restoration procedures provide an additional recovery path if data is corrupted or replication propagates an unwanted change.

Confirmed bookings should not be acknowledged as durable until the required database commit has succeeded. Where necessary, idempotency controls can prevent retries from creating duplicate bookings.

**Trade-offs and limitations:**

- Replication can introduce lag or failover complexity.
- Synchronous replication may increase write latency.
- Asynchronous replication may lose recently committed changes if the primary fails before those changes reach the replica.
- Backups improve recoverability but do not guarantee immediate availability.

**Marking:**

- 2 points: credible availability architecture.
- 1 point: addresses durability and recovery.
- 1 point: explains a relevant trade-off or limitation.

### Q21. Batch versus stream integration (4 points)

**Fraud detection — Stream processing**

Process incoming payment events continuously so that suspicious activity can be identified within seconds. The system can apply rules, thresholds, or other detection logic as events arrive.

**Monthly revenue report — Batch processing**

Aggregate and reconcile completed transaction data at the end of the month. Batch processing supports repeatable calculations over a defined period and can incorporate corrections or late-arriving records.

**How they work together:**

Both systems can consume a common event stream or use a shared durable transaction store. The streaming system supports low-latency operational decisions, while the batch system independently reconciles the complete data set.

The two approaches serve different objectives:

- Stream processing prioritizes low latency and continuous results.
- Batch processing prioritizes complete-period aggregation, reconciliation, and repeatability.

A single approach may not satisfy both requirements equally well. Streaming results can be provisional when events arrive late or out of order, while batch processing alone may detect fraud too late.

**Marking:**

- 1 point: stream processing for fraud detection.
- 1 point: batch processing for monthly reporting.
- 1 point: explains integration.
- 1 point: explains the different requirements and trade-offs.

---

# End of Examination Collection

**Collection:** Data Management 2026–2027\
**Coverage:** T0–T2\
**Examinations:** 3\
**Progression:** Foundation → Application → Comprehensive Challenge

## Suggested use

1. Attempt each exam under its stated time limit.
2. Record your answers before consulting the answer key.
3. Use the marking guides to identify weaknesses in SQL, normalization, indexing, and transaction reasoning.
4. Review incorrect answers and repeat the relevant problems without looking at the solutions.
5. Continue with the same structure for T3–T5, using progressively more integrated and challenging questions.

**Scope note:** These examinations are based on the T0–T2 material supplied in the conversation. They should not be interpreted as covering lecture-specific material that was not included in that source material.
