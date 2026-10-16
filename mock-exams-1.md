forward to do the same thing using T3-T5.

ChatGPT said:
I’ll generate all three university-style mock exams now, using the T0–T2 material available in this conversation. We’ll treat these as a self-contained set, so you can move on to T3–T5 afterward without needing to preserve this context.

The exams will follow the agreed progression:

Mock Exam 1 — Core Concepts: Definitions, fundamental distinctions, and foundational exercises.

Mock Exam 2 — Applied Problems: SQL query writing, database design, code interpretation, calculations, and debugging.

Mock Exam 3 — Comprehensive Challenge: Integrated scenarios, multi-step SQL problems, index analysis, and subtle conceptual distinctions.

Each exam will include multiple-choice and open-ended questions, practical tasks where appropriate, a point allocation, suggested duration, and a complete answer key with marking guidance.

Scope note: The questions will be based on the T0–T2 compressions provided here. I’ll avoid claiming coverage of lecture-specific material that those compressions do not contain.

Data Management 2026–2027
Mock Exam 1 — Core Concepts
Mock Exam 1
Foundation
Duration

60 minutes
Total marks

50 points
Coverage: T0 foundations · T1 data models and storage · T2 SQL

Instructions

Answer every question.

Choose one answer for each multiple-choice question.

Write SQL statements using standard SQL where possible. State any assumptions.

Marks are awarded for correct reasoning as well as final answers.

The answer key follows the entire exam. Attempt the paper before reading it.

Section A — Multiple choice (10 points)
10 questions × 1 point.

Q1. Which property describes the ability of a system to continue operating correctly and recover from failures?

A. Maintainability
B. Reliability
C. Throughput
D. Normalization

Q2. A service processes 8,000 requests per second. Which performance metric does this describe?

A. Latency
B. Response time
C. Throughput
D. Availability alone

Q3. Which model specifies database-specific column types, indexes, and storage implementation?

A. Conceptual
B. Logical
C. Physical
D. Entity-relationship

Q4. Which statement about primary and foreign keys is correct?

A. A foreign key must always be unique.
B. A primary key may contain NULL values.
C. A foreign key establishes a reference to an eligible key in another or the same table.
D. A primary key is used only to sort rows.

Q5. A relationship connects many students to many courses. What is the standard relational solution?

A. A single Student table with a text field containing every course
B. A junction table linking Student and Course
C. A foreign key only in Student, with no additional structure
D. A separate database for every student

Q6. Which normal form directly addresses partial dependencies on a proper subset of a candidate key?

A. 1NF
B. 2NF
C. 3NF
D. ACID

Q7. Which SQL command removes the table structure as well as its stored data?

A. DELETE
B. UPDATE
C. DROP TABLE
D. SELECT

Q8. Which clause filters groups after aggregation?

A. WHERE
B. HAVING
C. ORDER BY
D. FROM

Q9. Which join returns every row from the left table, including rows without a match on the right?

A. INNER JOIN
B. RIGHT JOIN
C. LEFT JOIN
D. CROSS JOIN

Q10. What is a principal trade-off of adding database indexes?

A. Indexes always improve every query.
B. Indexes eliminate the need for primary keys.
C. Indexes can improve suitable reads but consume storage and increase write overhead.
D. Indexes prevent concurrent transactions.

Section B — Short-answer questions (12 points)
Answer in clear, concise sentences.

Q11. Reliability, scalability and maintainability (3 points)

Define each of the following and give one example of how it matters in a data-intensive application:

Reliability

Scalability

Maintainability

Q12. Modeling levels (3 points)

A university wants to build a database to manage students and course registrations.

Explain what would be specified at the conceptual, logical and physical modeling levels. Give one example of a design decision at each level.

Q13. ACID properties (3 points)

A transaction transfers €200 between two bank accounts. Explain how atomicity, consistency and durability apply to this transaction.

Q14. Normalization (3 points)

Consider the following dependencies:

EmployeeID
→
DepartmentID
EmployeeID→DepartmentID
DepartmentID
→
DepartmentName
DepartmentID→DepartmentName
Explain why storing DepartmentName in every employee record can cause a data anomaly. Describe a suitable decomposition.

Section C — SQL fundamentals (12 points)
Consider the following schema:


sql
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
Q15. Basic retrieval (2 points)

Write a query to retrieve the names and ages of students who are at least 18 years old, ordered by age from oldest to youngest.

Q16. Aggregation (3 points)

Write a query that returns each course_id and the average mark for that course. Only include courses whose average mark is at least 60.

Q17. Joins (3 points)

Write a query to display every student’s name and their course IDs. Students who have no enrolments must also appear.

Q18. Modification and safety (2 points)

Write an SQL statement that changes the city of the student with student_id = 12 to Madrid.

Then explain what would happen if the WHERE clause were omitted.

Q19. SQL command categories (2 points)

Classify each command as DDL, DQL, DML, DCL or TCL:

CREATE TABLE

SELECT

UPDATE

COMMIT

Section D — Database design and system reasoning (16 points)
Q20. Cardinality and schema design (4 points)

An online shop has customers, orders and products.

A customer can place multiple orders.

Each order belongs to one customer.

Each order can contain multiple products.

A product can appear in multiple orders.

Identify the cardinality of each relationship and propose the required tables and foreign keys. Explain how you would represent the order–product relationship.

Q21. Schema selection (3 points)

Choose the most appropriate model for each situation and justify your choices.

A business intelligence system analyzes sales by product, region and month.

An organization stores a parent-child hierarchy in which each child has one parent.

A reporting system has a central sales fact table and dimension tables that are further normalized into related tables.

Choose from: hierarchical, relational, star, snowflake.

Q22. Batch versus stream processing (3 points)

A company has two workloads:

Workload A calculates the previous day's sales totals every night.

Workload B detects suspicious payments as transactions arrive.

For each workload, recommend batch or stream processing and explain why.

Q23. Indexes and performance (3 points)

A query frequently searches for students by city and then filters by age.


sql
SELECT *
FROM Students
WHERE city = 'Madrid'
  AND age >= 20;
Recommend an index. Explain one potential benefit and one cost of maintaining it.

Q24. Faults, errors and failures (3 points)

A database server has a faulty disk. The disk causes a read operation to return corrupted data, and the application consequently returns an incorrect result to the user.

Identify the fault, the error and the failure in this scenario.

Mock Exam 1 — Answer key and marking guide
Stop here if you have not yet attempted the exam.

Section A answers
Q

Answer

Explanation

1

B

Reliability concerns correct operation and recovery from failures.

2

C

Throughput measures operations processed per unit of time.

3

C

Physical modeling defines implementation details.

4

C

Foreign keys reference eligible keys and support referential integrity.

5

B

A junction table represents many-to-many associations.

6

B

2NF addresses partial dependencies.

7

C

DROP TABLE removes the table definition and its data.

8

B

HAVING filters groups after aggregation.

9

C

LEFT JOIN preserves all left-side rows.

10

C

Indexes trade extra storage and write maintenance for potentially faster reads.

Section B model answers
Q11 (3 points)

Reliability: The system operates correctly and recovers from failures. Example: recovering committed records after a server crash.

Scalability: The system can handle increasing workloads or data volumes. Example: adding capacity when the number of users increases.

Maintainability: The system can be understood, modified and operated efficiently. Example: clear schemas and documented database procedures make changes safer.

Award 1 point for each correct definition with a relevant example.

Q12 (3 points)

Conceptual: Identifies business entities and relationships, such as Students enrolling in Courses.

Logical: Specifies attributes, keys and relationships, such as Students(student_id, name) and Enrolments(student_id, course_id).

Physical: Specifies DBMS-specific types, indexes and implementation, such as an integer primary key and an index on a frequently searched field.

Award 1 point per level with an appropriate example.

Q13 (3 points)

Atomicity: Both the debit and credit succeed, or neither is committed.

Consistency: Database constraints and business rules remain satisfied.

Durability: Once the transaction commits, its changes persist through subsequent failures.

Award 1 point per correctly explained property.

Q14 (3 points)

DepartmentName depends on DepartmentID, which in turn is determined by EmployeeID. Storing the department name repeatedly creates a transitive dependency. Changing a department name requires multiple employee records to be updated, potentially producing inconsistent values.

A suitable decomposition is:


text
Employee(EmployeeID PK, DepartmentID FK)
Department(DepartmentID PK, DepartmentName)
Award 1 point for identifying the dependency, 1 for explaining the anomaly, and 1 for the decomposition.

Section C model answers
Q15 (2 points)


sql
SELECT name, age
FROM Students
WHERE age >= 18
ORDER BY age DESC;
1 point for selecting and filtering the correct columns and rows; 1 point for descending age order.

Q16 (3 points)


sql
SELECT course_id, AVG(mark) AS average_mark
FROM Enrolments
GROUP BY course_id
HAVING AVG(mark) >= 60;
AVG(mark) calculates the mean.

GROUP BY course_id calculates a separate mean for each course.

HAVING filters the calculated averages.

Award 1 point each for correct grouping, aggregation and group filtering.

Q17 (3 points)


sql
SELECT s.name, e.course_id
FROM Students AS s
LEFT JOIN Enrolments AS e
    ON s.student_id = e.student_id;
LEFT JOIN preserves students without enrolments. Their course_id will be NULL.

Award 1 point for the join, 1 for the matching condition and 1 for preserving unmatched students.

Q18 (2 points)


sql
UPDATE Students
SET city = 'Madrid'
WHERE student_id = 12;
Without WHERE, every row in Students would be updated to have city = 'Madrid'.

1 point for correct SQL; 1 point for explaining the missing-filter consequence.

Q19 (2 points)

CREATE TABLE — DDL.

SELECT — DQL.

UPDATE — DML.

COMMIT — TCL.

0.5 point per correct classification.

Section D model answers
Q20 (4 points)

Customer–Order: one-to-many.

Order–Product: many-to-many.

Suggested tables:


text
Customer(CustomerID PK, ...)
Order(OrderID PK, CustomerID FK, ...)
Product(ProductID PK, ...)
OrderItem(
    OrderID PK, FK,
    ProductID PK, FK,
    Quantity,
    UnitPrice
)
The OrderItem junction table represents the products within each order. A composite primary key (OrderID, ProductID) prevents duplicate product entries within the same order, assuming one line per product. If multiple lines for the same product are allowed, use a line identifier instead.

Award 1 point for each correct cardinality, 1 for the customer foreign key, and 1 for the junction-table design.

Q21 (3 points)

Star: A fact table connects directly to sales dimensions, supporting analytical queries with a relatively simple structure.

Hierarchical: A tree naturally represents parent-child relationships where each child has one parent.

Snowflake: Dimensions are normalized into additional related tables, reducing dimension-data redundancy at the cost of more joins.

1 point each.

Q22 (3 points)

Workload A — Batch: The previous day's sales are aggregated periodically over a completed data set.

Workload B — Stream: Incoming payments must be processed as events arrive to detect suspicious activity with low delay.

Award 1 point per correct choice and 0.5 per suitable justification.

Q23 (3 points)


sql
CREATE INDEX idx_students_city_age
ON Students (city, age);
Benefit: The index can help locate rows matching the city and then the age range, potentially reducing scanning and sorting work.

Cost: It consumes additional storage and must be maintained when indexed values are inserted, updated or deleted.

Award 1 point for the index definition or correct column order, 1 for the benefit and 1 for the cost.

Q24 (3 points)

Fault: The faulty disk.

Error: The corrupted data read or resulting incorrect internal state.

Failure: The application returns an incorrect result to the user.

Award 1 point per correct distinction.

Data Management 2026–2027
Mock Exam 2 — Applied Problems
Mock Exam 2
Application
Duration

75 minutes
Total marks

60 points
Focus: practical SQL, normalization, transaction reasoning, query behavior, and index analysis.

Instructions

Write executable SQL wherever requested.

Explain important decisions instead of merely naming a technique.

Assume standard SQL unless a DBMS is specified.

If a question is ambiguous, state a reasonable assumption.

Section A — Applied multiple choice (8 points)
8 questions × 1 point.

Q1. An application takes 1.8 seconds to return a query result. Which metric most directly describes this elapsed time?

A. Throughput
B. Latency
C. Availability
D. Scalability

Q2. A table uses (OrderID, ProductID) as its composite primary key. Which statement best describes a violation of 2NF?

A. Quantity depends on the complete composite key.
B. ProductName depends only on ProductID.
C. Both columns are part of the primary key.
D. Each row contains one quantity.

Q3. A reporting query must include all departments, even those with no employees. Which approach is appropriate?

A. INNER JOIN Departments to Employees
B. LEFT JOIN Departments to Employees
C. WHERE Employees.EmployeeID IS NOT NULL after a left join
D. CROSS JOIN Departments and Employees

Q4. Which query returns departments with more than five employees?

A. WHERE COUNT(*) > 5
B. GROUP BY DepartmentID WHERE COUNT(*) > 5
C. GROUP BY DepartmentID HAVING COUNT(*) > 5
D. ORDER BY COUNT(*) > 5

Q5. A frequently queried indexed column is updated thousands of times per second. What is a likely consequence of adding more indexes to the table?

A. Write operations may become more expensive.
B. Writes will always become faster.
C. Indexes require no maintenance.
D. The table will no longer require storage.

Q6. A system must calculate the total sales for the previous month. Which processing approach is usually the simplest fit?

A. Stream processing is mandatory.
B. Batch processing.
C. A primary key alone.
D. A B-tree page split.

Q7. Which statement about a B+ tree index is generally correct?

A. It cannot support range queries.
B. Its leaf nodes are commonly linked to support efficient range scans.
C. It always stores table rows in the same physical order as its keys.
D. It is a synonym for a foreign key.

Q8. A transaction runs two operations concurrently with another transaction. Which ACID property addresses how their concurrent execution is controlled?

A. Atomicity
B. Consistency
C. Isolation
D. Durability

Section B — SQL problem solving (20 points)
Use this schema:


sql
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
Q9. Retrieve employees (3 points)

Write a query to retrieve the employee names and salaries of employees earning more than 50,000, ordered from highest salary to lowest.

Q10. Aggregate by department (4 points)

Write a query returning:

Department name.

Number of employees in that department.

Average employee salary.

Include only departments with at least three employees and an average salary greater than 40,000.

Your query should include only departments that have employees.

Q11. Preserve empty departments (3 points)

Write a query showing every department and the number of employees in it, including departments with zero employees.

Q12. Find unassigned employees (3 points)

Write a query returning the IDs and names of employees who have not been assigned to any project.

Q13. Multi-table query (4 points)

Write a query returning each project name and the total number of hours assigned to it. Include only projects whose total recorded hours exceed 100. Assume hours are non-negative and the intended report includes projects with assignments only.

Q14. Update and delete safety (3 points)

An administrator intends to increase the salary of employee 25 by 5%.

Write the SQL update.

Explain what could happen if the employee filter is omitted.

Give one additional precaution before executing the update.

Section C — Data modeling and normalization (12 points)
Q15. Design a course registration database (5 points)

A university records:

Student ID and student name.

Course ID and course title.

Each student's registration date and final mark for each course.

A student can register for multiple courses, and each course can have multiple students.

Design the tables, specifying primary and foreign keys. Explain where RegistrationDate and FinalMark should be stored and why.

Q16. Identify and remove dependencies (4 points)

Consider:


text
Enrollment(
    StudentID,
    CourseID,
    StudentName,
    CourseTitle,
    InstructorID,
    InstructorName,
    FinalMark
)
Assume the primary key is (StudentID, CourseID) and the following dependencies hold:


text
StudentID -> StudentName
CourseID -> CourseTitle, InstructorID
InstructorID -> InstructorName
(StudentID, CourseID) -> FinalMark
Identify:

A partial dependency.

A transitive dependency.

A suitable normalized decomposition.

Q17. Choose a physical index (3 points)

A database frequently executes:


sql
SELECT employee_name
FROM Employees
WHERE department_id = 4
ORDER BY salary DESC;
Recommend a composite index that could help this query. Explain why index column order matters, and state one reason the index might not eliminate all sorting or data-access costs.

Section D — Reliability and system design (12 points)
Q18. Diagnose a data-system incident (4 points)

An online shop occasionally displays an old stock quantity after a purchase. The database server itself remains operational, but a cache has not been updated after a successful database write.

Explain:

The likely cause of the stale result.

One strategy to reduce this problem.

One reliability or consistency concern.

How you would test that the fix works.

Q19. Availability and fault tolerance (4 points)

A database service runs on a single server. A hardware failure makes the service unavailable until the server is repaired.

Propose two architectural or operational improvements that could reduce downtime. Explain what each improves and mention one limitation.

Q20. B-tree reasoning (4 points)

A B-tree index page becomes full when a new key is inserted.

Explain:

What a page split is.

How the parent node may be affected.

Why balanced tree height matters for search performance.

Why a B-tree index does not necessarily mean physical table rows are stored in key order.

Mock Exam 2 — Answer key and marking guide
Section A answers
Q

Answer

Reason

1

B

Latency measures the time taken by an operation.

2

B

ProductName depends on only part of the composite key.

3

B

A left join preserves all departments.

4

C

HAVING filters aggregated groups.

5

A

Every relevant index may need maintenance during writes.

6

B

A completed month's data can be processed in a batch.

7

B

Linked leaf nodes commonly improve range scans.

8

C

Isolation governs concurrent transaction interference.

Section B model answers
Q9 (3 points)


sql
SELECT employee_name, salary
FROM Employees
WHERE salary > 50000
ORDER BY salary DESC;
1 point: correct columns and table.

1 point: correct filter.

1 point: descending salary order.

Q10 (4 points)


sql
SELECT d.department_name,
       COUNT(e.employee_id) AS employee_count,
       AVG(e.salary) AS average_salary
FROM Departments AS d
INNER JOIN Employees AS e
    ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name
HAVING COUNT(e.employee_id) >= 3
   AND AVG(e.salary) > 40000;
1 point: correct join.

1 point: correct grouping and aggregate expressions.

1 point: correct HAVING conditions.

1 point: valid query structure and output columns.

Q11 (3 points)


sql
SELECT d.department_name,
       COUNT(e.employee_id) AS employee_count
FROM Departments AS d
LEFT JOIN Employees AS e
    ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name;
Use COUNT(e.employee_id), not COUNT(*), because an unmatched department still contributes one joined row with NULL employee columns. COUNT(e.employee_id) returns zero for that department.

1 point: left join.

1 point: correct grouping.

1 point: correct counting behavior.

Q12 (3 points)

One valid solution:


sql
SELECT e.employee_id, e.employee_name
FROM Employees AS e
LEFT JOIN Assignments AS a
    ON e.employee_id = a.employee_id
WHERE a.employee_id IS NULL;
Alternative using NOT EXISTS:


sql
SELECT e.employee_id, e.employee_name
FROM Employees AS e
WHERE NOT EXISTS (
    SELECT 1
    FROM Assignments AS a
    WHERE a.employee_id = e.employee_id
);
Award full marks for either correct approach.

Q13 (4 points)


sql
SELECT p.project_name,
       SUM(a.hours_worked) AS total_hours
FROM Projects AS p
INNER JOIN Assignments AS a
    ON p.project_id = a.project_id
GROUP BY p.project_id, p.project_name
HAVING SUM(a.hours_worked) > 100;
1 point: correct joins.

1 point: correct grouping.

1 point: correct SUM.

1 point: correct aggregate filter.

Q14 (3 points)


sql
UPDATE Employees
SET salary = salary * 1.05
WHERE employee_id = 25;
Without the WHERE clause, every employee's salary could increase by 5%. A precaution is to run a SELECT using the same condition first, verify the target employee, and execute the change in a transaction where supported.

1 point: correct SQL.

1 point: consequence of missing WHERE.

1 point: sensible safety measure.

Section C model answers
Q15 (5 points)


text
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
The composite key (StudentID, CourseID) identifies each student-course registration, assuming each student can register for a given course only once. The registration date and final mark belong in Registration because they describe the association between a particular student and a particular course.

2 points: correct entity tables and primary keys.

1 point: correct junction table and foreign keys.

1 point: appropriate composite key.

1 point: correct placement and explanation of registration-specific attributes.

Q16 (4 points)

Partial dependency: StudentID → StudentName, because StudentID is only part of the composite key.

Transitive dependency: CourseID → InstructorID → InstructorName.

Suitable decomposition:


text
Student(StudentID PK, StudentName)

Instructor(InstructorID PK, InstructorName)

Course(CourseID PK, CourseTitle, InstructorID FK)

Enrollment(
    StudentID PK, FK,
    CourseID PK, FK,
    FinalMark
)
1 point: partial dependency.

1 point: transitive dependency.

2 points: appropriate decomposition preserving relationships and FinalMark.

Q17 (3 points)

A reasonable candidate is:


sql
CREATE INDEX idx_employee_department_salary
ON Employees (department_id, salary DESC);
The leading department_id supports filtering by department. The salary ordering within that department may support the descending sort. However, the DBMS might still need extra work because of its index implementation, query plan, or data-access requirements; returning employee_name may also require fetching table records unless the chosen index covers that column.

1 point: suitable index.

1 point: correct explanation of column order.

1 point: valid limitation.

Section D model answers
Q18 (4 points)

The cache contains an older value because a successful database update was not propagated or invalidated.

Invalidate or update the relevant cache entry after the database transaction commits; use suitable ordering, retry and recovery mechanisms.

A stale cache can expose outdated stock quantities and contribute to overselling if inventory validation relies on stale data.

Test a purchase followed by a stock read, including repeated requests, cache misses, failures during invalidation and concurrent purchases.

Award 1 point per component.

Q19 (4 points)

Two possible improvements:

Replication/failover: Maintain a replica that can take over if the primary fails. Limitation: replication lag or failover errors can affect recovery, and configuration matters.

Backups and recovery testing: Restore data after corruption or a serious failure. Limitation: restoring backups does not necessarily provide immediate service availability.

Other valid answers include redundant infrastructure, health monitoring, automated failover and disaster recovery planning.

Award 2 points for each well-explained improvement, including a relevant limitation.

Q20 (4 points)

A page split divides a full node into multiple nodes so another entry can be inserted.

The parent must be updated to represent the new key ranges; splits may propagate upward and create a new root.

A balanced tree keeps search paths relatively short, typically providing logarithmic search complexity and limiting page reads.

The index organizes its own keys and references. Unless the storage design specifically clusters rows by that index, the physical table records need not follow the index order.

Award 1 point per component.

Data Management 2026–2027
Mock Exam 3 — Comprehensive Challenge
Mock Exam 3
Challenge
Duration

90 minutes
Total marks

70 points
Focus: integrated design, multi-step SQL, anomaly analysis, transaction correctness, and performance trade-offs.

Instructions

Read the assumptions and schema carefully.

Show intermediate reasoning for multi-step questions.

SQL answers should be logically correct; equivalent solutions receive credit.

Explain why a proposed solution works and identify its limitations.

Section A — Conceptual challenge (10 points)
5 questions × 2 points.

Q1. A service has low average response time but occasionally takes 20 seconds to respond. Explain why its average latency alone may not adequately describe user experience. Identify one additional metric or distribution statistic that would help.

Q2. A designer proposes putting a customer's department name directly into every employee row to avoid joins. Explain the trade-off and identify the normalization problem if department names depend on department IDs.

Q3. A database transaction successfully commits. Shortly afterward, the server loses power. Which ACID property concerns whether the committed changes survive, and how does this differ from atomicity?

Q4. A query filters employees by department, orders them by salary, and returns their names. Explain why an index can improve this query without guaranteeing that every operation becomes faster.

Q5. Explain why conceptual modeling should normally precede physical database design. Give one example of a costly mistake that could arise from choosing physical structures before understanding business requirements.

Section B — Integrated SQL case study (24 points)
A university uses the following schema:


sql
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
Assume that each student can register for a course at most once and that mark can be NULL until grading is complete.

Q6. Student performance (3 points)

Write a query that returns the names of students who have at least one mark greater than or equal to 80. Each student should appear only once.

Q7. Course averages (4 points)

Return each course ID, course name, number of enrolment records, and average non-NULL mark. Only include courses with at least five non-NULL marks and an average mark of at least 70.

Q8. Students with no registrations (3 points)

Write a query that finds students who have never registered for any course.

Q9. Department-level report (5 points)

Write a query returning each department and its average mark across all enrolments in that department. Include only departments with an average mark greater than 65. Courses with no non-NULL marks should not qualify.

Q10. Interpret a SQL query (4 points)

Consider:


sql
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
Explain:

What rows are removed by WHERE.

What is counted by COUNT(*).

What the HAVING condition means.

Why this result does not necessarily represent the average mark across all enrolments.

Q11. Repair a query (3 points)

A student wants to list departments with an average mark of at least 60, but writes:


sql
SELECT c.department, AVG(e.mark)
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
WHERE AVG(e.mark) >= 60
GROUP BY c.department;
Explain the error and rewrite the query correctly.

Q12. Update safely (2 points)

An administrator wants to set the mark to 75 for student 14 in course 22. Write the update statement using both identifying columns.

Section C — Database design and normalization (12 points)
Q13. Normalize a sales record (6 points)

A company initially stores sales in this table:


text
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
Assume:

Each order belongs to one customer.

An order can contain multiple products.

Each product has one current product name.

UnitPrice records the price agreed for that order line.

An order contains at most one line for each product.

Dependencies include:


text
OrderID -> OrderDate, CustomerID
CustomerID -> CustomerName
ProductID -> ProductName
(OrderID, ProductID) -> UnitPrice, Quantity
Propose a normalized schema. Identify one partial dependency and explain why the order-line price should not automatically be replaced by the current product price.

Q14. Relationship design under changing requirements (3 points)

The university later allows students to retake a course, with a separate registration date and mark for each attempt.

Explain why (StudentID, CourseID) is no longer sufficient as the primary key and propose a better design.

Q15. Choose a schema for analytics (3 points)

A retail company must produce reports by day, product, store and region. Compare a star schema and a snowflake schema for this workload. Recommend one, justifying the recommendation with at least two considerations.

Section D — Performance and systems reasoning (12 points)
Q16. Composite index selection (4 points)

A large employee table is queried frequently with:


sql
SELECT employee_id, employee_name, salary
FROM Employees
WHERE department_id = 7
  AND salary >= 60000
ORDER BY salary DESC;
Recommend an index and explain:

Why the column order matters.

How the index may help filtering and sorting.

Why it may not eliminate every table read.

Q17. B-tree insertion and deletion (4 points)

Explain how a B-tree maintains its balance when:

A full leaf page receives a new key.

A deletion causes a node to become underfull.

Then explain why these operations may require changes to parent pages.

Q18. Diagnose a performance problem (4 points)

A database table has 50 million rows. A query filters on a frequently used column but remains slow. The administrator creates indexes on 15 additional columns, after which inserts become slower.

Explain:

Why the original query may have been slow even with an index.

Why adding many indexes can slow inserts.

Two steps you would take to investigate or improve performance.

Section E — Transactions and system reliability (12 points)
Q19. Transaction failure analysis (4 points)

An application transfers €500 from Account A to Account B:

Deduct €500 from Account A.

Add €500 to Account B.

The server fails between these operations.

Explain the potential inconsistency and how a transaction with appropriate ACID guarantees addresses it. Identify which ACID properties are most directly relevant.

Q20. Design for fault tolerance (4 points)

An online booking system must continue serving users during a single server failure and must recover confirmed bookings after a crash.

Propose an architecture and operational approach that addresses both availability and data durability. Explain at least one trade-off or limitation.

Q21. Batch versus stream integration (4 points)

A delivery company has two requirements:

Detect a potentially fraudulent payment within seconds.

Produce a reconciled monthly revenue report.

Recommend a processing approach for each. Explain how the systems might work together and why a single processing approach is not necessarily ideal for both.

Mock Exam 3 — Answer key and marking guide
Section A model answers
Q1 (2 points)

An average can conceal a long tail of slow requests. For example, most requests might complete quickly while a small percentage take 20 seconds. A useful additional measure is the 95th or 99th percentile latency, or a latency distribution.

1 point: identifies the limitation of the average.

1 point: identifies a suitable metric.

Q2 (2 points)

Duplicating department names may reduce some joins, but it creates redundancy. If a department name changes, many employee records must be updated. When EmployeeID → DepartmentID and DepartmentID → DepartmentName, storing the department name in the employee table introduces a transitive dependency.

1 point: trade-off or update anomaly.

1 point: dependency explanation.

Q3 (2 points)

Durability concerns whether committed changes survive a crash. Atomicity means a transaction's operations succeed together or are rolled back together. Atomicity does not, by itself, guarantee that a successful commit survives a later failure.

- 1 point per distinction.

Q4 (2 points)

An appropriate index may reduce scanning and help retrieve qualifying rows in salary order. However, indexes consume storage, require maintenance, and may still require table lookups for employee_name or other columns. The optimizer may also choose another access plan depending on data distribution and selectivity.

1 point: benefit.

1 point: limitation.

Q5 (2 points)

Conceptual modeling clarifies business entities, relationships and requirements before implementation decisions are made. For example, designing a database around an incorrect assumption that each order has only one product could require expensive schema changes when multi-product orders are later needed.

1 point: reason for the sequence.

1 point: valid example.

Section B model answers
Q6 (3 points)


sql
SELECT DISTINCT s.student_name
FROM Students AS s
JOIN Enrolments AS e
    ON s.student_id = e.student_id
WHERE e.mark >= 80;
DISTINCT ensures each student appears only once.

1 point: correct join.

1 point: correct filter.

1 point: prevents duplicate students.

Q7 (4 points)


sql
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
COUNT(e.mark) ignores NULL marks, so the threshold applies to graded enrolments. AVG(e.mark) also ignores NULL marks.

1 point: join and output columns.

1 point: grouping.

1 point: correct count of non-NULL marks.

1 point: both HAVING conditions.

Q8 (3 points)


sql
SELECT s.student_id, s.student_name
FROM Students AS s
WHERE NOT EXISTS (
    SELECT 1
    FROM Enrolments AS e
    WHERE e.student_id = s.student_id
);
A valid LEFT JOIN ... WHERE e.student_id IS NULL solution also receives full credit.

1 point: correct relationship check.

1 point: correctly identifies missing registrations.

1 point: valid query structure.

Q9 (5 points)


sql
SELECT c.department,
       AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
GROUP BY c.department
HAVING AVG(e.mark) > 65;
AVG ignores NULL marks. Departments whose enrolments have no non-NULL marks produce a NULL average and do not satisfy AVG(e.mark) > 65.

1 point: join.

1 point: grouping by department.

1 point: correct aggregate.

1 point: correct threshold.

1 point: correctly handles departments with no graded marks.

Q10 (4 points)

WHERE e.mark >= 50 removes enrolment rows with marks below 50 and rows whose mark is NULL.

COUNT(*) counts the remaining joined enrolment rows in each department group.

HAVING COUNT(*) >= 10 keeps departments with at least ten qualifying rows.

The average is calculated only over rows that passed the WHERE filter. It is therefore not the average of all enrolments, but the average of qualifying marks.

Award 1 point per explanation.

Q11 (3 points)

The error is that an aggregate condition belongs in HAVING, not WHERE, because grouping and aggregation occur after row filtering.

Correct query:


sql
SELECT c.department, AVG(e.mark) AS average_mark
FROM Courses AS c
JOIN Enrolments AS e
    ON c.course_id = e.course_id
GROUP BY c.department
HAVING AVG(e.mark) >= 60;
1 point: explains the error.

1 point: correct use of GROUP BY.

1 point: correct HAVING condition.

Q12 (2 points)


sql
UPDATE Enrolments
SET mark = 75
WHERE student_id = 14
  AND course_id = 22;
Both conditions are needed to identify the intended composite-key row.

1 point: correct SET.

1 point: both key columns in the filter.

Section C model answers
Q13 (6 points)

One suitable design is:


text
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
Partial dependency: OrderID → OrderDate, CustomerID. These attributes depend on only part of the composite key (OrderID, ProductID).

Transitive dependency: CustomerID → CustomerName, reached through the order's customer reference. Product names similarly depend on ProductID.

UnitPrice should remain on OrderLine because it represents the historical agreed price for that transaction. The current product price may change after the order was placed.

Marking:

3 points: appropriate schema and keys.

1 point: partial dependency.

1 point: customer/product redundancy addressed.

1 point: historical order price explained.

Q14 (3 points)

A student can now have multiple registrations for the same course. Therefore, (StudentID, CourseID) would no longer uniquely identify each attempt.

A suitable design is:


text
Registration(
    RegistrationID PK,
    StudentID FK,
    CourseID FK,
    RegistrationDate,
    FinalMark
)
Alternatively, a composite key such as `(StudentID, CourseID, AttemptNumber
