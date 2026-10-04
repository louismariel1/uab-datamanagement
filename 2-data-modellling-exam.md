Below is a **comprehensive non-MCQ exam with model answers** covering the major concepts from the lecture notes on **Data Modeling and Database Design**, including data models, abstraction levels, entities/attributes/relationships, keys, normalization, relational databases, and database-modeling schemas.

 # Data Modeling and Database Design

 ## Comprehensive Non-MCQ Examination with Model Answers

 ### Instructions

 - Answer all questions.
- Give clear definitions and explain concepts using examples where appropriate.
- For design questions, justify your choices.
- The exam covers conceptual understanding, application, comparison, and database-design problems.

---

 # Part A — Short-Answer Questions

 ### Question 1

 What is data modeling, and why is it important in database design?

 ### Model Answer

 Data modeling is the process of identifying, organizing, and representing the data that an information system needs to store and the relationships among that data.

 It is important because it:

 - Provides a clear structure for organizing data.
- Helps identify entities, attributes, and relationships.
- Reduces data redundancy and inconsistency.
- Improves data integrity.
- Provides a blueprint for designing the database.
- Helps developers and users understand the data requirements.
- Supports maintainability and scalability.

---

 ### Question 2

 What are the three main levels of data abstraction in database design?

 ### Model Answer

 The three levels are:

 1. **Conceptual level** — describes the major entities, attributes, and relationships from a high-level business perspective.
2. **Logical level** — converts the conceptual design into a logical database structure such as tables, attributes, primary keys, and foreign keys.
3. **Physical level** — describes how the data is actually stored, including storage structures, indexes, files, and other implementation details.

---

 ### Question 3

 Define an entity, attribute, and relationship.

 ### Model Answer

 - **Entity:** A distinguishable object or concept about which data is stored. Example: Student, Course, or Employee.
- **Attribute:** A property that describes an entity. Example: StudentID, Name, and Email are attributes of Student.
- **Relationship:** An association between two or more entities. Example: A Student enrolls in a Course.

---

 ### Question 4

 What is a primary key?

 ### Model Answer

 A primary key is an attribute, or combination of attributes, that uniquely identifies each record in a table.

 A primary key must be unique and should not contain NULL values.

 Example:

 `Student(StudentID, Name, Email)`

 Here, `StudentID` can serve as the primary key.

---

 ### Question 5

 What is a foreign key?

 ### Model Answer

 A foreign key is an attribute in one table that references the primary key of another table.

 It is used to establish relationships between tables and maintain referential integrity.

 Example:

 `Student(StudentID, Name)`

 `Enrollment(StudentID, CourseID)`

 Here, `Enrollment.StudentID` can reference `Student.StudentID`.

---

 ### Question 6

 What is normalization?

 ### Model Answer

 Normalization is the process of organizing data into well-structured tables to reduce unnecessary data redundancy and prevent insertion, update, and deletion anomalies.

 It improves data consistency and integrity by ensuring that each piece of information is stored in an appropriate location.

---

 ### Question 7

 What is data redundancy?

 ### Model Answer

 Data redundancy occurs when the same piece of information is unnecessarily stored in multiple locations.

 For example, if a customer's address is stored repeatedly in every order record, the address is redundant.

 Redundancy can cause:

 - Increased storage requirements.
- Inconsistent data.
- Update anomalies.
- More difficult maintenance.

---

 ### Question 8

 What is a database schema?

 ### Model Answer

 A database schema is the logical structure or blueprint of a database. It describes tables, attributes, relationships, constraints, and other structural elements.

 The schema defines how the data is organized rather than the actual data values stored in the database.

---

 ### Question 9

 What is a flat-file database?

 ### Model Answer

 A flat-file database is a database in which data is stored in a single file, usually in a simple tabular format.

 Examples include CSV, XLS, and TXT files.

 The data is generally organized into rows and columns, but there are no sophisticated structures for representing relationships between records.

---

 ### Question 10

 What is the hierarchical data model?

 ### Model Answer

 The hierarchical data model organizes data in a tree-like structure.

 Each record normally has one parent, while a parent can have multiple children. Therefore, it naturally represents one-to-many relationships.

 It is particularly suitable for data with clear parent-child relationships and nested structures.

---

 # Part B — Conceptual and Explanation Questions

 ### Question 11

 Explain the main characteristics of the flat data model.

 ### Model Answer

 The flat data model is one of the simplest database models.

 Its main characteristics include:

 - Data is stored in a file.
- Data may be stored in plain-text or spreadsheet-like formats such as CSV, XLS, or TXT.
- Data is usually represented as one table.
- Each row represents a record.
- Each column represents an attribute.
- Records generally follow a uniform format.
- There are no sophisticated structures for identifying relationships between records.
- It is easy to understand and use for simple datasets.

 Its main weakness is that it becomes difficult to manage complex relationships and large amounts of data.

---

 ### Question 12

 Explain the hierarchical data model and give an example.

 ### Model Answer

 The hierarchical model represents data as a tree.

 There is a root at the top, followed by parent and child records.

 For example:

```
University
│
├── Faculty of Computing
│   ├── Student A
│   ├── Student B
│   └── Student C
│
└── Faculty of Business
    ├── Student D
    └── Student E
```

 The University is the root, faculties are children of the university, and students are children of faculties.

 The model is efficient when data naturally follows a parent-child structure.

---

 ### Question 13

 Explain the relational data model.

 ### Model Answer

 The relational model organizes data into relations, commonly called tables.

 Each table contains:

 - **Rows**, representing records or tuples.
- **Columns**, representing attributes.

 Tables can be connected using primary keys and foreign keys.

 For example:

```
STUDENT
-------------------------
StudentID | Name | Email
1         | Ali  | ...
2         | Sara | ...

COURSE
-------------------------
CourseID | CourseName
101      | Database
102      | Networking

ENROLLMENT
-------------------------
StudentID | CourseID
1         | 101
1         | 102
2         | 101
```

 The relational model provides a standard way of representing and querying structured data and is commonly implemented using SQL database systems.

---

 ### Question 14

 Why are primary keys and foreign keys important in relational databases?

 ### Model Answer

 Primary keys uniquely identify records in tables.

 Foreign keys establish relationships between tables by referencing primary keys in other tables.

 Together they help:

 - Identify records uniquely.
- Establish relationships.
- Maintain referential integrity.
- Prevent invalid references.
- Organize related information across multiple tables.

---

 ### Question 15

 Explain referential integrity.

 ### Model Answer

 Referential integrity ensures that relationships between related tables remain valid.

 For example, if an `Enrollment` table contains:

 `StudentID = 25`

 then Student 25 should exist in the Student table.

 A foreign key constraint can prevent an enrollment from referencing a student who does not exist.

---

 ### Question 16

 What is the difference between a conceptual model and a logical model?

 ### Model Answer

 A **conceptual model** focuses on the business view of the data. It identifies entities, attributes, and relationships without worrying about implementation details.

 A **logical model** translates this understanding into a structured database design. It specifies tables, attributes, keys, relationships, and constraints.

 In simple terms:

 **Conceptual = What data and relationships exist?**

 **Logical = How will the data be logically organized?**

---

 ### Question 17

 What is the purpose of the physical data model?

 ### Model Answer

 The physical data model describes how the database will actually be implemented and stored.

 It can include:

 - Storage structures.
- Indexes.
- Data types.
- File organization.
- Partitioning.
- Physical storage considerations.
- Performance-related structures.

 The physical model is therefore concerned with implementation rather than simply describing the business data.

---

 ### Question 18

 Explain why normalization is important in database design.

 ### Model Answer

 Normalization helps organize data efficiently and reduce redundancy.

 It helps prevent:

 - **Update anomalies:** The same information must be changed in multiple places.
- **Insertion anomalies:** New information cannot be inserted without unrelated information.
- **Deletion anomalies:** Deleting one piece of information unintentionally removes another important piece of information.

 Normalization also improves consistency and data integrity.

---

 # Part C — Relational Model Benefits and Limitations

 ### Question 19

 Describe four benefits of the relational database model.

 ### Model Answer

 Four important benefits are:

 1. **Simplicity**\
    Data is organized into tables, making the model relatively easy to understand.
2. **User-friendliness**\
    SQL provides a relatively straightforward way to retrieve and manipulate data.
3. **Data accuracy and consistency**\
    Keys, constraints, and well-defined structures help maintain reliable data.
4. **Data integrity**\
    Relationships and constraints help ensure that data remains valid and consistent.

 Additional benefits include reduced redundancy, standardized querying, and strong transaction support.

---

 ### Question 20

 What is SQL, and what role does it play in relational databases?

 ### Model Answer

 SQL stands for **Structured Query Language**.

 It is used to interact with relational databases.

 SQL can be used to:

 - Create database structures.
- Insert data.
- Retrieve data.
- Update data.
- Delete data.
- Define constraints.
- Manage database objects.

 For example:

```
SELECT Name
FROM Student
WHERE StudentID = 10;
```

 This retrieves the name of a student with a specific ID.

---

 ### Question 21

 Explain the limitations of relational databases discussed in the lecture.

 ### Model Answer

 Important limitations include:

 - **Maintenance challenges:** Large relational databases can become increasingly difficult to manage as data volume grows.
- **Cost implications:** Some relational database systems can require significant software, infrastructure, and specialized personnel.
- **Limited scalability:** Traditional relational systems may rely heavily on vertical scaling, which can become expensive.
- **Structural complexity:** Tables may be less convenient for representing highly complex or deeply nested objects.
- **Performance degradation:** Queries involving many tables and complex joins can become slower as data and workload increase.

 These limitations do not mean relational databases are unsuitable; rather, they show why other models may be preferred for certain applications.

---

 ### Question 22

 What is meant by scalability in database systems?

 ### Model Answer

 Scalability is the ability of a database system to continue handling increasing amounts of data, users, transactions, or workload while maintaining acceptable performance.

 A system with poor scalability may experience slower response times as workload increases.

---

 ### Question 23

 Distinguish between vertical and horizontal scalability.

 ### Model Answer

 **Vertical scalability** means increasing the resources of a single machine, such as:

 - CPU.
- RAM.
- Storage capacity.

 **Horizontal scalability** means adding more machines or database nodes to distribute the workload.

 Relational databases have traditionally relied heavily on vertical scaling, although modern relational systems can also support various forms of horizontal scaling.

---

 # Part D — ACID Transactions

 ### Question 24

 What does ACID stand for?

 ### Model Answer

 ACID stands for:

 - **Atomicity**
- **Consistency**
- **Isolation**
- **Durability**

 These properties help ensure reliable and correct transaction processing in database systems.

---

 ### Question 25

 Explain Atomicity with an example.

 ### Model Answer

 Atomicity means that a transaction is treated as an all-or-nothing operation.

 For example, suppose a bank transfer moves €100 from Account A to Account B.

 Two operations must occur:

 1. Subtract €100 from A.
2. Add €100 to B.

 If the first operation succeeds but the second fails, the database should roll back the transaction rather than leaving the system in an inconsistent state.

 Therefore, either both operations occur or neither occurs.

---

 ### Question 26

 Explain Consistency in the ACID model.

 ### Model Answer

 Consistency means that a transaction takes the database from one valid state to another valid state.

 Database rules and constraints must remain satisfied after the transaction.

 For example, a foreign key constraint should not be violated after a transaction completes.

---

 ### Question 27

 Explain Isolation in the ACID model.

 ### Model Answer

 Isolation means that concurrent transactions should not improperly interfere with one another.

 Each transaction should behave as though it is executing in an appropriately controlled environment, even when multiple transactions execute at the same time.

 This prevents problems caused by uncontrolled concurrent access.

---

 ### Question 28

 Explain Durability in the ACID model.

 ### Model Answer

 Durability means that once a transaction has been successfully committed, its changes are permanent.

 If the system subsequently crashes, committed data should not simply disappear.

---

 # Part E — Applied Database Design Questions

 ### Question 29

 A university wants to store information about students and courses. Each student can enroll in multiple courses, and each course can contain multiple students.

 Identify the entities, important attributes, and relationship.

 ### Model Answer

 **Entities:**

 - Student
- Course
- Enrollment

 **Possible Student attributes:**

 - StudentID
- Name
- Email
- DateOfBirth

 **Possible Course attributes:**

 - CourseID
- CourseName
- Credits

 Because students can enroll in many courses and courses can have many students, the relationship is **many-to-many**.

 A separate Enrollment entity/table can resolve this relationship.

```
STUDENT
StudentID (PK)
Name
Email

COURSE
CourseID (PK)
CourseName
Credits

ENROLLMENT
StudentID (FK)
CourseID (FK)
```

 The combination of `StudentID` and `CourseID` can be used as a composite primary key for Enrollment.

---

 ### Question 30

 A company stores the following information in one table:

```
EmployeeID
EmployeeName
DepartmentID
DepartmentName
DepartmentLocation
```

 Explain the potential problem with this design.

 ### Model Answer

 The design contains information about both employees and departments in the same table.

 If many employees work in the same department, the department name and location will be repeated for every employee.

 This creates redundancy.

 For example:

```
1 | Ali  | D01 | IT | Building A
2 | Sara | D01 | IT | Building A
3 | John | D01 | IT | Building A
```

 If the department location changes, multiple records may need to be updated.

 This can create update anomalies and inconsistent data.

 A better design would separate the entities:

```
EMPLOYEE
EmployeeID
EmployeeName
DepartmentID

DEPARTMENT
DepartmentID
DepartmentName
DepartmentLocation
```

---

 ### Question 31

 Why would the design in Question 30 benefit from normalization?

 ### Model Answer

 Normalization separates information into logically appropriate tables.

 The Department information only needs to be stored once.

 The Employee table can reference the Department table using `DepartmentID`.

 Benefits include:

 - Reduced redundancy.
- Easier updates.
- Better consistency.
- Improved data integrity.
- Reduced risk of update anomalies.

---

 ### Question 32

 Design a relational database for a library.

 The library needs to store:

 - Books.
- Authors.
- Members.
- Borrowing transactions.

 A book can have multiple authors, and an author can write multiple books.

 ### Model Answer

 A suitable design is:

```
BOOK
----------------------
BookID (PK)
Title
ISBN
PublicationYear

AUTHOR
----------------------
AuthorID (PK)
AuthorName

BOOK_AUTHOR
----------------------
BookID (FK)
AuthorID (FK)

MEMBER
----------------------
MemberID (PK)
MemberName
Email

BORROWING
----------------------
BorrowingID (PK)
BookID (FK)
MemberID (FK)
BorrowDate
ReturnDate
```

 The `BOOK_AUTHOR` table resolves the many-to-many relationship between books and authors.

 The `BORROWING` table records which member borrowed which book.

---

 ### Question 33

 A company has the following requirements:

 - A department can have many employees.
- Each employee belongs to one department.
- Each employee has a unique employee number.

 Identify the cardinality and propose a relational design.

 ### Model Answer

 The relationship between Department and Employee is **one-to-many**:

```
One Department → Many Employees
```

 A suitable design is:

```
DEPARTMENT
DepartmentID (PK)
DepartmentName

EMPLOYEE
EmployeeID (PK)
EmployeeName
DepartmentID (FK)
```

 `DepartmentID` in Employee references `DepartmentID` in Department.

---

 # Part F — Comparison Questions

 ### Question 34

 Compare flat-file, hierarchical, and relational data models.

 ### Model Answer

 | Feature | Flat-File | Hierarchical | Relational |
| --- | --- | --- | --- |
| Structure | Single file/table | Tree | Tables |
| Relationships | Very limited | Parent-child | Keys and relationships |
| Complexity | Low | Moderate | Moderate |
| Many-to-many relationships | Poor support | Poor/natural limitation | Strong support |
| Querying | Basic | Structure-dependent | SQL |
| Best suited for | Simple datasets | Nested/parent-child data | Structured business data |

The relational model is generally more flexible than the flat-file and hierarchical models for complex relationships.

---

 ### Question 35

 Compare hierarchical and relational database models.

 ### Model Answer

 The hierarchical model organizes information as a tree, where records have parent-child relationships.

 The relational model organizes information into tables and uses keys to represent relationships.

 The hierarchical model is efficient when relationships are naturally one-to-many and predictable.

 The relational model is more flexible because it can represent one-to-one, one-to-many, and many-to-many relationships.

---

 ### Question 36

 Why might an organization choose a relational database instead of a flat-file database?

 ### Model Answer

 An organization may choose a relational database because it provides:

 - Better organization of large datasets.
- Relationships between tables.
- Primary and foreign keys.
- Data integrity constraints.
- Reduced redundancy through normalization.
- Powerful querying through SQL.
- Transaction management.
- Better support for concurrent users.
- More reliable data management.

 A flat file may be adequate for small, simple datasets but becomes difficult to manage as complexity increases.

---

 # Part G — Scenario-Based Questions

 ### Question 37

 A hospital stores patient information in multiple spreadsheets. The same patient may appear in several, the appropriate choice ultimately depends on the application's requirements. A document-oriented spreadsheets, and different versions of the patient's phone number exist.

 What database problems are present?

 ### Model Answer

 The hospital is experiencing:

 - Data redundancy.
- Data inconsistency.
- Lack of centralized data management.
- Possible update anomalies.
- Potential integrity problems.

 A properly designed database could centralize patient information and use a unique PatientID to identify each patient.

---

 ### Question 38

 A company has millions of records and thousands of users accessing its database simultaneously. Queries that once took one second now take ten seconds.

 What database issue is being illustrated?

 ### Model Answer

 This illustrates a **performance and scalability problem**.

 As data volume and user load increase, queries may become slower.

 Possible causes include:

 - Large datasets.
- Complex joins.
- Missing or inefficient indexes.
- Increased concurrent transactions.
- Poor database design.
- Insufficient hardware resources.

 Database administrators may need to optimize queries, indexes, database structures, and infrastructure.

---

 ### Question 39

 A company needs to store highly nested product information where products can contain categories, subcategories, specifications, and nested components.

 Which database model could naturally represent this type of structure, and why?

 ### Model Answer

 A hierarchical model can naturally represent nested data because it uses a tree-like parent-child structure.

 However, the appropriate choice ultimately depends on the application's requirements. A document-oriented NoSQL database may also be suitable when the nested structure is complex and flexible.

---

 ### Question 40

 A banking application transfers money between two accounts. Explain why ACID properties are important.

 ### Model Answer

 ACID properties are critical because financial transactions must be reliable.

 - **Atomicity:** Both sides of the transfer must succeed or neither should occur.
- **Consistency:** Database rules must remain valid.
- **Isolation:** Simultaneous transactions should not produce incorrect account balances.
- **Durability:** Once the transfer is committed, the result must survive system failures.

 Without these properties, money could potentially disappear, be duplicated, or produce incorrect balances.

---

 # Part H — Database Design Problems

 ### Question 41

 Consider the following unnormalized table:

```
ORDER_ID | CUSTOMER_NAME | CUSTOMER_PHONE | PRODUCT1 | PRODUCT2 | PRODUCT3
```

 Identify at least three problems with this design.

 ### Model Answer

 Problems include:

 1. **Repeating groups** — Product1, Product2, and Product3 represent repeated attributes.
2. **Limited scalability** — The table only provides space for three products.
3. **Data redundancy** — Customer information may be repeated across orders.
4. **Update anomalies** — Changing customer information may require multiple updates.
5. **Poor flexibility** — Adding more products requires changing the table structure.
6. **Difficult querying** — Searching for products across multiple product columns becomes awkward.

 A normalized design would separate customers, orders, products, and order items.

---

 ### Question 42

 Redesign the table from Question 41 using a relational approach.

 ### Model Answer

 A better design could be:

```
CUSTOMER
----------------------
CustomerID (PK)
CustomerName
CustomerPhone

ORDER
----------------------
OrderID (PK)
CustomerID (FK)
OrderDate

PRODUCT
----------------------
ProductID (PK)
ProductName
Price

ORDER_ITEM
----------------------
OrderID (FK)
ProductID (FK)
Quantity
```

 This design allows an order to contain any number of products without adding new columns.

---

 ### Question 43

 Explain why the `ORDER_ITEM` table is necessary.

 ### Model Answer

 An order can contain many products, and the same product can appear in many different orders.

 Therefore, Order and Product have a many-to-many relationship.

 The `ORDER_ITEM` table resolves this relationship.

 It can also store relationship-specific attributes such as:

 - Quantity.
- Unit price at the time of purchase.
- Discount.

---

 # Part I — Higher-Level Discussion Questions

 ### Question 44

 "Good data modeling is more important than simply storing data." Discuss this statement.

 ### Model Answer

 Simply storing data does not guarantee that the data will be useful, accurate, or maintainable.

 Good data modeling determines:

 - What information should be stored.
- How information is related.
- How redundancy is controlled.
- How data integrity is maintained.
- How users will retrieve the information.
- How the system can evolve over time.

 Poor modeling can result in duplication, inconsistent data, difficult queries, poor performance, and maintenance problems.

 Therefore, data modeling provides the foundation for a reliable database system.

---

 ### Question 45

 Why does the choice of data model affect system performance and scalability?

 ### Model Answer

 Different data models organize and access data differently.

 For example:

 - A hierarchical model can be efficient for predictable parent-child access.
- A relational model provides powerful querying and strong integrity mechanisms.
- Other models may be more suitable for highly flexible, distributed, or nested data.

 The chosen model affects:

 - How data is stored.
- How relationships are represented.
- How queries are executed.
- How easily the system can scale.
- How complex data access becomes.

 Therefore, the database model should match the application's requirements.

---

 ### Question 46

 Explain the statement: "Different schemas suit different business needs."

 ### Model Answer

 There is no single database structure that is ideal for every application.

 For example:

 - A simple flat file may be sufficient for a small personal dataset.
- A hierarchical structure may work well for clearly nested information.
- A relational database is appropriate for structured business data with complex relationships and strong consistency requirements.
- Other database models may be better for highly distributed or flexible data.

 The appropriate choice depends on factors such as:

 - Data structure.
- Relationships.
- Query requirements.
- Scalability.
- Performance.
- Consistency requirements.
- Cost.
- Maintenance requirements.

---

 # Part J — Extended Essay Questions

 ### Question 47

 Explain the complete database design process from requirements analysis to implementation.

 ### Model Answer

 A typical database design process can be described as follows:

 **1\. Requirements analysis**

 Determine what information the organization needs and how users will use the data.

 **2\. Conceptual modeling**

 Identify major entities, attributes, and relationships.

 For example:

```
Student ─── enrolls in ─── Course
```

 **3\. Logical modeling**

 Convert the conceptual model into tables and define:

 - Attributes.
- Primary keys.
- Foreign keys.
- Relationships.
- Constraints.

 **4\. Normalization**

 Analyze the tables to reduce unnecessary redundancy and prevent anomalies.

 **5\. Physical design**

 Determine how the database will actually be implemented, including:

 - Data types.
- Indexes.
- Storage.
- Performance structures.

 **6\. Implementation**

 Create the database using an appropriate database management system.

 **7\. Testing**

 Test:

 - Data integrity.
- Queries.
- Transactions.
- Performance.
- Security.
- Concurrent access.

 **8\. Maintenance**

 Monitor and optimize the database as mechanisms, relationships through primary and foreign keys, and reliable transaction processing through can require significant maintenance and specialized expertise. They can become expensive to operate at scale, particularly when substantial infrastructure is required. Complex queries involving many tables may become difficult to manage or slower as data and workloads grow. Traditional relational systems may also face challenges when for a large organization. Explain how you would decide whether to use a flat-file, hierarchical, or data volume and requirements change.

---

 ### Question 48

 Discuss the advantages and limitations of relational databases.

 ### Model Answer

 Relational databases have several important advantages.

 They provide a simple table-based structure, standardized SQL querying, strong integrity mechanisms, relationships through primary and foreign keys, and reliable transaction processing through ACID properties.

 Normalization can reduce data redundancy and improve consistency.

 However, relational databases also have limitations.

 Large systems can require significant maintenance and specialized expertise. They can become expensive to operate at scale, particularly when substantial infrastructure is required. Complex queries involving many tables may become difficult to manage or slower as data and workloads grow. Traditional relational systems may also face challenges when scaling primarily through vertical expansion.

 The relational model may also be less natural for highly nested or irregular data structures.

 Therefore, relational databases remain highly useful, but the database architecture should be selected according to the application's requirements.

---

 # Part K — Comprehensive Design Question

 ### Question 49

 You have been asked to design a database for an online university system.

 The system must store:

 - Students.
- Lecturers.
- Courses.
- Departments.
- Student enrollments.
- Course grades.

 Requirements:

 - Each department can have many courses.
- Each course belongs to one department.
- A lecturer can teach multiple courses.
- A course can be taught by one lecturer.
- A student can enroll in many courses.
- A course can contain many students.
- Each enrollment can have a grade.

 Design an appropriate relational schema.

 ### Model Answer

 A suitable relational design is:

```
DEPARTMENT
-------------------------
DepartmentID (PK)
DepartmentName

STUDENT
-------------------------
StudentID (PK)
StudentName
Email

LECTURER
-------------------------
LecturerID (PK)
LecturerName
Email
DepartmentID (FK)

COURSE
-------------------------
CourseID (PK)
CourseName
Credits
DepartmentID (FK)
LecturerID (FK)

ENROLLMENT
-------------------------
StudentID (PK, FK)
CourseID (PK, FK)
Grade
EnrollmentDate
```

 ### Relationships

```
DEPARTMENT
    │
    └──────< COURSE
               │
               └────── LECTURER

STUDENT
    │
    └──────< ENROLLMENT >────── COURSE
```

 ### Explanation

 - `DepartmentID` is the primary key of Department.
- `Course.DepartmentID` is a foreign key referencing Department.
- `LecturerID` identifies each lecturer.
- `Course.LecturerID` identifies the lecturer teaching a course.
- Student and Course have a many-to-many relationship.
- `Enrollment` resolves that many-to-many relationship.
- `Grade` belongs in Enrollment because it describes a particular student's result in a particular course.

 This design reduces redundancy and provides clear relationships and referential integrity.

---

 # Part L — Very Short Revision Questions

 ### Question 50

 What does PK stand for?

 ### Answer

 **Primary Key.**

---

 ### Question 51

 What does FK stand for?

 ### Answer

 **Foreign Key.**

---

 ### Question 52

 What does SQL stand for?

 ### Answer

 **Structured Query Language.**

---

 ### Question 53

 What does ACID stand for?

 ### Answer

 **Atomicity, Consistency, Isolation, Durability.**

---

 ### Question 54

 What is a tuple?

 ### Answer

 A tuple is a row or record in a relational table.

---

 ### Question 55

 What is a relation?

 ### Answer

 A relation is essentially a table in the relational database model.

---

 ### Question 56

 What is the main purpose of normalization?

 ### Answer

 To organize data effectively, reduce redundancy, and prevent data anomalies.

---

 ### Question 57

 What type of structure does the hierarchical model use?

 ### Answer

 A tree-like parent-child structure.

---

 ### Question 58

 What is the main structure used by the relational model?

 ### Answer

 Tables consisting of rows and columns.

---

 ### Question 59

 What is the difference between a row and a column?

 ### Answer

 A **row** represents a record, while a **column** represents an attribute or field.

---

 # Final Exam Challenge

 ### Question 60

 You are the database designer for a large organization. Explain how you would decide whether to use a flat-file, hierarchical, or relational database model.

 ### Model Answer

 I would begin by analyzing the organization's requirements rather than choosing a database model based only on popularity.

 For a small and simple dataset with limited relationships, a **flat-file model** might be sufficient. For data with a clear and predictable parent-child structure, a **hierarchical model** could be appropriate.

 For most structured business applications involving multiple entities, complex relationships, data integrity requirements, concurrent users, and transaction processing, a **relational model** would generally be a strong choice.

 I would evaluate:

 - Size and complexity of the data.
- Types of relationships.
- Query requirements.
- Data integrity requirements.
- Transaction requirements.
- Expected workload.
- Scalability requirements.
- Performance requirements.
- Maintenance requirements.
- Cost and available expertise.

 The final decision should balance the organization's functional requirements with performance, scalability, reliability, and long-term maintenance.

---

 # High-Priority Topics to Study

 Before the exam, make sure you can confidently explain:

 1. **Data modeling and its purpose**
2. **Conceptual, logical, and physical models**
3. **Entities, attributes, and relationships**
4. **Primary keys and foreign keys**
5. **Cardinality and relationship types**
6. **Normalization and data redundancy**
7. **Flat-file data model**
8. **Hierarchical data model**
9. **Relational data model**
10. **Advantages of relational databases**
11. **Limitations of relational databases**
12. **SQL and relational databases**
13. **ACID properties**
14. **Atomicity, Consistency, Isolation, and Durability**
15. **Referential integrity**
16. **Database scalability**
17. **Database performance**
18. **Database schema**
19. **Many-to-many relationships and junction tables**
20. **Applying database concepts to real-world design problems**

 ## Key Exam Strategy

 For **definition questions**, give the definition followed by an example.

 For **comparison questions**, discuss both models and then state when each is appropriate.

 For **design questions**, identify:

 **Entities → Attributes → Primary Keys → Relationships → Foreign Keys → Constraints → Normalization**

 For **scenario questions**, always explain **why** your design or choice is appropriate rather than simply naming a database model.

 If you want, I can also turn this into a **100-mark university-style final exam with a separate answer key and marking scheme**.
