# Stage 1 — Mock Exam 1 of 3

## Data Management 2026–2027 · T0–T2

This first mock exam begins the three-exam strategy. It tests the combined content of T0 (Data Management foundations, if included in your materials), T1 (Data Models & Data Storage), T2 (SQL), and the T3 notes you supplied (NoSQL Databases & Data Modeling).

Mock Exam 1 focuses on broad coverage and core understanding. The questions mix definitions, comparisons, SQL interpretation, database design, MongoDB operations, and index behavior. The next exams can increase the difficulty and emphasize application, tricky distinctions, and integrated problems.

## Exam conditions

Questions

# 40

Suggested time

# 50 min

Format

### Multiple choice

Scoring

### 40 points

- Choose one best answer per question.
- Each question is worth one point.
- No notes or answer key while attempting.
- The answer key is intentionally withheld until you submit your answers, so you can use this as a closed-book mock.

## Part A — Data modeling and relational foundations

## Your answer sheet

0/40 answered

Part A Data models & foundations

Part B SQL & indexes

Part C NoSQL & MongoDB

QUESTION 1 · Data modeling

Which database modeling level focuses on business entities and their relationships without implementation details?

A. Physical

B. Logical

C. Conceptual

D. Storage

QUESTION 2 · Data modeling

Which statement correctly distinguishes a logical model from a physical model?

A. Logical models specify disk pages; physical models define business needs.

B. Logical models define attributes and keys; physical models specify implementation details such as data types and indexes.

C. Logical models only apply to NoSQL databases.

D. There is no meaningful distinction.

QUESTION 3 · Keys

What is the main purpose of a primary key?

A. To sort every table automatically

B. To allow duplicate records

C. To uniquely identify each row and disallow NULL values

D. To connect tables without referencing any key

QUESTION 4 · Relationships

A customer may place many orders, but each order belongs to one customer. Where should the foreign key normally be placed?

A. In the Customer table only

B. In the Orders table

C. In a separate junction table in every case

D. In neither table

QUESTION 5 · Relationships

Students can take many courses, and each course can have many students. What is the usual relational implementation?

A. Store all course IDs in one text field in Student

B. Remove the relationship

C. Create a junction table such as Enrollment

D. Put StudentID as the sole foreign key in Course

QUESTION 6 · Normalization

A table stores three phone numbers in one cell. Which normal form requirement is directly violated?

A. 1NF

B. 2NF

C. 3NF

D. ACID

QUESTION 7 · Normalization

A table has composite key (StudentID, CourseID). StudentName depends only on StudentID. Which issue does this illustrate?

A. A transitive dependency only

B. A partial dependency relevant to 2NF

C. A violation of durability

D. A many-to-many relationship that cannot be represented

QUESTION 8 · Schemas

Which schema typically has a central fact table connected directly to dimension tables?

A. Hierarchical

B. Flat

C. Star

D. Network

QUESTION 9 · Data foundations

Which metric measures the number of operations processed per unit of time?

A. Latency

B. Throughput

C. Response time

D. Redundancy

QUESTION 10 · Relational model

In relational terminology, what is a tuple?

A. A row

B. A column

C. A schema

D. An index

QUESTION 11 · Database evolution

Which statement best distinguishes navigational databases from relational databases?

A. Navigational databases use SQL exclusively.

B. Navigational databases rely on pointers and access paths, while relational databases organize data into tables and support flexible queries.

C. Relational databases cannot represent relationships.

D. Navigational databases always use document storage.

QUESTION 12 · ACID

A bank transfer must debit one account and credit another, or do neither. Which property is most directly responsible?

A. Isolation

B. Durability

C. Atomicity

D. Availability

QUESTION 13 · ACID

Which ACID property ensures that committed changes survive a restart or failure?

A. Consistency

B. Isolation

C. Atomicity

D. Durability

QUESTION 14 · SQL fundamentals

Which statement correctly distinguishes a DBMS from a database?

A. A DBMS is the organized data; a database is the software.

B. A DBMS manages stored data, queries, access, and transactions; the database is the organized data itself.

C. SQL and DBMS mean the same thing.

D. A database is always a single CSV file.

Previous part  Next part

 Submit and mark exam

## Coverage checklist

- Data modeling: conceptual, logical and physical models; entities, keys, cardinalities, normalization, star schemas.
- Relational and SQL: database vs. DBMS, command categories, CRUD, filtering, grouping, joins, data types and ACID.
- Indexes: trade-offs, composite indexes, B-tree search, insertion, page splits and B+ tree range scans.
- NoSQL: SQL vs. NoSQL, scaling, ACID/BASE/CAP, four NoSQL models, MongoDB CRUD and operators, aggregation, wide-column and graph databases.

Strategy: Take Mock Exam 1 without notes, review the explanations after submission, and use your mistakes to guide Mock Exam 2. Mock Exam 3 should then serve as a final mixed, exam-style assessment.
