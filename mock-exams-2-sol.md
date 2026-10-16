# Data Management 2026–2027 — Three Practice Exams with Comprehensive Solutions

The following solution pack is structured as a GitHub-ready Markdown document (`.md`). It covers all three 120-minute practice exams, with model answers, executable SQL, explanations, marking guidance, and notes on common mistakes.

The solutions follow the original progression:

- Exam 1 — Core Concepts: foundational terminology, data modeling, normalization, SQL fundamentals, and system reliability.
- Exam 2 — Applied Problems: multi-table SQL, aggregation, schema design, indexing, transactions, and performance troubleshooting.
- Exam 3 — Comprehensive Challenge: integrated SQL, advanced normalization scenarios, composite indexes, B-tree behavior, transaction correctness, and fault-tolerant system design.

Each exam is worth 100 points, with a suggested duration of 120 minutes. The solutions explain not only what the correct answer is, but why it works and what alternative answers may also receive credit.

The material below is self-contained and can be saved as `data-management-2026-2027-practice-exams-solutions.md`.

Writing

# Data Management 2026–2027

# Practice Exams 1–3: Comprehensive Solutions and Explanations

**Subject:** Data Management\
**Level:** University\
**Format:** SQL, database design, normalization, indexing, transactions, and system reliability\
**SQL dialect:** Standard SQL where possible\
**Purpose:** Self-assessment, exam preparation, and revision

---

# Exam 1 — Core Concepts

## Section A — Multiple-choice solutions

### Q1. Reliability

**Correct answer: B — Reliability**

Reliability is the ability of a system to perform its required functions correctly over time, including recovering appropriately from failures.

- Maintainability concerns how easily a system can be repaired or modified.
- Throughput measures the amount of work completed per unit of time.
- Normalization concerns the organization of relational data.

### Q2. Throughput

**Correct answer: C — Throughput**

A system processing 8,000 requests per second has a throughput of 8,000 requests/second.

Latency measures the time taken by an individual operation, not the number of operations completed per second.

### Q3. Physical data model

**Correct answer: C — Physical**

The physical model describes implementation-specific decisions, including:

- Database-specific data types.
- Index definitions.
- Storage structures.
- Partitioning and physical organization.

The conceptual model describes business entities and relationships, while the logical model describes attributes, keys, and relationships independently of many implementation details.

### Q4. Primary keys and foreign keys

**Correct answer: C**

A foreign key references an eligible key in the same table or another table, enforcing referential integrity.

A primary key must be unique and cannot contain NULL values. A foreign key does not generally have to be unique because many rows may reference the same parent row.

### Q5. Many-to-many relationships

**Correct answer: B — A junction table**

A many-to-many relationship is represented using an associative or junction table.

For example:

```
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(80) NOT NULL
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL
);

CREATE TABLE Enrolments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id)
        REFERENCES Students(student_id),
    FOREIGN KEY (course_id)
        REFERENCES Courses(course_id)
);
```

The composite primary key prevents a student from being registered for the same course more than once.

### Q6. Second Normal Form

**Correct answer: B — 2NF**

Second Normal Form eliminates partial dependencies of non-prime attributes on proper subsets of candidate keys.

For example:

```
OrderLine(OrderID, ProductID, ProductName, Quantity)
```

Suppose the primary key is `(OrderID, ProductID)`.

If `ProductID -> ProductName`, then `ProductName` depends on only part of the composite key. This violates 2NF.

The solution is to place product information in a separate `Product` table.

### Q7. DROP TABLE

**Correct answer: C — DROP TABLE**

```
DROP TABLE Students;
```

This removes the table definition and its stored data, subject to database permissions and dependency rules.

Compare:

```
DELETE FROM Students;
```

`DELETE` removes rows but normally preserves the table structure.

### Q8. HAVING

**Correct answer: B — HAVING**

`WHERE` filters rows before grouping. `HAVING` filters groups after aggregation.

```
SELECT course_id, AVG(mark) AS average_mark
FROM Enrolments
GROUP BY course_id
HAVING AVG(mark) >= 60;
```

### Q9. LEFT JOIN

**Correct answer: C — LEFT JOIN**

A left join preserves every row from the left table, even when there is no matching row on the right.

Unmatched right-side columns are returned as NULL.

### Q10. Index trade-offs

**Correct answer: C**

Indexes can improve suitable read queries but introduce costs:

- Additional storage.
- Maintenance during inserts, updates, and deletes.
- Possible overhead during index creation and rebuilding.

An index does not automatically improve every query.

---

## Section B — Short-answer solutions

### Q11. Reliability, scalability, and maintainability

**Reliability:** The ability of a system to operate correctly and recover appropriately from failures.

Example: A database recovers committed transactions after a server crash.

**Scalability:** The ability of a system to accommodate increasing data volumes or workloads.

Example: A service adds database replicas or computing capacity as its user base grows.

**Maintainability:** The ease with which a system can be understood, repaired, modified, and operated.

Example: Well-normalized schemas, clear naming, and documented migrations make changes safer.

**Marking:** Award one point for each correct definition and relevant example.

### Q12. Conceptual, logical, and physical modeling

**Conceptual model**

Describes the business entities and their relationships.

Example: Students enrol in courses.

**Logical model**

Defines attributes, primary keys, foreign keys, and relationship cardinalities.

Example:

```
Student(StudentID PK, StudentName)
Course(CourseID PK, CourseTitle)
Enrolment(StudentID PK/FK, CourseID PK/FK)
```

**Physical model**

Defines implementation details for a particular DBMS.

Example: Choosing integer data types, creating indexes, and selecting a partitioning strategy.

The distinction matters because business requirements should drive the design rather than being constrained prematurely by a particular storage implementation.

### Q13. ACID properties

For a transfer of €200 between two accounts:

**Atomicity:** The debit and credit happen together or neither takes effect.

**Consistency:** Database constraints and business rules remain satisfied. For example, the transaction must not leave the database in an invalid state under its defined rules.

**Durability:** Once committed, the changes survive subsequent failures according to the database's durability guarantees.

The fourth ACID property is **Isolation**, which concerns interference between concurrent transactions.

### Q14. Normalization and transitive dependencies

Given:

```
EmployeeID -> DepartmentID
DepartmentID -> DepartmentName
```

Department name is determined by department ID, which is determined by employee ID.

Storing `DepartmentName` in every employee row introduces redundancy and a transitive dependency.

Possible anomalies include:

- **Update anomaly:** A department rename requires multiple employee records to be updated.
- **Insertion anomaly:** A department cannot easily be recorded before an employee belongs to it.
- **Deletion anomaly:** Deleting the last employee could inadvertently remove the only stored copy of department information.

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

---

## Section C — SQL solutions

Use the following schema:

```
Students(student_id, name, city, age)
Courses(course_id, course_name)
Enrolments(student_id, course_id, mark)
```

### Q15. Retrieve adult students

```
SELECT name, age
FROM Students
WHERE age >= 18
ORDER BY age DESC;
```

**Explanation:**

- `WHERE age >= 18` excludes students younger than 18.
- `ORDER BY age DESC` sorts from oldest to youngest.

### Q16. Average mark by course

```
SELECT course_id,
       AVG(mark) AS average_mark
FROM Enrolments
GROUP BY course_id
HAVING AVG(mark) >= 60;
```

**Explanation:**

1. `GROUP BY course_id` creates a group for each course represented in `Enrolments`.
2. `AVG(mark)` calculates the average of non-NULL marks.
3. `HAVING` retains only groups whose average is at least 60.

Courses with no enrolment records do not appear in this query.

### Q17. Every student and their course IDs

```
SELECT s.name, e.course_id
FROM Students AS s
LEFT JOIN Enrolments AS e
    ON s.student_id = e.student_id;
```

A `LEFT JOIN` ensures that students without enrolments are retained. Their `course_id` is NULL.

An inner join would incorrectly exclude those students.

### Q18. Update one student's city

```
UPDATE Students
SET city = 'Madrid'
WHERE student_id = 12;
```

Without the `WHERE` clause, every student row would be updated to Madrid.

**Safety recommendation:** Before executing the update, verify the target row:

```
SELECT student_id, name, city
FROM Students
WHERE student_id = 12;
```

### Q19. SQL command categories

| Command | Category | Purpose |
| --- | --- | --- |
| `CREATE TABLE` | DDL | Defines database structures |
| `SELECT` | DQL | Retrieves data |
| `UPDATE` | DML | Modifies data |
| `COMMIT` | TCL | Commits a transaction |

DCL covers access-control commands such as `GRANT` and `REVOKE`.

---

## Section D — Database design and system reasoning

### Q20. Customers, orders, and products

**Customer–Order:** One-to-many.

A customer may place several orders, but each order belongs to one customer.

**Order–Product:** Many-to-many.

An order may contain several products, and a product may occur in multiple orders.

A suitable design is:

```
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE,
    FOREIGN KEY (customer_id)
        REFERENCES Customers(customer_id)
);

CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL
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

`OrderItems` resolves the many-to-many relationship.

The `unit_price` column is useful because it can preserve the price agreed when the order was placed, even if the current product price changes.

If the same product may appear on multiple lines in a single order, introduce a line identifier instead of using `(order_id, product_id)` as the sole key.

### Q21. Choosing a data model

**Star schema:** Best suited to many analytical workloads involving a central fact table and directly connected dimensions.

**Hierarchical model:** Suitable for tree structures in which each child has one parent.

**Snowflake schema:** Suitable for analytical systems where dimensions are normalized into additional related tables.

**Relational model:** A general-purpose model for structured data, keys, constraints, and transactional operations.

For a reporting system with normalized dimensions, snowflake is the most appropriate of the listed choices. For a straightforward business intelligence system analyzing sales by product, region, and month, star is often preferable.

### Q22. Batch versus stream processing

**Workload A: Previous day's sales totals**

Use batch processing. The workload processes a completed period of data at scheduled intervals.

**Workload B: Suspicious payments as transactions arrive**

Use stream processing. Events are evaluated as they arrive, supporting low-latency detection.

These approaches can coexist. A streaming system may identify suspicious transactions immediately while a batch process performs end-of-day reconciliation.

### Q23. Indexing city and age

Query:

```
SELECT *
FROM Students
WHERE city = 'Madrid'
  AND age >= 20;
```

A reasonable index is:

```
CREATE INDEX idx_students_city_age
ON Students (city, age);
```

**Why this order?**

The query applies an equality condition to `city` and a range condition to `age`. In a typical B-tree index, placing the equality column first allows the DBMS to narrow the search to the relevant city and then scan the qualifying age range.

**Benefit:** The database may examine fewer rows than with a full table scan.

**Cost:** The index requires storage and must be maintained during relevant writes.

Whether the index is beneficial depends on selectivity, table size, the DBMS, and the optimizer's chosen execution plan.

### Q24. Fault, error, and failure

- **Fault:** The faulty disk.
- **Error:** The corrupted data or invalid internal state resulting from the disk problem.
- **Failure:** The externally observable incorrect result returned to the user.

A fault can cause an error, which can subsequently produce a failure. Appropriate detection, redundancy, validation, and recovery mechanisms can prevent some faults from reaching users.

---

# Exam 2 — Applied Problems

## Section A — Multiple-choice solutions

| Question | Answer | Explanation |
| --- | --- | --- |
| Q1 | B — Latency | Elapsed time for an operation. |
| Q2 | B | `ProductName` depends on only `ProductID`, part of the composite key. |
| Q3 | B — LEFT JOIN | Preserves departments with no employees. |
| Q4 | C | `HAVING` filters groups after aggregation. |
| Q5 | A | More indexes generally increase write-maintenance work. |
| Q6 | B — Batch | A completed month's data can be processed periodically. |
| Q7 | B | B+ tree leaf nodes are commonly linked for efficient range scans. |
| Q8 | C — Isolation | Controls the effects of concurrent transactions. |

## Section B — SQL problem-solving solutions

Assume the following tables:

```
Departments(department_id, department_name)
Employees(employee_id, employee_name, department_id, salary)
Projects(project_id, project_name)
Assignments(employee_id, project_id, hours_worked)
```

### Q9. Employees earning more than 50,000

```
SELECT employee_name, salary
FROM Employees
WHERE salary > 50000
ORDER BY salary DESC;
```

The filter is strictly greater than 50,000, so an employee earning exactly 50,000 is excluded.

### Q10. Departments with at least three employees

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

**Explanation:**

- `INNER JOIN` excludes departments without employees.
- `GROUP BY` produces one result per department.
- `COUNT(e.employee_id)` counts employees.
- `HAVING` applies both aggregate conditions.

The average ignores NULL salaries. Consequently, a department may have three employees but fewer than three non-NULL salary values.

### Q11. Every department, including empty departments

```
SELECT d.department_id,
       d.department_name,
       COUNT(e.employee_id) AS employee_count
FROM Departments AS d
LEFT JOIN Employees AS e
    ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name;
```

**Important distinction:**

For an empty department, the left join still generates one row with NULL employee columns.

Therefore:

```
COUNT(*)
```

would count that joined row as one, while:

```
COUNT(e.employee_id)
```

returns zero because `employee_id` is NULL.

### Q12. Employees without project assignments

Solution using `NOT EXISTS`:

```
SELECT e.employee_id, e.employee_name
FROM Employees AS e
WHERE NOT EXISTS (
    SELECT 1
    FROM Assignments AS a
    WHERE a.employee_id = e.employee_id
);
```

The subquery checks whether any assignment exists for each employee.

An employee is returned only when no matching assignment exists.

An equivalent solution is:

```
SELECT e.employee_id, e.employee_name
FROM Employees AS e
LEFT JOIN Assignments AS a
    ON e.employee_id = a.employee_id
WHERE a.employee_id IS NULL;
```

### Q13. Projects with more than 100 assigned hours

```
SELECT p.project_id,
       p.project_name,
       SUM(a.hours_worked) AS total_hours
FROM Projects AS p
INNER JOIN Assignments AS a
    ON p.project_id = a.project_id
GROUP BY p.project_id, p.project_name
HAVING SUM(a.hours_worked) > 100;
```

`SUM` calculates the total hours per project. `HAVING` excludes projects whose total is 100 or less.

The inner join also excludes projects without assignments, as required.

### Q14. Increase one employee's salary by 5%

```
UPDATE Employees
SET salary = salary * 1.05
WHERE employee_id = 25;
```

Without the `WHERE` clause, all employees would receive the increase.

**Safe procedure:**

```
SELECT employee_id, employee_name, salary
FROM Employees
WHERE employee_id = 25;
```

Verify the target, perform the update, and check the result.

Where supported, a transaction provides an additional safety mechanism:

```
BEGIN;

UPDATE Employees
SET salary = salary * 1.05
WHERE employee_id = 25;

SELECT employee_id, employee_name, salary
FROM Employees
WHERE employee_id = 25;

COMMIT;
```

If verification reveals a problem before the commit, the transaction can be rolled back. Exact transaction syntax and behavior depend on the DBMS.

---

## Section C — Data modeling and normalization

### Q15. Course registration database

A normalized design is:

```
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(100) NOT NULL
);

CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseTitle VARCHAR(150) NOT NULL
);

CREATE TABLE Registration (
    StudentID INT,
    CourseID INT,
    RegistrationDate DATE NOT NULL,
    FinalMark DECIMAL(5,2),
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID)
        REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID)
        REFERENCES Courses(CourseID)
);
```

The primary keys identify individual students and courses.

The registration table resolves the many-to-many relationship. `RegistrationDate` and `FinalMark` describe a specific student-course association, so they belong in that table rather than in either entity table.

The composite primary key assumes that a student can register for a particular course only once. If retakes are permitted, the key must be redesigned.

### Q16. Partial and transitive dependencies

Given:

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

Primary key:

```
(StudentID, CourseID)
```

Dependencies:

```
StudentID -> StudentName
CourseID -> CourseTitle, InstructorID
InstructorID -> InstructorName
(StudentID, CourseID) -> FinalMark
```

**Partial dependency**

```
StudentID -> StudentName
```

`StudentName` depends on only one component of the composite key.

Similarly, `CourseTitle` and `InstructorID` depend on `CourseID` alone.

**Transitive dependency**

```
CourseID -> InstructorID -> InstructorName
```

The instructor name is determined indirectly through the course's instructor identifier.

**Normalized decomposition:**

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
    StudentID PK/FK,
    CourseID PK/FK,
    FinalMark
)
```

This reduces duplicated student, course, and instructor information while preserving the relationships.

### Q17. Composite index for department and salary

Query:

```
SELECT employee_name
FROM Employees
WHERE department_id = 4
ORDER BY salary DESC;
```

Candidate index:

```
CREATE INDEX idx_employee_department_salary
ON Employees (department_id, salary DESC);
```

**Column order:** `department_id` is first because the query filters on department equality. The salary key then orders entries within that department.

**Potential benefit:** The DBMS may retrieve the department's employees in salary order without a separate sorting operation.

**Limitation:** The index does not necessarily contain `employee_name`. The DBMS may still need to retrieve table rows to obtain that column.

Whether a sort is avoided depends on the DBMS, the index implementation, and the execution plan.

---

## Section D — Reliability and system design

### Q18. Stale inventory cache

**Cause**

The database update succeeds, but the corresponding cache entry remains unchanged. Subsequent reads return stale data.

**Possible solution**

Invalidate the cache entry after the database transaction commits, or update it using a reliable cache-consistency mechanism.

For more robust systems, consider:

- Retry mechanisms for failed invalidations.
- Version numbers or timestamps.
- Event-driven cache updates.
- Transactional outbox patterns where database changes and publication events must be coordinated.

**Consistency concern**

If inventory decisions rely on stale cached quantities, concurrent purchases may lead to overselling.

**Testing**

1. Record the initial inventory.
2. Execute a purchase.
3. Read inventory through the normal application path.
4. Confirm that the new value is returned.
5. Repeat with concurrent purchases and simulated cache-update failures.
6. Verify recovery and retry behavior.

### Q19. Availability and fault tolerance

Two improvements are appropriate.

**1\. Replication and failover**

Maintain a replica that can take over when the primary server fails.

Benefit: Reduces downtime.

Limitation: Failover can take time, and asynchronous replication can lose recently acknowledged changes in some failure scenarios.

**2\. Backups and tested recovery procedures**

Maintain recoverable backups and regularly test restoration.

Benefit: Supports recovery from corruption, accidental deletion, and severe infrastructure failures.

Limitation: Restoration may take considerable time, so backups alone do not provide continuous availability.

Other valid approaches include redundant infrastructure, health monitoring, automated recovery, and disaster recovery planning.

### Q20. B-tree insertion and page splits

**Page split**

When a B-tree node becomes full, the database may divide its contents across multiple nodes and promote or otherwise propagate a separator key according to the tree's implementation.

**Parent changes**

The parent must be updated to represent the new key ranges or child pointers. If the parent also becomes full, the split can propagate upward. A root split may increase the tree's height.

**Why balance matters**

A balanced tree keeps search paths relatively short. Typical B-tree operations have logarithmic complexity in the number of indexed entries, reducing the number of pages that need to be visited.

**Why rows need not follow index order**

An index is a separate access structure containing keys and references to data. Unless the table is specifically organized or clustered by that index, physical row storage need not follow key order.

---

# Exam 3 — Comprehensive Challenge

## Section A — Conceptual challenge

### Q1. Average latency and slow requests

Average latency can conceal a long tail of slow requests.

For example, most requests might take 50 milliseconds, while a small proportion take 20 seconds.

Useful additional metrics include:

- 95th-percentile latency.
- 99th-percentile latency.
- A latency histogram.

The 99th percentile indicates the latency below which approximately 99% of observed requests complete.

### Q2. Duplicating department names

Storing department names in every employee record may reduce some joins but introduces redundancy.

Given:

```
EmployeeID -> DepartmentID
DepartmentID -> DepartmentName
```

department name depends transitively on employee ID.

Renaming a department may require many rows to be updated, risking inconsistent values.

A normalized solution stores the department name once in a separate `Department` table and references it through `DepartmentID`.

### Q3. Durability versus atomicity

**Durability** means that committed changes persist through subsequent failures under the database's durability guarantees.

**Atomicity** means that a transaction's operations succeed as a unit or are rolled back as a unit.

A transaction can be atomic during execution yet still require durability mechanisms, such as logging and recovery, to ensure committed changes survive a crash.

### Q4. Index benefit and limitations

An appropriate index may:

- Reduce the number of rows examined.
- Support filtering.
- Supply rows in the required order.
- Reduce sorting work.

However, an index can require additional storage, increase write costs, and still require table lookups for columns not stored in the index.

The optimizer may also choose a table scan when it estimates that scanning is cheaper.

### Q5. Why conceptual modeling comes first

Conceptual modeling clarifies business entities, relationships, and constraints before physical implementation decisions are made.

For example, assuming that an order can contain only one product could result in a schema that must be substantially redesigned when multi-product orders become a requirement.

Understanding the business rules first reduces the risk of expensive structural changes.

---

## Section B — Integrated SQL case study

Schema:

```
Students(student_id, student_name, city)
Courses(course_id, course_name, department)
Enrolments(student_id, course_id, mark, registration_date)
```

The primary key of `Enrolments` is `(student_id, course_id)`. Marks may be NULL until grading is complete.

### Q6. Students with at least one mark of 80 or higher

```
SELECT DISTINCT s.student_name
FROM Students AS s
JOIN Enrolments AS e
    ON s.student_id = e.student_id
WHERE e.mark >= 80;
```

The join connects students to their enrolments. The condition selects marks of at least 80.

`DISTINCT` prevents a student with multiple qualifying enrolments from appearing multiple times.

If different students can share the same name and each student must appear separately, return `student_id` as well.

### Q7. Course averages

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

`COUNT(e.mark)` counts non-NULL marks, unlike `COUNT(*)`, which counts rows.

`AVG(e.mark)` also ignores NULL values.

Therefore, the query correctly requires at least five graded enrolments and an average mark of at least 70.

### Q8. Students without registrations

```
SELECT s.student_id, s.student_name
FROM Students AS s
WHERE NOT EXISTS (
    SELECT 1
    FROM Enrolments AS e
    WHERE e.student_id = s.student_id
);
```

For each student, the subquery searches for a matching enrolment. `NOT EXISTS` returns the student only if no such record exists.

### Q9. Department-level average marks

```
SELECT c.department,
       AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
GROUP BY c.department
HAVING AVG(e.mark) > 65;
```

The query groups enrolments by department and calculates the average of non-NULL marks.

Departments without graded marks have a NULL average, which does not satisfy `AVG(e.mark) > 65`.

### Q10. Interpret a SQL query

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

**1\. What does****`WHERE`****remove?**

It removes enrolment rows with marks below 50 and rows whose marks are NULL. Only rows satisfying `e.mark >= 50` remain.

**2\. What does****`COUNT(*)`****count?**

It counts the remaining joined rows in each department group. In this schema, each enrolment belongs to one course, so these are qualifying enrolment records.

**3\. What does****`HAVING COUNT(*) >= 10`****mean?**

It retains departments with at least ten qualifying enrolment rows.

**4\. Why is this not the average across all enrolments?**

The `WHERE` clause removes marks below 50 before aggregation. Therefore, the average includes only qualifying marks, not all recorded marks.

### Q11. Correct an aggregate-filtering error

Incorrect query:

```
SELECT c.department, AVG(e.mark)
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
WHERE AVG(e.mark) >= 60
GROUP BY c.department;
```

**Problem:** `WHERE` filters individual rows before aggregation. It cannot directly filter groups based on `AVG(e.mark)`.

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

The `HAVING` clause evaluates the aggregate after grouping.

### Q12. Update one enrolment mark

```
UPDATE Enrolments
SET mark = 75
WHERE student_id = 14
  AND course_id = 22;
```

Both conditions identify the intended composite-key row.

Using only `student_id` could update that student's marks for multiple courses.

---

## Section C — Database design and normalization

### Q13. Normalize a sales record

Original table:

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

Dependencies:

```
OrderID -> OrderDate, CustomerID
CustomerID -> CustomerName
ProductID -> ProductName
(OrderID, ProductID) -> UnitPrice, Quantity
```

**Normalized schema:**

```
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100) NOT NULL
);

CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(150) NOT NULL
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE NOT NULL,
    CustomerID INT NOT NULL,
    FOREIGN KEY (CustomerID)
        REFERENCES Customers(CustomerID)
);

CREATE TABLE OrderLine (
    OrderID INT,
    ProductID INT,
    UnitPrice DECIMAL(10,2) NOT NULL,
    Quantity INT NOT NULL,
    PRIMARY KEY (OrderID, ProductID),
    FOREIGN KEY (OrderID)
        REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID)
        REFERENCES Products(ProductID)
);
```

**Partial dependency**

The composite key is `(OrderID, ProductID)`, but:

```
OrderID -> OrderDate, CustomerID
```

These attributes depend on only part of the key.

**Transitive dependencies**

```
CustomerID -> CustomerName
ProductID -> ProductName
```

Storing customer and product names in every order line introduces redundancy.

**Why preserve****`UnitPrice`****in****`OrderLine`****?**

`UnitPrice` represents the agreed price for that particular order line. The current product price may change later, but historical sales records must preserve the original transaction price.

Replacing historical order-line prices with the current product price would corrupt the financial meaning of past orders.

### Q14. Allowing course retakes

If a student may register for the same course multiple times, `(StudentID, CourseID)` no longer uniquely identifies each registration attempt.

A suitable design is:

```
CREATE TABLE Registration (
    RegistrationID INT PRIMARY KEY,
    StudentID INT NOT NULL,
    CourseID INT NOT NULL,
    RegistrationDate DATE NOT NULL,
    FinalMark DECIMAL(5,2),
    FOREIGN KEY (StudentID)
        REFERENCES Students(student_id),
    FOREIGN KEY (CourseID)
        REFERENCES Courses(course_id)
);
```

Each attempt receives its own `RegistrationID`.

An alternative is a composite key such as `(StudentID, CourseID, AttemptNumber)`. This works when the application explicitly manages attempt numbers.

### Q15. Star versus snowflake schema

**Star schema**

A central fact table connects directly to dimension tables.

Advantages:

- Relatively simple analytical queries.
- Fewer joins.
- Often convenient for reporting and BI tools.

Disadvantages:

- Dimension attributes may be duplicated.
- Dimension tables may be wider and less normalized.

**Snowflake schema**

Dimensions are divided into additional related tables.

Advantages:

- Less duplicated dimension data.
- Better representation of normalized hierarchies.

Disadvantages:

- More joins.
- Potentially more complex queries and reporting logic.

**Recommendation:** For reporting by day, product, store, and region, a star schema is a strong default. It offers straightforward joins and supports typical analytical queries efficiently.

A snowflake schema may be preferable if dimension hierarchies are complex or the reduction in redundancy is especially valuable.

---

## Section D — Performance and systems reasoning

### Q16. Composite index selection

Query:

```
SELECT employee_id, employee_name, salary
FROM Employees
WHERE department_id = 7
  AND salary >= 60000
ORDER BY salary DESC;
```

Candidate index:

```
CREATE INDEX idx_employee_department_salary
ON Employees (department_id, salary DESC);
```

**Column order**

The equality predicate on `department_id` comes first, followed by the salary range and ordering column.

**Filtering and sorting**

The index may narrow the search to department 7 and provide qualifying salary entries in descending order, reducing scanning and sorting costs.

**Why table reads may remain necessary**

The index does not necessarily contain `employee_name`. The database may still need to retrieve table rows to obtain that column.

Index support for ordering and the exact execution plan depend on the DBMS.

### Q17. B-tree insertion and deletion

**Insertion into a full leaf**

A full leaf page may split into two pages. A separator key or equivalent routing information is propagated to the parent.

If the parent becomes full, the split may propagate upward. A root split can increase the height of the tree.

**Deletion that makes a node underfull**

The database may rebalance the tree by redistributing entries between siblings or merging nodes, depending on the implementation and occupancy rules.

The parent may need to update separator keys or child references.

**Why balance matters**

A balanced B-tree keeps the number of levels relatively small. Searches, insertions, and deletions typically require logarithmic work in the number of indexed entries.

### Q18. Slow queries and too many indexes

**Why the original query may have been slow**

Possible reasons include:

- The indexed column has low selectivity.
- The query returns a large proportion of the table.
- The index does not match the filter and ordering pattern.
- The query applies a function or conversion that prevents efficient index use.
- The execution plan is suboptimal.
- Table lookups or disk I/O dominate the cost.

An index existing on a column does not guarantee that the DBMS will use it.

**Why additional indexes slow inserts**

Each applicable index must be maintained when rows are inserted. Adding indexes increases the amount of work per insert and may introduce page splits, additional logging, and extra storage requirements.

**Two recommended steps**

1. Examine the execution plan and runtime statistics to determine whether the original query uses the index efficiently.
2. Evaluate index selectivity and workload requirements. Remove redundant or unused indexes and design only those that materially improve important queries.

Also consider current statistics, data distribution, partitioning, and whether the query can be rewritten.

---

## Section E — Transactions and system reliability

### Q19. Failure during a bank transfer

A transfer involves two operations:

1. Deduct €500 from Account A.
2. Add €500 to Account B.

If the first operation succeeds but the server fails before the second completes, the accounts may become inconsistent if the operations are not handled atomically.

A transaction groups both operations:

```
BEGIN;

UPDATE Accounts
SET balance = balance - 500
WHERE account_id = 'A';

UPDATE Accounts
SET balance = balance + 500
WHERE account_id = 'B';

COMMIT;
```

If the application detects a failure before commit, it should roll back the transaction where possible.

**Most relevant ACID properties:**

- **Atomicity:** Both account changes succeed together or neither is committed.
- **Consistency:** Defined account and transaction rules remain satisfied.
- **Durability:** A successful commit survives subsequent failures under the database's durability guarantees.
- **Isolation:** Concurrent transfers do not interfere in ways prohibited by the selected isolation level.

The example illustrates the principle, but a production implementation should also validate sufficient funds, verify affected-row counts, and handle transaction errors. Database-specific syntax and recovery behavior must be considered.

### Q20. Fault-tolerant online booking system

A suitable architecture combines redundancy, transaction-safe storage, and tested recovery.

**Availability**

- Run multiple application instances.
- Use a load balancer and health checks.
- Maintain database replication and an appropriate failover procedure.

**Data durability**

- Commit bookings through database transactions.
- Configure durable database logging and recovery.
- Maintain regular backups and test restoration.
- Ensure that acknowledged bookings are replicated or otherwise recoverable according to the required durability guarantees.

**Important limitation**

Replication alone does not guarantee that every acknowledged transaction survives every failure. Asynchronous replication may lag, while synchronous replication may increase write latency and reduce availability when replicas are unreachable.

A further challenge is preventing duplicate bookings during retries or failover. Idempotency keys, unique constraints, and transaction-safe booking logic help address this problem.

### Q21. Batch and stream integration

**Fraud detection within seconds**

Use stream processing. Each payment event is evaluated shortly after it arrives, enabling low-latency alerts or additional verification.

**Monthly revenue reconciliation**

Use batch processing. A completed period of transactions is aggregated and reconciled against authoritative financial records.

**How they work together**

The streaming system can detect suspicious events immediately, while the batch system later reconciles the complete transaction history and corrects discrepancies.

Both systems should use consistent event identifiers, reliable data ingestion, and clear handling of late or duplicate events.

**Why not use only one approach?**

Stream processing is suitable for immediate reactions but may require more complex event-time, retry, and state-management logic. Batch processing simplifies large-scale periodic calculations but does not naturally provide immediate detection.

Using both allows each workload to use the processing model that best fits its latency and completeness requirements.

---

# Final Revision Checklist

Before the examination, make sure you can explain and apply the following concepts without relying on the answer key.

## SQL

- [ ]`SELECT`, `WHERE`, `ORDER BY`, and `DISTINCT`.
- [ ]`INNER JOIN` versus `LEFT JOIN`.
- [ ]`GROUP BY`, `COUNT`, `SUM`, and `AVG`.
- [ ]`WHERE` versus `HAVING`.
- [ ]`COUNT(*)` versus `COUNT(column)`.
- [ ]`NOT EXISTS` and anti-join patterns.
- [ ]Safe `UPDATE` statements using primary keys or other appropriate filters.
- [ ]NULL handling in comparisons and aggregates.

## Database design

- [ ]Primary keys, foreign keys, and composite keys.
- [ ]One-to-one, one-to-many, and many-to-many relationships.
- [ ]Junction tables and relationship-specific attributes.
- [ ]Conceptual, logical, and physical modeling.
- [ ]1NF, 2NF, 3NF, partial dependencies, and transitive dependencies.
- [ ]Historical transaction data versus current entity attributes.
- [ ]Star versus snowflake schemas.

## Performance

- [ ]Index benefits and write-maintenance costs.
- [ ]Composite index column order.
- [ ]Equality predicates, range predicates, and sorting.
- [ ]B-tree splits, rebalancing, and tree height.
- [ ]Query execution plans and index selectivity.
- [ ]Why an index does not guarantee a faster query.

## Reliability and transactions

- [ ]ACID properties.
- [ ]Fault, error, and failure.
- [ ]Batch versus stream processing.
- [ ]Replication, failover, backup, and recovery.
- [ ]Cache invalidation and stale-data risks.
- [ ]Transaction safety and concurrency considerations.

---

# Exam technique

1. **Read the schema carefully.** Identify primary keys, foreign keys, nullable columns, and relationship cardinalities before writing SQL.
2. **Write the simplest correct query first.** Add aggregation, filtering, and ordering in a deliberate sequence.
3. **Check NULL behavior.** Remember that `AVG(column)` and `COUNT(column)` ignore NULL values.
4. **Distinguish row filtering from group filtering.** Use `WHERE` before aggregation and `HAVING` for aggregate conditions.
5. **Explain design choices.** For normalization and indexing questions, state both the benefit and the trade-off.
6. **Check destructive operations.** Verify the target rows before executing `UPDATE` or `DELETE`.
7. **Use explicit assumptions.** If a question leaves a business rule unspecified, state the assumption that makes your design valid.

**End of solution pack.**

Important note: The complete solutions above correspond to the three exams included in the material available in this conversation. Those versions were originally labeled 60, 75, and 90 minutes, with totals of 50, 60, and 70 points. If you want the solutions to match a subsequently restructured set of three 120-minute, 100-point exams question-for-question, please provide those revised exam papers so I can align the answers and marking schemes precisely.
