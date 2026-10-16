Yes. I’ll restructure the collection into three complete 120-minute university-style practice exams, each combining foundational concepts, applied problems, and a comprehensive challenge rather than isolating those levels into separate papers.

The exams will progressively increase in difficulty, while retaining the T0–T2 scope, SQL schemas, normalization, indexing, transactions, reliability, and system-design topics from the original material.

## New exam structure

| Component | Exam 1 | Exam 2 | Exam 3 |
| --- | --- | --- | --- |
| Duration | 120 min | 120 min | 120 min |
| Total marks | 100 | 100 | 100 |
| Concepts and fundamentals | 25 | 20 | 15 |
| Applied problems | 45 | 45 | 40 |
| Comprehensive challenge | 30 | 35 | 45 |
| Difficulty | Foundation to intermediate | Intermediate to advanced | Advanced and integrated |

Each exam will include its own instructions, time allocation, point values, self-contained schemas, practical SQL questions, and a comprehensive solution guide with explanations and marking criteria.

The papers will use standard SQL where possible, state assumptions when database-specific behavior matters, and distinguish between a query that produces the correct result and an explanation of why it works. The solutions will also cover common mistakes, alternative valid approaches, and partial-credit guidance.

Writing

# Data Management 2026–2027

## T0–T2 Practice Examination Collection

**Three complete university-style practice exams — 120 minutes each**

- **Exam 1:** Foundations and Applied Database Skills
- **Exam 2:** Applied SQL, Database Design, and Performance
- **Exam 3:** Comprehensive Database Challenge

## Table of Contents

1. General Examination Instructions
2. Practice Exam 1 — Foundations and Applied Skills
3. Practice Exam 1 — Complete Solutions
4. Practice Exam 2 — Applied Database Problems
5. Practice Exam 2 — Complete Solutions
6. Practice Exam 3 — Comprehensive Challenge
7. Practice Exam 3 — Complete Solutions
8. Final Revision Checklist

---

# General Examination Instructions

These practice exams cover the supplied T0–T2 material: database foundations, data models, relational design, normalization, SQL, indexing, transaction processing, reliability, and system performance.

All three papers contain conceptual questions, practical problems, and integrated challenges. Their difficulty increases progressively.

## Examination rules

- Answer every question.
- Show your reasoning for conceptual and design questions.
- Write syntactically valid SQL wherever possible.
- Assume standard SQL unless the question states otherwise.
- State any necessary assumptions.
- Do not consult the solutions until you have completed the exam.
- Equivalent SQL solutions receive credit when they satisfy the stated requirements.

## Timing and marks

| Exam | Duration | Total marks | Recommended approach |
| --- | --- | --- | --- |
| Exam 1 | 120 minutes | 100 | Build accuracy and fundamentals |
| Exam 2 | 120 minutes | 100 | Apply concepts to realistic problems |
| Exam 3 | 120 minutes | 100 | Solve integrated, multi-step problems |

---

# Practice Exam 1 — Foundations and Applied Skills

**Duration:** 120 minutes\
**Total:** 100 points\
**Difficulty:** Foundation to intermediate

## Recommended time allocation

| Section | Focus | Time | Marks |
| --- | --- | --- | --- |
| A | Concepts and fundamentals | 30 min | 25 |
| B | Applied SQL and database design | 50 min | 45 |
| C | Comprehensive challenge | 40 min | 30 |
| **Total** |  | **120 min** | **100** |

## Section A — Concepts and Fundamentals (25 points)

### Question 1. Multiple choice (10 points)

Choose one answer per question. Each question is worth 2 points.

**1.1.** Which metric measures how many operations a system completes per unit of time?

A. Latency\
B. Throughput\
C. Maintainability\
D. Normalization

**1.2.** Which modeling level defines implementation details such as indexes and DBMS-specific data types?

A. Conceptual\
B. Logical\
C. Physical\
D. Business

**1.3.** Which normal form addresses partial functional dependencies on a proper subset of a candidate key?

A. 1NF\
B. 2NF\
C. 3NF\
D. ACID

**1.4.** Which SQL clause filters groups after aggregate calculations?

A. `WHERE`\
B. `FROM`\
C. `HAVING`\
D. `ORDER BY`

**1.5.** Which ACID property ensures that a committed transaction's changes survive a subsequent crash, subject to the database's durability guarantees?

A. Atomicity\
B. Consistency\
C. Isolation\
D. Durability

### Question 2. Short-answer concepts (9 points)

**2.1. Reliability and scalability (3 points)**

Define reliability and scalability. Give one practical example of each in a database system.

**2.2. Primary and foreign keys (3 points)**

Explain the purpose of a primary key and a foreign key. Give one example of how a foreign key helps maintain data integrity.

**2.3. Fault, error, and failure (3 points)**

A disk develops a hardware defect. A read returns corrupted information, and an application displays an incorrect result.

Identify the fault, the error, and the failure.

### Question 3. Data models and architecture (6 points)

**3.1. Modeling levels (3 points)**

A university needs a database for students and courses. Explain one design decision at each of the conceptual, logical, and physical modeling levels.

**3.2. Batch and stream processing (3 points)**

A retailer has two requirements:

- Calculate yesterday's total revenue every night.
- Detect potentially fraudulent transactions within seconds.

Recommend a processing model for each requirement and justify your choices.

---

## Section B — Applied SQL and Database Design (45 points)

Use the following schema for Questions 4–7.

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

### Question 4. Basic SQL retrieval (8 points)

**4.1. Filtering and sorting (3 points)**

Retrieve employee names and salaries for employees earning more than 50,000. Sort from highest salary to lowest.

**4.2. Aggregation (3 points)**

Return each department ID and its average employee salary. Include only departments whose average salary exceeds 45,000.

**4.3. Updating data safely (2 points)**

Increase the salary of employee 17 by 5%. Explain the consequence of omitting the `WHERE` clause.

### Question 5. Joins and subqueries (12 points)

**5.1. Preserve departments (4 points)**

Return every department and the number of employees in it, including departments with no employees.

**5.2. Find unassigned employees (4 points)**

Return the employee IDs and names of employees who have no project assignments.

**5.3. Project workload (4 points)**

Return each project name and its total assigned hours. Include only projects with more than 80 recorded hours. Assume hours are non-negative and projects without assignments should be excluded.

### Question 6. Normalization (12 points)

A university initially stores registration data in the following relation:

```
Registration(
    StudentID,
    StudentName,
    CourseID,
    CourseTitle,
    InstructorID,
    InstructorName,
    RegistrationDate,
    FinalMark
)
```

Assume the primary key is `(StudentID, CourseID)` and the following dependencies hold:

```
StudentID → StudentName

CourseID → CourseTitle, InstructorID

InstructorID → InstructorName

(StudentID, CourseID) → RegistrationDate, FinalMark
```

**6.1. Identify dependencies (4 points)**

Identify one partial dependency and one transitive dependency. Explain why each matters.

**6.2. Normalize the schema (6 points)**

Propose a normalized decomposition with primary and foreign keys.

**6.3. Registration attributes (2 points)**

Explain why `RegistrationDate` and `FinalMark` belong in the registration relation rather than in the Student or Course relation.

### Question 7. Index design (13 points)

A reporting query frequently runs against `Employees`:

```
SELECT employee_id, employee_name, salary
FROM Employees
WHERE department_id = 3
  AND salary >= 50000
ORDER BY salary DESC;
```

**7.1. Index proposal (4 points)**

Recommend an index and explain the column order.

**7.2. Benefits and limitations (4 points)**

Explain two ways the index may help and two reasons the query might still require significant work.

**7.3. Write overhead (3 points)**

Explain how adding many indexes can affect insert and update performance.

**7.4. Measurement (2 points)**

Name two things you would inspect before deciding whether the index has improved performance.

---

## Section C — Comprehensive Challenge (30 points)

### Question 8. Integrated university database (15 points)

A university has the following schema:

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

Assume marks may be `NULL` until grading is complete.

**8.1. High-performing students (4 points)**

Return student IDs and names of students who have at least one mark of 80 or above. Each student must appear once.

**8.2. Course performance (5 points)**

Return each course ID, course name, number of graded enrolments, and average mark. Include only courses with at least three graded enrolments and an average mark of at least 70.

**8.3. Students without enrolments (3 points)**

Return the IDs and names of students who have never registered for a course.

**8.4. Interpret the result (3 points)**

Explain why `COUNT(mark)` and `COUNT(*)` can produce different results for a group of enrolments containing `NULL` marks.

### Question 9. Transactions and reliability (15 points)

An online bank transfers €300 from Account A to Account B. The application performs a debit followed by a credit.

**9.1. Transaction failure (5 points)**

Explain what can happen if the server fails between the two operations. Explain how a transaction helps.

**9.2. ACID analysis (6 points)**

Explain how each ACID property applies to this transfer.

**9.3. Availability and recovery (4 points)**

Propose two measures to improve the service's availability or recovery capability. Explain one limitation of each.

---

# Practice Exam 1 — Complete Solutions

## Section A — Solutions

### Question 1. Multiple choice (10 points)

| Question | Answer | Explanation |
| --- | --- | --- |
| 1.1 | B | Throughput measures completed operations per unit of time. |
| 1.2 | C | Physical modeling specifies implementation details. |
| 1.3 | B | 2NF removes partial dependencies on candidate keys. |
| 1.4 | C | `HAVING` filters aggregated groups. |
| 1.5 | D | Durability concerns the persistence of committed changes. |

### Question 2. Short-answer concepts (9 points)

**2.1. Reliability and scalability**

- **Reliability:** The ability to operate correctly and recover appropriately from failures. Example: recovering committed data after a crash.
- **Scalability:** The ability to handle increasing data volumes or workload. Example: adding database capacity as user demand grows.

**Marking:** 1.5 points per definition and example, with partial credit for incomplete answers.

**2.2. Primary and foreign keys**

A primary key uniquely identifies each row and cannot contain `NULL`. A foreign key references an eligible key in another table or the same table.

For example, `Employees.department_id` referencing `Departments.department_id` prevents an employee from referencing a nonexistent department, unless the foreign key is nullable and the value is `NULL`.

**Marking:**

- 1 point: primary-key purpose.
- 1 point: foreign-key purpose.
- 1 point: valid integrity example.

**2.3. Fault, error, and failure**

- **Fault:** The hardware defect in the disk.
- **Error:** The corrupted information or incorrect internal state produced by the read.
- **Failure:** The application displays the incorrect result.

**Marking:** 1 point per distinction.

### Question 3. Data models and architecture (6 points)

**3.1. Modeling levels**

- **Conceptual:** Identify Students and Courses and their many-to-many relationship.
- **Logical:** Define `Students`, `Courses`, and a junction relation such as `Enrolments`, with attributes and keys.
- **Physical:** Select data types, indexes, constraints, and DBMS-specific storage choices.

**Marking:** 1 point per level.

**3.2. Batch and stream processing**

- **Daily revenue — Batch:** The previous day's transactions form a completed data set that can be aggregated periodically.
- **Fraud detection — Stream:** Transactions should be evaluated as events arrive to support decisions within seconds.

**Marking:** 1 point for each recommendation and 1 point for appropriate reasoning.

## Section B — Solutions

### Question 4. Basic SQL retrieval (8 points)

**4.1. Filtering and sorting**

```
SELECT employee_name, salary
FROM Employees
WHERE salary > 50000
ORDER BY salary DESC;
```

The filter removes rows that do not satisfy the salary condition. `DESC` sorts salaries from highest to lowest.

**Marking:** 1 point for selected columns and table, 1 for the filter, and 1 for sorting.

**4.2. Aggregation**

```
SELECT department_id,
       AVG(salary) AS average_salary
FROM Employees
GROUP BY department_id
HAVING AVG(salary) > 45000;
```

`GROUP BY` creates one group per department. `HAVING` filters the groups after calculating the average.

**Marking:** 1 point for grouping, 1 for the average, and 1 for the correct threshold.

**4.3. Updating data safely**

```
UPDATE Employees
SET salary = salary * 1.05
WHERE employee_id = 17;
```

Without `WHERE`, every employee's salary would be increased by 5%.

**Marking:** 1 point for correct SQL and 1 for explaining the consequence.

### Question 5. Joins and subqueries (12 points)

**5.1. Preserve departments**

```
SELECT d.department_id,
       d.department_name,
       COUNT(e.employee_id) AS employee_count
FROM Departments AS d
LEFT JOIN Employees AS e
    ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name;
```

The left join preserves departments with no employees. `COUNT(e.employee_id)` returns zero for unmatched departments because the employee ID is `NULL` in the generated joined row.

Using `COUNT(*)` would incorrectly count that generated row as one.

**Marking:** 2 points for the left join, 1 for grouping, and 1 for correct counting.

**5.2. Find unassigned employees**

```
SELECT e.employee_id, e.employee_name
FROM Employees AS e
WHERE NOT EXISTS (
    SELECT 1
    FROM Assignments AS a
    WHERE a.employee_id = e.employee_id
);
```

`NOT EXISTS` keeps an employee only if no matching assignment exists.

An equivalent solution is:

```
SELECT e.employee_id, e.employee_name
FROM Employees AS e
LEFT JOIN Assignments AS a
    ON e.employee_id = a.employee_id
WHERE a.employee_id IS NULL;
```

**Marking:** 4 points for a correct solution, including the relationship condition.

**5.3. Project workload**

```
SELECT p.project_id,
       p.project_name,
       SUM(a.hours_worked) AS total_hours
FROM Projects AS p
JOIN Assignments AS a
    ON p.project_id = a.project_id
GROUP BY p.project_id, p.project_name
HAVING SUM(a.hours_worked) > 80;
```

The inner join excludes projects without assignments. `SUM` calculates the total hours, and `HAVING` filters the groups.

**Marking:** 1 point for the join, 1 for grouping, 1 for the sum, and 1 for the threshold.

### Question 6. Normalization (12 points)

**6.1. Identify dependencies**

A partial dependency is:

```
StudentID → StudentName
```

`StudentID` is only part of the composite key `(StudentID, CourseID)`. Student names therefore do not belong in the registration relation.

A transitive dependency is:

```
CourseID → InstructorID → InstructorName
```

The instructor name depends on the instructor ID rather than directly on the complete registration key.

These dependencies create redundancy and potential update, insertion, and deletion anomalies.

**Marking:** 2 points for the partial dependency and explanation; 2 for the transitive dependency and explanation.

**6.2. Normalized schema**

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

Registration(
    StudentID PK, FK,
    CourseID PK, FK,
    RegistrationDate,
    FinalMark
)
```

The `Course` relation references the instructor. The `Registration` relation links students and courses and stores the attributes specific to that association.

This design removes duplicated student names and instructor names from registration records.

**Marking:**

- 2 points for Student, Instructor, and Course relations.
- 2 points for Registration with the correct composite key.
- 2 points for correct foreign keys and attribute placement.

**6.3. Registration attributes**

`RegistrationDate` and `FinalMark` describe a particular student's registration in a particular course. They are properties of the relationship, not of the student independently or the course independently.

**Marking:** 2 points for explaining both attributes' placement.

### Question 7. Index design (13 points)

**7.1. Index proposal**

```
CREATE INDEX idx_employees_department_salary
ON Employees (department_id, salary DESC);
```

`department_id` comes first because the query filters on a specific department. `salary` follows to support the salary range and descending ordering within that department.

**Marking:** 2 points for a suitable index and 2 for the column-order explanation.

**7.2. Benefits and limitations**

Possible benefits:

1. The index can reduce the number of employee records examined by restricting access to a department and salary range.
2. It may allow qualifying entries to be read in the requested salary order, reducing sorting work.

Possible limitations:

1. If the conditions match a large proportion of the table, a table scan may be competitive.
2. The query requests `employee_name`, which may require extra table lookups because that column is not in the index.
3. The optimizer may choose a different plan.
4. Performance depends on data distribution and index implementation.

**Marking:** 1 point for each of two valid benefits and two valid limitations.

**7.3. Write overhead**

Every applicable index must be updated when indexed values are inserted, deleted, or changed. Many indexes consume additional storage and can increase page writes, logging, page splits, and cache pressure.

**Marking:** 3 points for explaining maintenance work and its performance implications.

**7.4. Measurement**

Suitable checks include:

- The execution plan, including scans, index access, lookups, and sorts.
- Actual execution time and rows processed.
- Logical and physical reads.
- Representative workload measurements before and after the change.

**Marking:** 1 point per valid measurement or diagnostic.

## Section C — Solutions

### Question 8. Integrated university database (15 points)

**8.1. High-performing students**

```
SELECT DISTINCT s.student_id, s.student_name
FROM Students AS s
JOIN Enrolments AS e
    ON s.student_id = e.student_id
WHERE e.mark >= 80;
```

`DISTINCT` prevents a student with multiple high marks from appearing more than once.

**Marking:** 1 point for the join, 2 for the filter and output, and 1 for eliminating duplicates.

**8.2. Course performance**

```
SELECT c.course_id,
       c.course_name,
       COUNT(e.mark) AS graded_enrolments,
       AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
GROUP BY c.course_id, c.course_name
HAVING COUNT(e.mark) >= 3
   AND AVG(e.mark) >= 70;
```

`COUNT(e.mark)` counts non-`NULL` marks, so the minimum number of graded enrolments is applied correctly. `AVG` also ignores `NULL` marks.

**Marking:**

- 1 point for the join and selected columns.
- 1 point for grouping.
- 2 points for correct aggregate conditions.
- 1 point for counting graded enrolments rather than all enrolments.

**8.3. Students without enrolments**

```
SELECT s.student_id, s.student_name
FROM Students AS s
WHERE NOT EXISTS (
    SELECT 1
    FROM Enrolments AS e
    WHERE e.student_id = s.student_id
);
```

**Marking:** 3 points for a correct anti-join or equivalent solution.

**8.4.****`COUNT(mark)`****versus****`COUNT(*)`**

`COUNT(mark)` counts only rows where `mark` is not `NULL`. `COUNT(*)` counts every row, including rows where `mark` is `NULL`.

For example, if three enrolment records have marks `80`, `NULL`, and `65`:

- `COUNT(mark)` returns `2`.
- `COUNT(*)` returns `3`.

**Marking:** 2 points for the distinction and 1 for a correct example.

### Question 9. Transactions and reliability (15 points)

**9.1. Transaction failure**

If the server fails after the debit but before the credit, the transfer may be left incomplete. The source account loses €300 without the destination account receiving it.

A transaction groups both updates into one logical unit. If the transaction fails before commit, the database can roll back its changes, provided the transaction and recovery mechanisms operate correctly.

**Marking:** 2 points for the failure scenario, 2 for atomicity, and 1 for rollback.

**9.2. ACID analysis**

- **Atomicity:** Both the debit and credit occur, or neither is committed.
- **Consistency:** The transaction preserves applicable constraints and business rules.
- **Isolation:** Concurrent transfers do not interfere in a way that creates incorrect balances.
- **Durability:** After a successful commit, the transfer survives a subsequent failure according to the database's durability guarantees.

**Marking:** 1.5 points per property.

**9.3. Availability and recovery**

Two suitable measures are:

1. **Replication and failover:** A standby database can take over after primary failure. Limitation: replication lag, failover errors, or configuration mistakes can affect recovery.
2. **Backups and tested recovery procedures:** Backups permit restoration after data loss or corruption. Limitation: restoring a backup may take considerable time and does not ensure immediate availability.

Other acceptable measures include redundant application servers, monitoring, automated recovery, and disaster recovery planning.

**Marking:** 2 points per improvement, including a limitation.

---

# Practice Exam 2 — Applied Database Problems

**Duration:** 120 minutes\
**Total:** 100 points\
**Difficulty:** Intermediate to advanced

## Recommended time allocation

| Section | Focus | Time | Marks |
| --- | --- | --- | --- |
| A | Applied concepts | 25 min | 20 |
| B | SQL and database design | 55 min | 45 |
| C | Comprehensive challenge | 40 min | 35 |
| **Total** |  | **120 min** | **100** |

## Section A — Applied Concepts (20 points)

### Question 1. Multiple choice (8 points)

Each question is worth 2 points.

**1.1.** A table uses `(OrderID, ProductID)` as its primary key. `ProductName` depends only on `ProductID`. Which normal form is violated?

A. 1NF\
B. 2NF\
C. 3NF only\
D. None

**1.2.** A report must include every department, even departments with no employees. Which join is appropriate?

A. `INNER JOIN`\
B. `LEFT JOIN` from Departments to Employees\
C. `CROSS JOIN`\
D. `RIGHT JOIN` from Departments to Employees, regardless of table order

**1.3.** What is the most direct consequence of adding many indexes to a frequently updated table?

A. All queries become faster.\
B. Write operations may become more expensive.\
C. Storage requirements decrease.\
D. Transactions become unnecessary.

**1.4.** Which index property generally helps a B-tree support efficient range queries?

A. It contains no keys.\
B. It maintains an ordered search structure.\
C. It replaces every table constraint.\
D. It prevents page splits.

### Question 2. Functional dependencies and anomalies (6 points)

Consider:

```
OrderLine(
    OrderID,
    ProductID,
    OrderDate,
    ProductName,
    Quantity
)
```

The primary key is `(OrderID, ProductID)`.

Assume:

```
OrderID → OrderDate
ProductID → ProductName
(OrderID, ProductID) → Quantity
```

**2.1.** Identify two partial dependencies. (2 points)

**2.2.** Explain one update anomaly caused by this design. (2 points)

**2.3.** Propose a normalized decomposition. (2 points)

### Question 3. Query behavior and transactions (6 points)

**3.1. Aggregate filtering (3 points)**

Explain why the following query is invalid in standard SQL:

```
SELECT department_id, AVG(salary)
FROM Employees
WHERE AVG(salary) > 40000
GROUP BY department_id;
```

Rewrite it correctly.

**3.2. Isolation (3 points)**

Explain why atomicity alone does not prevent two concurrent transactions from producing an incorrect result when they read and update the same account balance.

---

## Section B — SQL and Database Design (45 points)

Use the following schema:

```
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    city VARCHAR(80)
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    FOREIGN KEY (customer_id)
        REFERENCES Customers(customer_id)
);

CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    current_price DECIMAL(10,2) NOT NULL
);

CREATE TABLE OrderItems (
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id)
        REFERENCES Orders(order_id),
    FOREIGN KEY (product_id)
        REFERENCES Products(product_id)
);
```

### Question 4. Customer and order queries (12 points)

**4.1. Recent orders (3 points)**

Return order IDs, order dates, and customer names for orders placed on or after 1 January 2026, ordered from newest to oldest.

**4.2. Customer spending (4 points)**

Calculate the total order value for each customer. Include only customers whose total spending exceeds 1,000. Assume an order item's value is `quantity * unit_price`.

**4.3. Customers without orders (3 points)**

Return customers who have never placed an order.

**4.4. Explain the price choice (2 points)**

Why should the spending calculation use `OrderItems.unit_price` rather than `Products.current_price`?

### Question 5. More advanced SQL (13 points)

**5.1. Product sales (4 points)**

Return each product ID and the total quantity sold. Include only products with a total quantity greater than 100. Assume only products with order items should appear.

**5.2. Above-average products (5 points)**

Return product IDs and names for products whose current price is greater than the average current price across all products.

**5.3. Query interpretation (4 points)**

Explain what the following query returns and why `WHERE` and `HAVING` perform different roles:

```
SELECT o.customer_id,
       COUNT(*) AS order_count,
       SUM(oi.quantity * oi.unit_price) AS total_value
FROM Orders AS o
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
WHERE o.order_date >= DATE '2026-01-01'
GROUP BY o.customer_id
HAVING SUM(oi.quantity * oi.unit_price) > 2000;
```

### Question 6. Database design (10 points)

A training platform stores courses, students, and course registrations.

Each student can register for several courses. Each course can have many students. Each registration stores a date and a result.

**6.1.** Design the tables, primary keys, and foreign keys. (4 points)

**6.2.** Explain where registration-specific attributes belong. (2 points)

**6.3.** The platform later permits retakes. Propose a revised key design. (2 points)

**6.4.** Explain one advantage and one limitation of a surrogate registration key. (2 points)

### Question 7. Index selection and query plans (10 points)

A large `Orders` table is queried frequently:

```
SELECT order_id, customer_id, order_date
FROM Orders
WHERE customer_id = 42
  AND order_date >= DATE '2026-01-01'
ORDER BY order_date DESC;
```

**7.1.** Recommend a composite index. (3 points)

**7.2.** Explain why column order matters. (3 points)

**7.3.** Explain two reasons the index might not produce the expected improvement. (2 points)

**7.4.** Name two measurements or diagnostics that would help evaluate the index. (2 points)

---

## Section C — Comprehensive Challenge (35 points)

### Question 8. Integrated sales reporting (15 points)

Use the customer, order, product, and order-item schema from Section B.

**8.1. Department-independent customer report (5 points)**

Write a query returning each customer ID and customer name, along with the total amount spent on orders placed during 2026. Include customers who spent nothing during 2026, showing zero spending.

**8.2. High-value customers (4 points)**

Write a query returning customers whose total lifetime order value exceeds 5,000.

**8.3. Data quality (3 points)**

Explain why summing `quantity * current_price` from `Products` can produce an incorrect historical revenue report.

**8.4. Index proposal (3 points)**

Suggest an index to support queries that filter orders by customer and date. Explain its expected benefit.

### Question 9. Performance and B-tree reasoning (10 points)

A database contains 50 million employee records. A query filtering on `department_id` remains slow. An administrator adds indexes on 15 columns, and inserts become slower.

**9.1.** Explain two reasons the original query could remain slow even with an index. (3 points)

**9.2.** Explain why additional indexes can slow inserts. (3 points)

**9.3.** Describe a B-tree page split and explain how it may affect parent nodes. (2 points)

**9.4.** Recommend two investigation or optimization steps. (2 points)

### Question 10. Transactions and fault tolerance (10 points)

A booking service confirms a reservation and charges a customer. These actions involve a database and an external payment service.

**10.1.** Explain why a single ordinary database transaction cannot automatically make both systems atomic. (3 points)

**10.2.** Propose a practical approach to handle partial failure. (4 points)

**10.3.** Explain how replication and backups contribute differently to availability and recovery. (3 points)

---

# Practice Exam 2 — Complete Solutions

## Section A — Solutions

### Question 1. Multiple choice (8 points)

| Question | Answer | Explanation |
| --- | --- | --- |
| 1.1 | B | `ProductName` depends on only part of the composite key, violating 2NF. |
| 1.2 | B | A left join preserves every department. |
| 1.3 | B | Index maintenance increases write work and storage requirements. |
| 1.4 | B | Ordered search structures support range retrieval. |

### Question 2. Functional dependencies and anomalies (6 points)

**2.1. Partial dependencies**

```
OrderID → OrderDate
ProductID → ProductName
```

Each determinant is only part of the composite key `(OrderID, ProductID)`.

**Marking:** 1 point per dependency.

**2.2. Update anomaly**

The order date is repeated for each product in the same order. Changing the order date requires multiple rows to be updated. If some rows are missed, the database may contain inconsistent dates for one order.

Similarly, a product name change may require updating many order-line records.

**Marking:** 2 points for a valid explanation.

**2.3. Normalized decomposition**

```
Orders(
    OrderID PK,
    OrderDate
)

Products(
    ProductID PK,
    ProductName
)

OrderLine(
    OrderID PK, FK,
    ProductID PK, FK,
    Quantity
)
```

**Marking:** 1 point for the entity relations and 1 for the correct junction relation and keys.

### Question 3. Query behavior and transactions (6 points)

**3.1. Aggregate filtering**

The aggregate `AVG(salary)` cannot be used in `WHERE` in this way because `WHERE` filters individual rows before grouping and aggregation. The aggregate condition belongs in `HAVING`.

Correct query:

```
SELECT department_id,
       AVG(salary) AS average_salary
FROM Employees
GROUP BY department_id
HAVING AVG(salary) > 40000;
```

**Marking:** 1 point for explaining the error and 2 points for the corrected query.

**3.2. Isolation**

Atomicity guarantees that a transaction's own operations succeed or fail together. It does not automatically prevent concurrent transactions from reading the same old balance and overwriting each other's updates.

Isolation controls the effects of concurrent execution. Depending on the operation, appropriate isolation, locking, or an atomic conditional update may be needed to prevent lost updates and related anomalies.

**Marking:**

- 1 point: defines atomicity's limitation.
- 1 point: explains the concurrency problem.
- 1 point: proposes an isolation or concurrency-control measure.

## Section B — Solutions

### Question 4. Customer and order queries (12 points)

**4.1. Recent orders**

```
SELECT o.order_id,
       o.order_date,
       c.customer_name
FROM Orders AS o
JOIN Customers AS c
    ON o.customer_id = c.customer_id
WHERE o.order_date >= DATE '2026-01-01'
ORDER BY o.order_date DESC;
```

**Marking:** 1 point for the join, 1 for the date condition, and 1 for sorting.

**4.2. Customer spending**

```
SELECT c.customer_id,
       c.customer_name,
       SUM(oi.quantity * oi.unit_price) AS total_spending
FROM Customers AS c
JOIN Orders AS o
    ON c.customer_id = o.customer_id
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(oi.quantity * oi.unit_price) > 1000;
```

The inner joins restrict the result to customers with orders containing order items. The grouping calculates a total per customer.

**Marking:**

- 1 point: correct joins.
- 1 point: correct line-value calculation.
- 1 point: grouping by customer.
- 1 point: correct `HAVING` condition.

**4.3. Customers without orders**

```
SELECT c.customer_id, c.customer_name
FROM Customers AS c
WHERE NOT EXISTS (
    SELECT 1
    FROM Orders AS o
    WHERE o.customer_id = c.customer_id
);
```

**Marking:** 3 points for a correct anti-join.

**4.4. Explain the price choice**

`OrderItems.unit_price` records the price agreed for the specific transaction. `Products.current_price` may change after an order is placed. Using the current price would therefore rewrite historical revenue according to today's prices rather than the transaction's actual price.

**Marking:** 2 points for explaining the historical-price requirement.

### Question 5. More advanced SQL (13 points)

**5.1. Product sales**

```
SELECT p.product_id,
       SUM(oi.quantity) AS total_quantity
FROM Products AS p
JOIN OrderItems AS oi
    ON p.product_id = oi.product_id
GROUP BY p.product_id
HAVING SUM(oi.quantity) > 100;
```

**Marking:** 1 point for the join, 1 for grouping, 1 for `SUM`, and 1 for the threshold.

**5.2. Above-average products**

```
SELECT product_id, product_name
FROM Products
WHERE current_price > (
    SELECT AVG(current_price)
    FROM Products
);
```

The subquery returns one scalar value: the average current price across all products. The outer query selects products priced above that value.

**Marking:** 2 points for the scalar subquery, 2 for the comparison, and 1 for the correct output.

**5.3. Query interpretation**

The query returns customers who have order items belonging to orders dated on or after 1 January 2026, grouped by customer, with their qualifying order count and total order-item value.

- `WHERE` filters individual joined rows by order date before grouping.
- `COUNT(*)` counts the joined order-item rows that remain in each customer group. It does not necessarily count distinct orders, because one order can have multiple items.
- `SUM(oi.quantity * oi.unit_price)` calculates the value of the remaining order items.
- `HAVING` keeps only customer groups whose calculated total exceeds 2,000.

**Marking:**

- 1 point: explains `WHERE`.
- 1 point: explains `COUNT(*)` correctly.
- 1 point: explains the sum.
- 1 point: explains `HAVING`.

### Question 6. Database design (10 points)

**6.1. Tables and keys**

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
    Result
)
```

The registration relation resolves the many-to-many relationship.

**Marking:** 2 points for Student and Course, 2 for Registration and its keys.

**6.2. Registration-specific attributes**

The date and result describe an individual student-course registration. They therefore belong in Registration, not in Student or Course.

**Marking:** 2 points.

**6.3. Retakes**

If a student can register for the same course more than once, `(StudentID, CourseID)` is no longer unique. One solution is:

```
Registration(
    RegistrationID PK,
    StudentID FK,
    CourseID FK,
    AttemptNumber,
    RegistrationDate,
    Result,
    UNIQUE(StudentID, CourseID, AttemptNumber)
)
```

Another valid solution is to use `(StudentID, CourseID, AttemptNumber)` as a composite primary key.

**Marking:** 2 points for a valid key design.

**6.4. Surrogate registration key**

An advantage is that other tables can reference a single, stable identifier without carrying a multi-column key.

A limitation is that the surrogate key does not automatically prevent duplicate attempts. A suitable unique constraint may still be required.

**Marking:** 1 point per valid advantage or limitation.

### Question 7. Index selection and query plans (10 points)

**7.1. Composite index**

```
CREATE INDEX idx_orders_customer_date
ON Orders (customer_id, order_date DESC);
```

**Marking:** 3 points for a suitable index and correct ordering.

**7.2. Column order**

The equality condition on `customer_id` makes it a useful leading column. `order_date` follows to support the date range and requested descending order for a particular customer.

The leading column matters because a composite B-tree index is generally most useful for predicates that constrain its leading columns. A different query workload could justify a different order.

**Marking:** 3 points for the equality, range, and ordering reasoning.

**7.3. Why improvement may be limited**

Possible reasons include:

- The customer has many orders, so a large proportion of relevant records still needs to be processed.
- The optimizer estimates that another plan is cheaper.
- Statistics are stale or data distribution is highly skewed.
- Additional sorting or data access remains necessary.

**Marking:** 1 point per valid reason.

**7.4. Measurements**

Suitable diagnostics include:

- Actual execution plan and estimated versus actual rows.
- Query execution time.
- Logical and physical reads.
- Index usage statistics and representative workload measurements.

**Marking:** 1 point per valid diagnostic.

## Section C — Solutions

### Question 8. Integrated sales reporting (15 points)

**8.1. Customer spending during 2026**

```
SELECT c.customer_id,
       c.customer_name,
       COALESCE(
           SUM(oi.quantity * oi.unit_price),
           0
       ) AS total_spending
FROM Customers AS c
LEFT JOIN Orders AS o
    ON c.customer_id = o.customer_id
   AND o.order_date >= DATE '2026-01-01'
   AND o.order_date < DATE '2027-01-01'
LEFT JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
GROUP BY c.customer_id, c.customer_name;
```

The date filter is in the join condition so that customers without qualifying orders are preserved. Putting the date condition in `WHERE` would remove customers with no matching orders.

`COALESCE` converts a `NULL` sum to zero.

**Marking:**

- 2 points: preserves all customers with left joins.
- 1 point: correct date range.
- 1 point: correct line-value aggregation.
- 1 point: handles missing spending as zero.

**8.2. High-value customers**

```
SELECT c.customer_id,
       c.customer_name,
       SUM(oi.quantity * oi.unit_price) AS lifetime_value
FROM Customers AS c
JOIN Orders AS o
    ON c.customer_id = o.customer_id
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(oi.quantity * oi.unit_price) > 5000;
```

**Marking:** 1 point for joins, 1 for the total calculation, 1 for grouping, and 1 for the threshold.

**8.3. Data quality**

The current product price can differ from the price paid when an order was placed. Multiplying historical quantities by current prices would misstate historical revenue. The stored order-line price should be used for transaction reporting.

**Marking:** 3 points for explaining the temporal distinction and its financial impact.

**8.4. Index proposal**

```
CREATE INDEX idx_orders_customer_date
ON Orders (customer_id, order_date);
```

This index can narrow the search to one customer and a date range. It may also reduce sorting when the required order is compatible with the index and its direction.

**Marking:** 2 points for a suitable index and 1 for its benefit.

### Question 9. Performance and B-tree reasoning (10 points)

**9.1. Why the query could remain slow (3 points)**

- The filter may match a large percentage of the table, making a scan competitive.
- The query may need many table lookups or additional sorting.
- The index may not match the predicate structure.
- The optimizer may choose a different plan due to its cost estimates.

**Marking:** Award up to 3 points for two well-explained reasons.

**9.2. Insert overhead (3 points)**

Each insert may require adding entries to multiple indexes. This increases CPU work, page writes, logging, storage use, and potentially page splits. More indexes can also increase cache pressure.

**Marking:** 3 points for a clear explanation of maintenance overhead.

**9.3. B-tree page split (2 points)**

When a page becomes full, a split redistributes entries into additional pages. The parent must be updated to represent the new key ranges. If the parent is also full, the split can propagate upward.

**Marking:** 1 point for the split and 1 for the parent update.

**9.4. Investigation or optimization (2 points)**

Suitable steps include:

1. Inspect the execution plan, selectivity, statistics, and number of rows read.
2. Design a targeted index for the actual query, then benchmark before and after.
3. Remove redundant or unused indexes after confirming their workload impact.

**Marking:** 1 point per suitable step.

### Question 10. Transactions and fault tolerance (10 points)

**10.1. Limits of one database transaction (3 points)**

A normal database transaction controls changes managed by that database. An external payment service has its own transaction and failure boundaries. A database rollback cannot automatically reverse a payment that has already been accepted by the external service.

**Marking:** 3 points for explaining the separate transaction boundaries.

**10.2. Handling partial failure (4 points)**

One practical approach is a **saga-style workflow**:

1. Record the booking request and its state in the database.
2. Initiate the payment with a unique idempotency key.
3. Record the payment result and confirm the booking only when the required steps succeed.
4. If payment fails, leave the booking unconfirmed or cancel it. If the booking cannot be completed after payment succeeds, initiate a refund or other compensating action.

Use durable status records, retries, timeouts, and reconciliation for uncertain outcomes.

An outbox pattern can help ensure that database state changes and outgoing events are recorded consistently. It does not, by itself, make the external payment atomic with the database.

**Marking:** 1 point each for state tracking, idempotency or safe retries, compensation, and recovery/reconciliation.

**10.3. Replication and backups (3 points)**

- **Replication:** Maintains additional copies that may support failover and reduce service interruption. It can also replicate unwanted changes or corruption.
- **Backups:** Provide a recovery point for restoring data after corruption, deletion, or a serious incident. Restoration can take time and may lose changes made since the backup, depending on the recovery strategy.

**Marking:** 2 points for the distinction and 1 for a limitation.

---

# Practice Exam 3 — Comprehensive Challenge

**Duration:** 120 minutes\
**Total:** 100 points\
**Difficulty:** Advanced and integrated

## Recommended time allocation

| Section | Focus | Time | Marks |
| --- | --- | --- | --- |
| A | Advanced concepts | 20 min | 15 |
| B | Integrated SQL and design | 50 min | 40 |
| C | Comprehensive challenge | 50 min | 45 |
| **Total** |  | **120 min** | **100** |

## Section A — Advanced Concepts (15 points)

### Question 1. Conceptual distinctions (9 points)

Answer each in concise but technically accurate terms.

**1.1. Latency and throughput (3 points)**

A service handles 5,000 requests per second but occasionally takes 15 seconds to respond to an individual request. Explain why throughput alone does not adequately describe performance.

**1.2. Normalization and denormalization (3 points)**

Explain one advantage and one disadvantage of normalizing a database. Give a situation in which controlled denormalization may be justified.

**1.3. Durability and availability (3 points)**

Explain the difference between durability and availability. Give one example of a system that improves one without necessarily improving the other.

### Question 2. Index and transaction reasoning (6 points)

**2.1. Composite index (3 points)**

Why might an index on `(department_id, salary)` help a query filtering by department and salary but be less useful for a query filtering only by salary?

**2.2. Concurrent updates (3 points)**

Two transactions read an account balance of 1,000. One withdraws 100 and the other withdraws 200. Explain how a lost update could occur and name one way to prevent it.

---

## Section B — Integrated SQL and Design (40 points)

Use this schema:

```
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    status VARCHAR(20) NOT NULL,
    FOREIGN KEY (customer_id)
        REFERENCES Customers(customer_id)
);

CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    current_price DECIMAL(10,2) NOT NULL
);

CREATE TABLE OrderItems (
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id)
        REFERENCES Orders(order_id),
    FOREIGN KEY (product_id)
        REFERENCES Products(product_id)
);
```

Assume completed orders have `status = 'COMPLETED'`. Order-line prices represent historical transaction prices.

### Question 3. Applied SQL (15 points)

**3.1. Completed order totals (4 points)**

Return each completed order ID and its total value, calculated from quantity and historical unit price.

**3.2. Customer revenue (4 points)**

Return customer IDs and lifetime revenue from completed orders. Include only customers with revenue above 10,000.

**3.3. Customers with no completed orders (3 points)**

Return customers who have never placed a completed order.

**3.4. Product revenue ranking (4 points)**

Return each product ID and its total revenue from completed orders, including only products whose total revenue exceeds 5,000. Sort by total revenue descending.

### Question 4. Query interpretation and repair (10 points)

Consider:

```
SELECT c.customer_id,
       COUNT(*) AS item_rows,
       AVG(oi.unit_price) AS average_unit_price,
       SUM(oi.quantity * oi.unit_price) AS revenue
FROM Customers AS c
JOIN Orders AS o
    ON c.customer_id = o.customer_id
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
WHERE o.status = 'COMPLETED'
  AND oi.unit_price >= 50
GROUP BY c.customer_id
HAVING SUM(oi.quantity * oi.unit_price) > 3000
ORDER BY revenue DESC;
```

**4.1. Explain the query (4 points)**

Explain the roles of `WHERE`, `COUNT(*)`, `AVG`, `HAVING`, and `ORDER BY`.

**4.2. Identify a reporting limitation (3 points)**

Explain why the reported average unit price and revenue may not represent all completed sales.

**4.3. Repair the requirement (3 points)**

Modify the query so that it reports the total revenue from all completed order items for customers whose total completed-order revenue exceeds 3,000, without filtering individual items by unit price.

### Question 5. Normalization and changing requirements (15 points)

A retail system initially stores:

```
Sales(
    OrderID,
    OrderDate,
    CustomerID,
    CustomerName,
    ProductID,
    ProductName,
    Quantity,
    UnitPrice
)
```

Assume:

```
OrderID → OrderDate, CustomerID
CustomerID → CustomerName
ProductID → ProductName
(OrderID, ProductID) → Quantity, UnitPrice
```

**5.1. Functional dependencies (4 points)**

Identify the composite key and two dependencies that cause redundancy.

**5.2. Normalize the design (6 points)**

Propose a normalized schema with primary and foreign keys.

**5.3. Historical prices (2 points)**

Explain why `UnitPrice` should remain in the order-line relation.

**5.4. Multiple lines per product (3 points)**

The business later allows the same product to appear on multiple lines in one order, for example because the lines have different discounts.

Explain why `(OrderID, ProductID)` is no longer sufficient as the primary key. Propose an alternative.

---

## Section C — Comprehensive Challenge (45 points)

### Question 6. Indexing and query optimization (15 points)

A large employee table contains 50 million rows.

The application frequently executes:

```
SELECT employee_id, employee_name, salary
FROM Employees
WHERE department_id = 7
  AND salary >= 60000
ORDER BY salary DESC;
```

The administrator has already created several single-column indexes, but the query remains slow. Inserts are also becoming slower.

**6.1. Composite index design (4 points)**

Recommend a composite index and explain the column order.

**6.2. Index coverage and trade-offs (4 points)**

Explain why the proposed index might still require table access. Discuss whether including `employee_name` could help and identify one cost.

**6.3. Diagnose the slow query (4 points)**

Describe four pieces of evidence or potential causes you would investigate before making further changes.

**6.4. Index maintenance (3 points)**

Recommend a process for determining whether any existing indexes should be removed.

### Question 7. Transaction correctness and reliability (15 points)

An online booking system performs the following operations:

1. Checks whether a room is available.
2. Charges a customer using an external payment service.
3. Inserts the booking into the database.
4. Sends a confirmation message.

The system occasionally creates duplicate bookings or charges a customer without recording the booking.

**7.1. Identify failure modes (4 points)**

Identify two plausible failure modes and explain how they lead to the reported problems.

**7.2. Design a safer workflow (6 points)**

Propose a workflow that handles concurrency, retries, payment outcomes, and confirmation messages.

**7.3. ACID and external services (3 points)**

Explain which guarantees a database transaction can provide and why it cannot automatically make the payment service part of the same atomic transaction.

**7.4. Recovery and monitoring (2 points)**

Name two operational controls that would help detect or recover from inconsistent booking and payment states.

### Question 8. Integrated analytics and processing architecture (15 points)

A retailer wants to:

- Detect potentially fraudulent payments within seconds.
- Produce a reconciled monthly revenue report.
- Preserve historical transaction prices.
- Continue operating during an application server failure.
- Recover confirmed transactions after a database crash.

**8.1. Batch versus stream (4 points)**

Choose a processing model for each analytical requirement and explain why.

**8.2. Historical data design (3 points)**

Explain how the database schema should preserve historical transaction prices and support reliable revenue calculations.

**8.3. Availability and durability (4 points)**

Propose an architecture that addresses application availability and database durability. Identify one limitation or trade-off.

**8.4. Integrated system design (4 points)**

Explain how transaction records, event processing, and monthly reconciliation could work together without treating a real-time result as the final financial truth.

---

# Practice Exam 3 — Complete Solutions

## Section A — Solutions

### Question 1. Conceptual distinctions (9 points)

**1.1. Latency and throughput**

Throughput measures the number of requests processed per unit of time. It does not show how long an individual request takes or whether a small proportion of requests experience very long delays.

Useful additional measures include median latency, the 95th or 99th percentile latency, and the distribution of response times.

**Marking:**

- 1 point: correct throughput definition.
- 1 point: explains its limitation.
- 1 point: identifies a useful latency measure.

**1.2. Normalization and denormalization**

Normalization reduces redundant storage and helps prevent update, insertion, and deletion anomalies. However, normalized designs can require more joins and may complicate some reporting queries.

Controlled denormalization can be justified in a measured analytical workload when duplicated or precomputed information materially improves performance and consistency can be maintained.

**Marking:**

- 1 point: normalization advantage.
- 1 point: disadvantage.
- 1 point: justified denormalization example.

**1.3. Durability and availability**

- **Durability:** Committed data survives subsequent failures according to the system's guarantees.
- **Availability:** The service is accessible and able to respond to requests.

A database can have durable data stored safely in backups but remain unavailable during a long restoration. Conversely, a replicated service may remain accessible while still requiring careful controls to guarantee the persistence of every acknowledged write.

**Marking:** 1 point for each definition and 1 for a valid example.

### Question 2. Index and transaction reasoning (6 points)

**2.1. Composite index**

A B-tree index on `(department_id, salary)` is ordered primarily by department and then by salary within each department. It can therefore support department equality filtering and salary conditions within that department.

A query filtering only by salary may not be able to use the leading part of that index efficiently because the index is not globally ordered by salary alone. The optimizer might still use the index in some circumstances, but a separate salary index or another plan may be more appropriate.

**Marking:** 1 point for the leading-column principle, 1 for the query example, and 1 for the limitation.

**2.2. Concurrent updates**

Both transactions might read 1,000. One computes 900 and the other computes 800. If each writes its calculated balance without appropriate concurrency control, the later write can overwrite the earlier one. The final balance may be 900 or 800 instead of the intended 700.

Possible protections include row locking, suitable transaction isolation, optimistic concurrency control, or an atomic update such as:

```
UPDATE Accounts
SET balance = balance - 100
WHERE account_id = 1
  AND balance >= 100;
```

The application must check the affected-row count and handle concurrent execution correctly. For multiple withdrawals, each operation should apply to the current committed balance.

**Marking:**

- 1 point: identifies the lost-update problem.
- 1 point: explains how it occurs.
- 1 point: proposes a valid control.

## Section B — Solutions

### Question 3. Applied SQL (15 points)

**3.1. Completed order totals**

```
SELECT o.order_id,
       SUM(oi.quantity * oi.unit_price) AS order_total
FROM Orders AS o
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
WHERE o.status = 'COMPLETED'
GROUP BY o.order_id;
```

The query filters completed orders, joins their order items, and calculates each order's total using the recorded unit prices.

**Marking:** 1 point for the join, 1 for the status filter, 1 for the value calculation, and 1 for grouping.

**3.2. Customer revenue**

```
SELECT c.customer_id,
       SUM(oi.quantity * oi.unit_price) AS lifetime_revenue
FROM Customers AS c
JOIN Orders AS o
    ON c.customer_id = o.customer_id
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
WHERE o.status = 'COMPLETED'
GROUP BY c.customer_id
HAVING SUM(oi.quantity * oi.unit_price) > 10000;
```

**Marking:** 1 point for joins, 1 for the completed-order filter, 1 for grouping and aggregation, and 1 for the threshold.

**3.3. Customers with no completed orders**

```
SELECT c.customer_id, c.customer_name
FROM Customers AS c
WHERE NOT EXISTS (
    SELECT 1
    FROM Orders AS o
    WHERE o.customer_id = c.customer_id
      AND o.status = 'COMPLETED'
);
```

This includes customers who have no orders and customers who have orders but none completed.

**Marking:** 3 points for a correct anti-join that checks the status condition.

**3.4. Product revenue ranking**

```
SELECT oi.product_id,
       SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM Orders AS o
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
WHERE o.status = 'COMPLETED'
GROUP BY oi.product_id
HAVING SUM(oi.quantity * oi.unit_price) > 5000
ORDER BY total_revenue DESC;
```

**Marking:** 1 point for joins and filtering, 1 for revenue calculation, 1 for grouping and threshold, and 1 for descending sorting.

### Question 4. Query interpretation and repair (10 points)

**4.1. Explain the query**

- `WHERE` retains completed-order item rows with a unit price of at least 50.
- `COUNT(*)` counts the qualifying joined order-item rows per customer.
- `AVG(oi.unit_price)` calculates the mean unit price among the retained rows.
- `HAVING` removes customer groups whose qualifying revenue does not exceed 3,000.
- `ORDER BY revenue DESC` sorts the surviving customers by calculated revenue.

**Marking:** 1 point for each correctly explained role, with 4 points maximum.

**4.2. Reporting limitation**

The query excludes items with unit prices below 50 before aggregation. As a result, both its average unit price and revenue are based only on the qualifying items, not all completed order items.

The revenue may therefore understate the customer's full completed-order revenue.

**Marking:** 3 points for identifying the row filter and its effects on both aggregates.

**4.3. Repair the requirement**

Remove the unit-price filter while retaining the completed-order filter:

```
SELECT c.customer_id,
       SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM Customers AS c
JOIN Orders AS o
    ON c.customer_id = o.customer_id
JOIN OrderItems AS oi
    ON o.order_id = oi.order_id
WHERE o.status = 'COMPLETED'
GROUP BY c.customer_id
HAVING SUM(oi.quantity * oi.unit_price) > 3000;
```

**Marking:**

- 1 point: removes the incorrect unit-price filter.
- 1 point: calculates full completed-order revenue.
- 1 point: correctly applies the group threshold.

### Question 5. Normalization and changing requirements (15 points)

**5.1. Functional dependencies**

The composite primary key is `(OrderID, ProductID)`, assuming an order contains at most one line per product.

Two partial dependencies are:

```
OrderID → OrderDate, CustomerID
ProductID → ProductName
```

Additionally, `CustomerID → CustomerName` creates a transitive dependency from `OrderID` to `CustomerName` through the customer reference.

**Marking:** 2 points for the composite key and 2 for correct dependencies.

**5.2. Normalized design**

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
    Quantity,
    UnitPrice
)
```

The order line contains transaction-specific attributes. Customer and product details are stored once in their respective entity relations.

**Marking:**

- 2 points for Customer and Product.
- 2 points for Orders and its foreign key.
- 2 points for OrderLine and its composite key.

**5.3. Historical prices**

`UnitPrice` records the price agreed for a specific transaction. Replacing it with the current product price would alter the calculated value of historical sales whenever the product price changes.

**Marking:** 2 points.

**5.4. Multiple lines per product**

If the same product can appear more than once in an order, `(OrderID, ProductID)` no longer uniquely identifies a line.

One solution is:

```
OrderLine(
    OrderLineID PK,
    OrderID FK,
    ProductID FK,
    Quantity,
    UnitPrice,
    Discount
)
```

The line identifier uniquely identifies each order line. If the business has a meaningful line number, a composite key `(OrderID, LineNumber)` is also appropriate.

**Marking:** 1 point for identifying the key problem and 2 for a valid alternative.

## Section C — Solutions

### Question 6. Indexing and query optimization (15 points)

**6.1. Composite index design**

A candidate index is:

```
CREATE INDEX idx_employees_department_salary
ON Employees (department_id, salary DESC);
```

The equality predicate on `department_id` makes it a useful leading column. `salary` then supports the range condition and requested ordering within the selected department.

**Marking:** 2 points for the index and 2 for the column-order explanation.

**6.2. Index coverage and trade-offs**

The query returns `employee_name`, which is not included in the proposed index. The database may need additional table lookups to retrieve that column.

Where supported, a covering index could include the name:

```
-- Illustrative syntax for DBMSs supporting INCLUDE
CREATE INDEX idx_employees_department_salary_cover
ON Employees (department_id, salary DESC)
INCLUDE (employee_name);
```

`employee_id` may already be available through the index depending on the DBMS and storage model. Covering-index syntax and behavior are implementation-specific.

A covering index may reduce table lookups, but it consumes more storage and increases maintenance work when included or indexed values change.

**Marking:**

- 1 point: explains missing-column table access.
- 1 point: proposes a reasonable covering strategy.
- 1 point: explains the storage or write cost.
- 1 point: notes implementation or plan dependence.

**6.3. Diagnose the slow query**

Investigate at least four of the following:

1. **Selectivity:** What proportion of the 50 million records satisfies the department and salary conditions?
2. **Execution plan:** Is the query scanning the table, using an index, performing many lookups, or sorting?
3. **Statistics:** Are table and index statistics current and representative?
4. **Data distribution:** Is one department unusually large or does salary data have significant skew?
5. **Index structure:** Do the indexes match the predicate order and required sort?
6. **Resource bottlenecks:** Is the system limited by disk I/O, memory, CPU, or contention?
7. **Measurement quality:** Do representative execution times and logical reads confirm the perceived problem?

**Marking:** 1 point per relevant investigative area, up to 4.

**6.4. Index maintenance**

A responsible process is to:

1. Inventory indexes and identify which queries and constraints depend on them.
2. Review execution plans, usage statistics, and write overhead over a representative workload.
3. Identify redundant or unused indexes, taking into account infrequent but critical queries.
4. Test candidate removals in a staging or controlled environment and monitor the effect before making permanent changes.

Do not remove an index solely because it has low usage in a short observation period; it may support a critical periodic query or constraint.

**Marking:** 3 points for a measurement-based process that considers both read and write workloads.

### Question 7. Transaction correctness and reliability (15 points)

**7.1. Identify failure modes**

Two plausible failures are:

- **Duplicate submission:** A timeout causes the client to retry a booking request. If the system does not recognize the retry, it may create a second booking or charge.
- **Payment succeeds but booking is not recorded:** The external payment succeeds, but the application crashes before committing the booking. The customer is charged without a corresponding confirmed booking.
- **Concurrent availability checks:** Two requests both see a room as available before either reserves it, producing an overbooking if availability is not enforced atomically.

**Marking:** 2 points for each of two well-explained failure modes.

**7.2. Design a safer workflow**

A reasonable workflow includes:

1. **Use a durable booking request:** Assign a unique request or idempotency key and store the booking's state.
2. **Protect availability:** Use a database transaction, appropriate locking or isolation, and constraints to prevent conflicting reservations.
3. **Handle payment explicitly:** Submit the payment with an idempotency key so retries do not create duplicate charges. Record payment status and distinguish success, failure, and uncertain outcomes.
4. **Commit booking state safely:** Confirm the booking only when the required reservation and payment conditions are satisfied.
5. **Handle partial failure:** If payment succeeds but the booking cannot be completed, initiate a compensating action such as a refund, and reconcile the outcome.
6. **Send confirmations reliably:** Record a confirmation event in the database transaction, using an outbox pattern or equivalent durable mechanism. A separate worker sends the message and retries safely.

The exact workflow depends on whether inventory is reserved before payment, whether payment authorization can be reversed, and the guarantees provided by the payment service.

**Marking:**

- 1 point: durable state and idempotency.
- 1 point: concurrency-safe availability handling.
- 1 point: payment outcome and compensation.
- 1 point: reliable confirmation delivery.
- 2 points: integration of the workflow and failure recovery.

**7.3. ACID and external services**

A database transaction can make its own database operations atomic, consistent, isolated, and durable according to the configured guarantees.

It cannot automatically roll back an independent payment service because the payment service maintains its own state and transaction boundary. A distributed transaction protocol may be possible only when all participating systems support compatible coordination. Otherwise, idempotency, durable state transitions, retries, and compensating actions are practical alternatives.

**Marking:**

- 1 point: describes database transaction guarantees.
- 1 point: explains the separate external transaction boundary.
- 1 point: identifies a practical coordination approach.

**7.4. Recovery and monitoring**

Suitable controls include:

- Reconciliation jobs comparing successful payments against confirmed bookings.
- Alerts for bookings stuck in intermediate states.
- Durable audit records and payment identifiers.
- Idempotent retries and monitored compensation/refund workflows.

**Marking:** 1 point per valid operational control.

### Question 8. Integrated analytics and processing architecture (15 points)

**8.1. Batch versus stream**

- **Fraud detection:** Stream processing evaluates payment events as they arrive, enabling decisions within seconds.
- **Monthly revenue:** Batch processing aggregates a defined period's completed transactions and reconciles late events, corrections, and refunds.

**Marking:** 2 points per recommendation and justification.

**8.2. Historical data design**

Store the agreed price on each order line, rather than deriving historical sales from the current product price. Calculate revenue from the transaction quantity and stored unit price, while applying clearly defined rules for completed orders, cancellations, refunds, and adjustments.

Keep durable transaction records and identifiers so events can be matched to their original transactions and duplicates can be detected.

**Marking:** 3 points for historical price preservation and reliable transaction accounting.

**8.3. Availability and durability**

A suitable architecture might include:

- Multiple application servers behind a load balancer.
- Health checks and automatic routing away from failed application servers.
- A database with durable transaction logging and appropriate commit guarantees.
- A standby replica with a tested failover procedure.
- Regular backups and tested recovery procedures.

**Trade-offs:** Synchronous replication can increase write latency. Asynchronous replication can lose recently committed changes if the primary fails before the replica receives them. Backups support recovery but do not guarantee immediate availability.

**Marking:**

- 2 points: application availability.
- 1 point: database durability and recovery.
- 1 point: valid limitation.

**8.4. Integrated system design**

A coherent architecture could work as follows:

1. The transactional database records completed payment and order-line details durably.
2. A reliable event mechanism publishes payment events for near-real-time fraud detection. An outbox pattern can prevent the database update and event publication from becoming inconsistent.
3. The streaming system produces operational alerts, which may initially be provisional while events are delayed or corrected.
4. A monthly batch job reads the authoritative transaction records and reconciles payments, completed orders, refunds, and adjustments.
5. The reporting system records the reporting period and reconciliation status so the monthly total can be reproduced and audited.

The streaming result supports low-latency operational decisions; the reconciled financial report is based on the defined accounting rules and the complete available transaction data.

**Marking:**

- 1 point: durable transactional source.
- 1 point: event publication and streaming.
- 1 point: monthly reconciliation.
- 1 point: explains provisional versus reconciled results.

---

# Final Revision Checklist

Use this checklist after completing all three exams.

## Concepts

- [ ]Distinguish latency, throughput, reliability, scalability, availability, and maintainability.
- [ ]Explain conceptual, logical, and physical modeling.
- [ ]Understand primary keys, foreign keys, and referential integrity.
- [ ]Distinguish faults, errors, and failures.
- [ ]Explain ACID properties and transaction isolation.
- [ ]Compare batch and stream processing.

## SQL

- [ ]Write `SELECT`, `WHERE`, and `ORDER BY` queries.
- [ ]Use `GROUP BY`, `COUNT`, `SUM`, and `AVG`.
- [ ]Know when to use `HAVING` instead of `WHERE`.
- [ ]Use inner and left joins correctly.
- [ ]Find missing relationships with `NOT EXISTS` or a left anti-join.
- [ ]Use scalar subqueries and conditional updates.
- [ ]Understand how `NULL` affects aggregates and comparisons.
- [ ]Recognize when joins may duplicate rows.

## Database design

- [ ]Model one-to-many and many-to-many relationships.
- [ ]Design junction tables and composite keys.
- [ ]Identify partial and transitive dependencies.
- [ ]Normalize relations to reduce redundancy and anomalies.
- [ ]Place relationship-specific attributes in the correct relation.
- [ ]Preserve historical transaction values when current reference data changes.
- [ ]Revise key designs when requirements change.

## Performance and reliability

- [ ]Propose composite indexes based on query predicates and ordering.
- [ ]Explain index selectivity, coverage, and maintenance overhead.
- [ ]Interpret execution plans and performance measurements.
- [ ]Explain B-tree splits, balancing, and parent updates.
- [ ]Distinguish replication from backups.
- [ ]Explain the limits of database transactions when external services are involved.
- [ ]Design idempotent retries, compensating actions, and reconciliation workflows.

## Recommended practice order

1. Complete Exam 1 to establish foundational accuracy.
2. Complete Exam 2 under timed conditions, focusing on SQL and design decisions.
3. Complete Exam 3 without consulting previous solutions, emphasizing integrated reasoning.
4. Review every incorrect answer and rewrite the solution from memory.
5. Reattempt difficult SQL questions until you can explain both the query and its correctness.

**Scope note:** These practice exams are based on the T0–T2 material supplied in the conversation. They are designed as a self-contained revision set, not as a claim that every topic from the original lectures is covered.
