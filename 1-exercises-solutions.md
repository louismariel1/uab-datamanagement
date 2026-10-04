# Data Management

 ## Comprehensive Problem Set with Suggested Solutions

 ### Topics Covered

 - Data system principles
- Reliability and fault tolerance
- High availability
- Performance metrics
- Scalability
- Vertical, horizontal, and diagonal scaling
- Static and elastic scaling
- Conceptual data modeling
- ER and EER models
- Entities, attributes, and relationships
- Cardinality and participation
- Specialization/generalization
- Logical/relational modeling
- Primary and foreign keys
- Associative entities
- Normalization and data integrity
- Relational database design
- ACID properties

---

 # Problem 1 — E-Commerce System Architecture, Performance, and Scalability

 ## Scenario

 An international e-commerce company operates an online shopping platform. During normal periods, the system receives approximately **600 requests per second**. During major promotional events such as Black Friday, traffic can increase to **3,000 requests per second**.

 During normal operation:

 - Average throughput = 600 requests/second
- Average network latency = 35 ms
- Average server processing/service time = 15 ms
- Average response time = approximately 50 ms

 During a flash sale:

 - Request rate = 3,000 requests/second
- Average response time = 500 ms
- Database CPU utilization = 95%
- Application-server CPU utilization = 90%

 The company currently uses a single primary database server with one secondary replica.

 ## Tasks

 ### 1\. Performance Analysis

 a. Calculate the approximate response time during normal operation using:

 $$
Response\ Time \approx Network\ Latency + Service\ Time
$$

 b. Explain why the response time during the flash sale increases much more than the normal network latency.

 c. Identify at least three performance bottlenecks that may occur during the flash sale.

 ### 2\. Scalability

 Compare:

 - Vertical scaling
- Horizontal scaling
- Diagonal scaling

 Explain how each could be applied to this e-commerce system.

 ### 3\. Static vs. Elastic Scaling

 Should the company use static scaling or elastic scaling for Black Friday traffic? Explain your answer.

 ### 4\. Reliability and Availability

 The primary database server crashes while customers are placing orders.

 Explain the difference between:

 - Fault tolerance
- High availability

 Explain how database replication can help.

 ### 5\. Design Recommendation

 Propose a high-level architecture capable of handling the flash-sale workload.

---

 ## Suggested Solution

 ### 1\. Performance Analysis

 #### a. Normal response time

 Given:

 - Network latency = 35 ms
- Service time = 15 ms

 Therefore:

 $$
Response\ Time = 35 + 15 = 50ms
$$

 So the expected response time is approximately:

 **50 ms**

 #### b. Why response time increases rapidly

 At low utilization, the system has spare capacity. Requests can be processed immediately.

 As traffic approaches the maximum capacity of the servers and database, queues begin to form.

 Therefore, response time becomes:

 $$
Response\ Time =
Waiting\ Time + Service\ Time + Network\ Time
$$

 During the flash sale, CPU utilization reaches approximately 90–95%. This causes requests to wait for:

 - CPU resources
- Database connections
- Database locks
- I/O operations
- Network resources
- Application-server threads

 Consequently, the increase in response time is caused not simply by network latency but by **queueing and resource contention**.

 ### c. Possible bottlenecks

 Possible bottlenecks include:

 1. Database CPU saturation
2. Application-server CPU saturation
3. Database connection limits
4. Slow queries
5. Lock contention
6. Disk/storage I/O
7. Network congestion
8. Insufficient application-server capacity

---

 ### 2\. Scaling Strategies

 #### Vertical scaling

 Increase the resources of an existing server.

 For example:

 - More CPU cores
- More RAM
- Faster SSDs
- Higher network capacity

 **Advantage:** relatively simple.

 **Disadvantage:** hardware has physical limits and can become expensive.

---

 #### Horizontal scaling

 Add more servers.

 For example:

```
              Load Balancer
              /     |     \
             /      |      \
        Server 1 Server 2 Server 3
```

 Requests are distributed among multiple application servers.

 **Advantage:** provides much greater scalability and redundancy.

 **Disadvantage:** distributed systems are more complex.

---

 #### Diagonal scaling

 Combine vertical and horizontal scaling.

 For example:

 - Upgrade existing servers
- Add additional application servers
- Add database replicas

 This is often the most practical approach for a large e-commerce platform.

---

 ### 3\. Static vs. Elastic Scaling

 **Elastic scaling is recommended.**

 Black Friday creates temporary demand spikes. Maintaining enough servers for peak traffic throughout the entire year would waste resources.

 Elastic scaling allows the system to:

 - Add resources when demand increases
- Remove resources when demand decreases

 For example:

```
Normal:
3 application servers

Peak:
10 application servers

After sale:
3 application servers
```

 This improves both scalability and cost efficiency.

---

 ### 4\. Fault Tolerance vs. High Availability

 **Fault tolerance** means that the system can continue operating despite a component failure.

 **High availability** means minimizing downtime and ensuring that the service remains accessible.

 If the primary database crashes:

```
Primary DB  X
    |
    | replication
    v
Secondary DB
```

 The secondary can potentially be promoted to become the new primary.

 This provides:

 - Reduced downtime
- Improved availability
- Recovery from server failure

 However, replication alone does not guarantee zero data loss. The exact behavior depends on whether replication is synchronous or asynchronous.

---

 ### 5\. Recommended Architecture

 A possible architecture is:

```
                    Internet
                       |
                 Load Balancer
                       |
        -------------------------------
        |              |              |
    App Server 1   App Server 2   App Server 3
        |              |              |
        -------- Application Layer ----
                       |
                Database Cluster
                  /          \
             Primary       Replica
```

 Additional techniques could include:

 - Database indexing
- Query optimization
- Caching
- Read replicas
- Connection pooling
- Monitoring
- Automatic failover
- Elastic application-server scaling

---

 # Problem 2 — Healthcare and Telemedicine Database

 ## Scenario

 A healthcare platform manages doctors, patients, consultations, prescriptions, medications, and medical history.

 ### Users

 Every user has:

 - NIF/DNI
- Full name
- Phone
- Email

 Doctors additionally have:

 - Medical license number
- Specialty

 Patients may schedule multiple consultations.

 ### Consultations

 Each consultation contains:

 - Consultation ID
- Date
- Time
- Status
- Fee

 A consultation involves exactly one patient and one doctor.

 ### Prescriptions

 A doctor may prescribe zero or more medications during a consultation.

 Each prescription item records:

 - Medication
- Dosage
- Duration
- Administration instructions

 ### Medications

 Each medication has:

 - Drug code
- Trade name
- Active ingredient
- Warehouse stock level

 ### Medical History

 Each patient can have multiple medical-history entries.

 Each entry contains:

 - Entry ID
- Entry date
- Diagnosis notes
- Doctor responsible

---

 ## Tasks

 ### 1\. Conceptual Model

 Identify:

 - Entities
- Attributes
- Primary identifiers
- Relationships
- Cardinalities
- EER specialization, if appropriate

 ### 2\. Logical Model

 Convert the conceptual model into relational tables.

 ### 3\. Integrity Constraints

 Identify suitable:

 - Primary keys
- Foreign keys
- Unique constraints
- Domain constraints

 ### 4\. Normalization

 Explain how your design avoids unnecessary duplication.

---

 ## Suggested Solution

 ### 1\. Conceptual Model

 A suitable EER model contains:

 ### User

 Attributes:

 - NIF/DNI
- FullName
- Phone
- Email

 A possible specialization is:

```
                 USER
                /    \
               /      \
          PATIENT     DOCTOR
```

 Doctor-specific attributes:

 - MedicalLicenseNo
- Specialty

---

 ### Patient — Consultation

 A patient can have many consultations.

 A consultation belongs to exactly one patient.

 Therefore:

```
PATIENT 1 -------- N CONSULTATION
```

---

 ### Doctor — Consultation

 A doctor can conduct many consultations.

 Each consultation has one doctor.

 Therefore:

```
DOCTOR 1 -------- N CONSULTATION
```

---

 ### Consultation — Medication

 A consultation can prescribe multiple medications.

 A medication can be prescribed in many consultations.

 Therefore:

```
CONSULTATION M -------- N MEDICATION
```

 This many-to-many relationship requires an associative entity:

 **PRESCRIPTION**

 Attributes:

 - ConsultationID
- DrugCode
- Dosage
- Duration
- Instructions

---

 ### Patient — Medical History

 A patient can have many history entries.

 Each history entry belongs to one patient.

```
PATIENT 1 -------- N MEDICAL_HISTORY
```

 Each history entry is created by one doctor:

```
DOCTOR 1 -------- N MEDICAL_HISTORY
```

---

 ## 2\. Logical Model

 ### USER

```
USER(
    NIF_DNI PK,
    FullName,
    Phone,
    Email
)
```

 ### PATIENT

```
PATIENT(
    NIF_DNI PK FK → USER(NIF_DNI)
)
```

 ### DOCTOR

```
DOCTOR(
    NIF_DNI PK FK → USER(NIF_DNI),
    MedicalLicenseNo UNIQUE,
    Specialty
)
```

 ### CONSULTATION

```
CONSULTATION(
    ConsultationID PK,
    PatientNIF FK → PATIENT(NIF_DNI),
    DoctorNIF FK → DOCTOR(NIF_DNI),
    ConsultationDate,
    ConsultationTime,
    Status,
    Fee
)
```

 ### MEDICATION

```
MEDICATION(
    DrugCode PK,
    TradeName,
    ActiveIngredient,
    StockLevel
)
```

 ### PRESCRIPTION

```
PRESCRIPTION(
    ConsultationID PK FK → CONSULTATION(ConsultationID),
    DrugCode PK FK → MEDICATION(DrugCode),
    Dosage,
    DurationDays,
    Instructions
)
```

 The composite primary key:

```
(ConsultationID, DrugCode)
```

 ensures that a medication is not duplicated within the same consultation.

 ### MEDICAL\_HISTORY

```
MEDICAL_HISTORY(
    EntryID PK,
    PatientNIF FK → PATIENT(NIF_DNI),
    DoctorNIF FK → DOCTOR(NIF_DNI),
    EntryDate,
    DiagnosisNotes
)
```

---

 ## 3\. Integrity Constraints

 Examples:

 - NIF/DNI must be unique.
- Medical license number must be unique.
- Consultation ID must be unique.
- Drug code must be unique.
- Fee should not be negative.
- Stock level should not be negative.
- Status should be restricted to:
  - Scheduled
  - Completed
  - Canceled
- Foreign keys ensure that referenced patients, doctors, and medications exist.

---

 ## 4\. Normalization

 The design separates different concepts into different tables.

 For example, doctor information is not repeated in every consultation.

 Instead:

```
DOCTOR
   |
   +---- CONSULTATION
```

 Similarly, medication information is stored once in `MEDICATION`, while prescription-specific information is stored in `PRESCRIPTION`.

 This reduces:

 - Data duplication
- Update anomalies
- Insert anomalies
- Delete anomalies

---

 # Problem 3 — E-Learning and Course Management System

 ## Scenario

 An educational academy operates multiple regional centers.

 ### Centers

 Each center has:

 - CenterID
- City
- Address
- Phone
- Director

 ### Users

 Users are either:

 - Instructors
- Students

 All users have:

 - UserID
- Full name
- Email
- Registration date

 Instructors additionally have:

 - Academic degree
- Hourly rate

 Students additionally have:

 - Highest education level

 ### Courses

 Each course has:

 - CourseCode
- Title
- Total hours
- Fee
- Category

 Each course belongs to one primary center.

 A course can have zero or more prerequisite courses.

 ### Enrollments

 Students enroll in courses.

 An enrollment records:

 - Enrollment date
- Payment status
- Final grade
- Optional review
- Optional rating

 ### Live Classes

 Instructors teach live classes.

 Each class records:

 - Class ID
- Date
- Start time
- Duration
- Meeting URL
- Instructor

---

 ## Tasks

 ### 1\. Design the EER model.

 Identify entities, attributes, relationships, cardinalities, and specialization.

 ### 2\. Identify the recursive relationship involving courses.

 ### 3\. Convert the model to a relational schema.

 ### 4\. Explain how the many-to-many relationship between students and courses should be represented.

---

 ## Suggested Solution

 ### 1\. EER Model

 A suitable specialization is:

```
                   USER
                  /    \
                 /      \
          INSTRUCTOR   STUDENT
```

---

 ### Center — Course

 Each course belongs to exactly one center.

 A center can have many courses.

```
CENTER 1 -------- N COURSE
```

---

 ### Student — Course

 A student can enroll in many courses.

 A course can have many students.

 Therefore:

```
STUDENT M -------- N COURSE
```

 This requires an associative entity:

 **ENROLLMENT**

---

 ### Course Prerequisites

 A course can have multiple prerequisite courses.

 A course can also be a prerequisite for multiple other courses.

 Therefore, this is a recursive many-to-many relationship:

```
COURSE M -------- N COURSE
```

 This can be represented using:

 **COURSE\_PREREQUISITE**

---

 ### Course — Live Class

 A course can have many live classes.

 Each live class belongs to one course.

```
COURSE 1 -------- N LIVE_CLASS
```

---

 ### Instructor — Live Class

 An instructor can conduct multiple classes.

 Each class is conducted by one instructor.

```
INSTRUCTOR 1 -------- N LIVE_CLASS
```

---

 ## 2\. Recursive Relationship

 The recursive relationship is:

```
COURSE
   |
   | prerequisite for
   |
COURSE
```

 For example:

```
Database Fundamentals
          |
          v
Advanced Database Systems
```

 `Database Fundamentals` is a prerequisite for `Advanced Database Systems`.

---

 ## 3\. Logical Model

 ### USER

```
USER(
    UserID PK,
    FullName,
    Email,
    RegistrationDate
)
```

 ### INSTRUCTOR

```
INSTRUCTOR(
    UserID PK FK → USER(UserID),
    AcademicDegree,
    HourlyRate
)
```

 ### STUDENT

```
STUDENT(
    UserID PK FK → USER(UserID),
    HighestEducation
)
```

 ### CENTER

```
CENTER(
    CenterID PK,
    City,
    Address,
    Phone,
    Director
)
```

 ### COURSE

```
COURSE(
    CourseCode PK,
    Title,
    TotalHours,
    Fee,
    Category,
    CenterID FK → CENTER(CenterID)
)
```

 ### ENROLLMENT

```
ENROLLMENT(
    StudentID PK FK → STUDENT(UserID),
    CourseCode PK FK → COURSE(CourseCode),
    EnrollmentDate,
    PaymentStatus,
    FinalGrade,
    Review,
    Rating
)
```

 ### COURSE\_PREREQUISITE

```
COURSE_PREREQUISITE(
    CourseCode PK FK → COURSE(CourseCode),
    PrerequisiteCode PK FK → COURSE(CourseCode)
)
```

 ### LIVE\_CLASS

```
LIVE_CLASS(
    ClassID PK,
    CourseCode FK → COURSE(CourseCode),
    InstructorID FK → INSTRUCTOR(UserID),
    ClassDate,
    StartTime,
    Duration,
    MeetingURL
)
```

---

 ## 4\. Why ENROLLMENT Is Necessary

 The relationship between students and courses is many-to-many.

 A student may take:

```
Database
Programming
Networks
```

 while a course may contain:

```
Student A
Student B
Student C
```

 Therefore, storing `CourseCode` directly in `STUDENT` would not correctly represent the relationship.

 The associative entity `ENROLLMENT` resolves the M:N relationship.

 It also provides a place for relationship-specific attributes such as:

 - EnrollmentDate
- PaymentStatus
- FinalGrade
- Review
- Rating

---

 # Problem 4 — Logistics and Freight Delivery Network

 ## Scenario

 A global logistics company operates warehouses, trucks, cargo planes, shipments, and packages.

 ### Hubs

 Each hub has:

 - HubCode
- City
- Country
- Storage capacity
- Manager

 ### Vehicles

 All vehicles have:

 - VehicleID
- AcquisitionDate
- MaximumWeightCapacity
- OperationalStatus
- HomeHub

 Vehicles are specialized into:

 - Trucks
- Cargo planes

 Trucks additionally record:

 - Maximum road distance

 Cargo planes record:

 - Maximum flight altitude
- Wingspan

 ### Customers

 Customers have:

 - CustomerID
- Name
- Phone
- BillingAddress

 ### Shipments

 Customers create shipments.

 Each shipment contains one or more packages.

 ### Packages

 Each package has:

 - TrackingCode
- Weight
- Volume
- DeclaredValue
- Priority

 ### Transit History

 A package can pass through multiple hubs.

 For every transit event, record:

 - Package
- Hub
- Arrival date/time
- Departure date/time
- Vehicle used

---

 ## Tasks

 ### 1\. Design the EER conceptual model.

 ### 2\. Identify the vehicle specialization/generalization.

 ### 3\. Identify all many-to-many relationships.

 ### 4\. Convert the conceptual model into a relational schema.

 ### 5\. Explain how transit history should be represented.

---

 ## Suggested Solution

 ## 1\. Conceptual Model

 ### Hub

 Attributes:

 - HubCode
- City
- Country
- StorageCapacity
- Manager

---

 ### Vehicle

 Supertype:

```
VEHICLE
   /     \
  /       \
TRUCK    CARGO_PLANE
```

 Common attributes:

 - VehicleID
- AcquisitionDate
- MaxWeightCapacity
- OperationalStatus

 Truck-specific:

 - MaxRoadDistance

 Plane-specific:

 - MaxFlightAltitude
- Wingspan

---

 ### Hub — Vehicle

 Every vehicle has one home hub.

 A hub can have multiple vehicles.

```
HUB 1 -------- N VEHICLE
```

---

 ### Customer — Shipment

 A customer can create multiple shipments.

 Each shipment belongs to one customer.

```
CUSTOMER 1 -------- N SHIPMENT
```

---

 ### Shipment — Package

 A shipment contains one or more packages.

 Assuming each package belongs to exactly one shipment:

```
SHIPMENT 1 -------- N PACKAGE
```

---

 ### Package — Hub

 A package can pass through many hubs.

 A hub handles many packages.

 Therefore:

```
PACKAGE M -------- N HUB
```

 This relationship has attributes:

 - ArrivalDateTime
- DepartureDateTime
- VehicleID

 Therefore it should be represented by an associative entity such as:

 **TRANSIT**

---

 ### Vehicle — Transit

 A vehicle can be used for many transit legs.

 Each transit record uses one vehicle.

```
VEHICLE 1 -------- N TRANSIT
```

---

 # 2\. Vehicle Specialization

 The EER model uses generalization:

```
                    VEHICLE
                       |
              -------------------
              |                 |
            TRUCK          CARGO_PLANE
```

 Common properties are stored in `VEHICLE`.

 Specialized properties are stored in the subtype tables.

 This avoids repeating common vehicle information.

---

 # 3\. Many-to-Many Relationships

 The main M:N relationship is:

```
PACKAGE M -------- N HUB
```

 because:

 - One package can visit multiple hubs.
- One hub can process multiple packages.

 It is resolved using:

```
TRANSIT
```

---

 # 4\. Logical Model

 ### HUB

```
HUB(
    HubCode PK,
    City,
    Country,
    StorageCapacity,
    Manager
)
```

 ### VEHICLE

```
VEHICLE(
    VehicleID PK,
    AcquisitionDate,
    MaxWeightCapacity,
    OperationalStatus,
    HomeHub FK → HUB(HubCode)
)
```

 ### TRUCK

```
TRUCK(
    VehicleID PK FK → VEHICLE(VehicleID),
    MaxRoadDistance
)
```

 ### CARGO\_PLANE

```
CARGO_PLANE(
    VehicleID PK FK → VEHICLE(VehicleID),
    MaxFlightAltitude,
    Wingspan
)
```

 ### CUSTOMER

```
CUSTOMER(
    CustomerID PK,
    Name,
    Phone,
    BillingAddress
)
```

 ### SHIPMENT

```
SHIPMENT(
    ShipmentID PK,
    CustomerID FK → CUSTOMER(CustomerID)
)
```

 ### PACKAGE

```
PACKAGE(
    TrackingCode PK,
    ShipmentID FK → SHIPMENT(ShipmentID),
    Weight,
    Volume,
    DeclaredValue,
    Priority
)
```

 ### TRANSIT

```
TRANSIT(
    TrackingCode PK FK → PACKAGE(TrackingCode),
    HubCode PK FK → HUB(HubCode),
    ArrivalDateTime,
    DepartureDateTime,
    VehicleID FK → VEHICLE(VehicleID)
)
```

 A more robust implementation could add a `TransitSequence` or `TransitID` to explicitly preserve the order of hub visits.

 For example:

```
TRANSIT(
    TransitID PK,
    TrackingCode FK,
    HubCode FK,
    TransitSequence,
    ArrivalDateTime,
    DepartureDateTime,
    VehicleID FK
)
```

 This is preferable when a package might visit the same hub more than once.

---

 # Problem 5 — Data Modeling and Normalization

 ## Scenario

 Consider the following poorly designed table:

```
STUDENT_COURSE(
    StudentID,
    StudentName,
    StudentEmail,
    CourseID,
    CourseName,
    InstructorID,
    InstructorName,
    InstructorEmail,
    Grade
)
```

 A student can enroll in multiple courses, and each course can have many students.

 ## Tasks

 1. Identify possible redundancy problems.
2. Identify insertion, update, and deletion anomalies.
3. Identify the likely primary key.
4. Normalize the design into separate relations.
5. Identify the foreign keys.

---

 ## Suggested Solution

 ### 1\. Redundancy

 Suppose student `S01` takes three courses.

 Their:

 - StudentName
- StudentEmail

 will appear three times.

 Likewise, instructor information will be repeated for every student taking the instructor's course.

 This creates unnecessary duplication.

---

 ### 2\. Update anomaly

 If an instructor changes their email address, multiple rows may need to be updated.

 If one row is not updated, inconsistent information exists.

---

 ### 3\. Insertion anomaly

 Suppose a new course exists but no student has enrolled yet.

 The table may not allow the course to be stored without student information.

---

 ### 4\. Deletion anomaly

 If the only student enrolled in a course is deleted, information about the course may also disappear.

---

 ### 5\. Normalized Design

 A better design is:

```
STUDENT(
    StudentID PK,
    StudentName,
    StudentEmail
)
```

```
INSTRUCTOR(
    InstructorID PK,
    InstructorName,
    InstructorEmail
)
```

```
COURSE(
    CourseID PK,
    CourseName,
    InstructorID FK
)
```

```
ENROLLMENT(
    StudentID PK FK,
    CourseID PK FK,
    Grade
)
```

 The composite key of `ENROLLMENT` is:

```
(StudentID, CourseID)
```

 This design substantially reduces duplication and improves data integrity.

---

 # Problem 6 — Keys and Referential Integrity

 ## Scenario

 Consider these relations:

```
CUSTOMER(
    CustomerID,
    Name,
    Email
)
```

```
ORDER(
    OrderID,
    OrderDate,
    CustomerID
)
```

```
ORDER_ITEM(
    OrderID,
    ProductID,
    Quantity
)
```

 ## Tasks

 1. Identify the primary key of each table.
2. Identify the foreign keys.
3. Identify the composite key.
4. Explain referential integrity.
5. Explain what should happen if an order is deleted.

---

 ## Suggested Solution

 ### Primary Keys

```
CUSTOMER
PK = CustomerID
```

```
ORDER
PK = OrderID
```

```
ORDER_ITEM
PK = (OrderID, ProductID)
```

---

 ### Foreign Keys

```
ORDER.CustomerID
    → CUSTOMER.CustomerID
```

 and:

```
ORDER_ITEM.OrderID
    → ORDER.OrderID
```

 `ProductID` would also reference a `PRODUCT` table.

---

 ### Referential Integrity

 Referential integrity ensures that a foreign cannot be recorded-key value corresponds to an existing referenced record.

 For example, an order cannot reference:

```
CustomerID = 9999
```

 if customer 9999 does not exist.

---

 ### Deleting an Order

 If an order is deleted, its order items should normally also be removed.

 Possible implementation:

```
ON DELETE CASCADE
```

 Alternatively, deletion can be prevented if business rules require historical orders to remain permanently stored.

---

 # Problem 7 — ACID Transactions

 ## Scenario

 A customer purchases a product for €500.

 The transaction must:

 1. Deduct one item from inventory.
2. Create the order.
3. Record the payment.
4. Confirm the transaction.

 Suppose the database crashes after step 2 but before step 3.

 ## Tasks

 Explain how each ACID property applies.

---

 ## Suggested Solution

 ### Atomicity

 The transaction is **all-or-nothing**.

 If payment cannot be recorded, the earlier changes should be rolled back.

 Therefore, the system should not leave:

```
Inventory = reduced
Order = created
Payment = missing
```

 as a partially completed transaction.

---

 ### Consistency

 The transaction must preserve database rules.

 For example:

```
Inventory >= 0
```

 and every order must refer to a valid customer.

---

 ### Isolation

 If two customers attempt to purchase the last available product simultaneously, their transactions should not incorrectly interfere with one another.

 The database must prevent both transactions from incorrectly believing they purchased the same final item.

---

 ### Durability

 Once the transaction is committed, the changes must survive a crash.

 Therefore, after a successful commit:

```
Order exists
Payment exists
Inventory updated
```

 must remain true even if the database server subsequently fails.

---

 # Problem 8 — Comprehensive Integrated Database Design

 ## Scenario

 A university wants to build a database for its academic system.

 The university contains multiple departments.

 Each department has:

 - DepartmentID
- Name
- Office location

 Each department employs multiple lecturers.

 Each lecturer has:

 - LecturerID
- Name
- Email
- Academic rank

 Students have:

 - StudentID
- Name
- Email
- Date of birth

 The university offers courses.

 Each course has:

 - CourseID
- Course title
- Credits
- Department

 Students can enroll in many courses.

 A course can have many students.

 For every enrollment, the system stores:

 - Enrollment date
- Grade

 A course can have prerequisite courses.

 Lecturers teach courses.

 Some courses may be taught by multiple lecturers.

---

 ## Tasks

 ### A. Conceptual Modeling

 Identify:

 1. All entities
2. Attributes
3. Primary identifiers
4. Relationships
5. Cardinalities
6. Many-to-many relationships
7. Recursive relationships

 ### B. Logical Modeling

 Create a normalized relational schema.

 ### C. Explain your design decisions.

---

 ## Suggested Solution

 ### A. Entities

 The main entities are:

 - DEPARTMENT
- LECTURER
- STUDENT
- COURSE
- ENROLLMENT

 A recursive relationship exists between `COURSE` and itself.

---

 ### Relationships

 #### Department — Lecturer

```
DEPARTMENT 1 -------- N LECTURER
```

 #### Department — Course

```
DEPARTMENT 1 -------- N COURSE
```

 #### Student — Course

```
STUDENT M -------- N COURSE
```

 Resolved through:

```
ENROLLMENT
```

 #### Lecturer — Course

 Because a course may have multiple lecturers and a lecturer may teach multiple courses:

```
LECTURER M -------- N COURSE
```

 Resolved through:

```
TEACHING
```

 #### Course prerequisites

 Recursive M:N:

```
COURSE M -------- N COURSE
```

 Resolved through:

```
COURSE_PREREQUISITE
```

---

 ## B. Relational Schema

 ### DEPARTMENT

```
DEPARTMENT(
    DepartmentID PK,
    Name,
    OfficeLocation
)
```

 ### LECTURER

```
LECTURER(
    LecturerID PK,
    Name,
    Email,
    AcademicRank,
    DepartmentID FK → DEPARTMENT(DepartmentID)
)
```

 ### STUDENT

```
STUDENT(
    StudentID PK,
    Name,
    Email,
    DateOfBirth
)
```

 ### COURSE

```
COURSE(
    CourseID PK,
    CourseTitle,
    Credits,
    DepartmentID FK → DEPARTMENT(DepartmentID)
)
```

 ### ENROLLMENT

```
ENROLLMENT(
    StudentID PK FK → STUDENT(StudentID),
    CourseID PK FK → COURSE(CourseID),
    EnrollmentDate,
    Grade
)
```

 ### TEACHING

```
TEACHING(
    LecturerID PK FK → LECTURER(LecturerID),
    CourseID PK FK → COURSE(CourseID)
)
```

 ### COURSE\_PREREQUISITE

```
COURSE_PREREQUISITE(
    CourseID PK FK → COURSE(CourseID),
    PrerequisiteCourseID PK FK → COURSE(CourseID)
)
```

---

 # Problem 9 — Design Evaluation and Critical Thinking

 ## Scenario

 A student proposes the following database design:

```
STUDENT(
    StudentID,
    Name,
    Course1,
    Course2,
    Course3,
    Instructor1,
    Instructor2,
    Instructor3
)
```

 ## Tasks

 1. Identify at least four problems with this design.
2. Explain why it violates good relational database design principles.
3. Redesign it.
4. Explain how your redesign improves scalability and maintainability.

---

 ## Suggested Solution

 ### Problems

 The design contains:

 - Repeating groups
- Fixed limits on the number of courses
- Duplication
- Poor representation of relationships
- Difficulty querying data
- Update anomalies
- Poor scalability

 For example, what happens when a student takes a fourth course?

 A new column would be required:

```
Course4
```

 This demonstrates poor scalability at the schema level.

---

 ### Improved Design

 Use separate relations:

```
STUDENT(
    StudentID PK,
    Name
)
```

```
COURSE(
    CourseID PK,
    CourseName
)
```

```
INSTRUCTOR(
    InstructorID PK,
    InstructorName
)
```

```
ENROLLMENT(
    StudentID PK FK,
    CourseID PK FK
)
```

```
TEACHING(
    InstructorID PK FK,
    CourseID PK FK
)
```

 The number of courses is now unlimited without modifying the database structure.

---

 # Problem 10 — Integrated Exam-Style Question

 ## Scenario

 A food-delivery company wants a database to manage customers, restaurants, drivers, orders, meals, payments, and deliveries.

 A customer can place many orders.

 An order belongs to exactly one customer and one restaurant.

 An order can contain multiple meals.

 A meal can appear in many orders.

 Each order has one payment.

 A driver can deliver many orders, but each order is assigned to one driver.

 Restaurants offer many meals.

 Each meal belongs to one restaurant.

 The system must also record the quantity of each meal in an order and the price charged at the time of purchase.

 ## Tasks

 1. Draw or describe the conceptual ER model.
2. Identify all 1:N relationships.
3. Identify all M:N relationships.
4. Identify associative entities.
5. Identify attributes belonging to relationships rather than entities.
6. Convert the model into a relational schema.
7. Explain how the schema supports normalization.

---

 ## Suggested Solution

 ### Entities

 - CUSTOMER
- RESTAURANT
- DRIVER
- ORDER
- MEAL
- PAYMENT

 Associative entity:

 - ORDER\_ITEM

---

 ### 1:N Relationships

```
CUSTOMER 1 -------- N ORDER
```

```
RESTAURANT 1 -------- N ORDER
```

```
RESTAURANT 1 -------- N MEAL
```

```
DRIVER 1 -------- N ORDER
```

```
ORDER 1 -------- 1 PAYMENT
```

---

 ### M:N Relationship

 Customers and restaurants do not necessarily have a direct M:N relationship in this model.

 The major M:N relationship is:

```
ORDER M -------- N MEAL
```

 This is resolved by:

```
ORDER_ITEM
```

---

 ### Relationship Attributes

 The following belong to `ORDER_ITEM` rather than `MEAL`:

 - Quantity
- PriceAtPurchase

 This is important because the same meal may be ordered:

```
Quantity = 2
PriceAtPurchase = €12
```

 in one order and:

```
Quantity = 1
PriceAtPurchase = €14
```

 in another order.

---

 ## Relational Schema

 ### CUSTOMER

```
CUSTOMER(
    CustomerID PK,
    Name,
    Phone,
    Email
)
```

 ### RESTAURANT

```
RESTAURANT(
    RestaurantID PK,
    Name,
    Address
)
```

 ### DRIVER

```
DRIVER(
    DriverID PK,
    Name,
    Phone
)
```

 ### MEAL

```
MEAL(
    MealID PK,
    RestaurantID FK → RESTAURANT(RestaurantID),
    Name,
    CurrentPrice
)
```

 ### ORDER

```
ORDER(
    OrderID PK,
    CustomerID FK → CUSTOMER(CustomerID),
    RestaurantID FK → RESTAURANT(RestaurantID),
    DriverID FK → DRIVER(DriverID),
    OrderDateTime,
    Status
)
```

 ### ORDER\_ITEM

```
ORDER_ITEM(
    OrderID PK FK → ORDER(OrderID),
    MealID PK FK → MEAL(MealID),
    Quantity,
    PriceAtPurchase
)
```

 ### PAYMENT

```
PAYMENT(
    PaymentID PK,
    OrderID UNIQUE FK → ORDER(OrderID),
    PaymentDate,
    Amount,
    PaymentMethod,
    PaymentStatus
)
```

---

 # Final Review Questions

 Before submitting any database-design solution, verify the following:

 1. Have all important entities been identified?
2. Does every entity have an appropriate primary key?
3. Are attributes assigned to the correct entity?
4. Have 1:1, 1:N, and M:N relationships been identified?
5. Have M:N relationships been resolved using associative entities?
6. Are relationship-specific attributes stored in the relationship/associative entity?
7. Have recursive relationships been identified?
8. Have EER specializations/generalizations been considered?
9. Does every foreign key reference an appropriate primary key or candidate key?
10. Does the design avoid unnecessary duplication?
11. Does the design reduce insertion, update, and deletion anomalies?
12. Is the design appropriately normalized?
13. Are important integrity constraints identified?
14. Can the schema scale when the number of records increases?
15. Does the database design support consistency and reliability?

 ## Key Exam Principle

 A strong database-design answer should normally progress through:

```
Requirements
     ↓
Entities & Attributes
     ↓
Relationships
     ↓
Cardinalities
     ↓
ER/EER Conceptual Model
     ↓
Relational Schema
     ↓
Primary & Foreign Keys
     ↓
Normalization
     ↓
Integrity Constraints
```

 The most important distinction to remember is:

 **Conceptual model = what data and relationships exist.**

 **Logical model = how those entities and relationships are represented as relations/tables.**

 **Physical model = how the database is actually implemented and optimized using storage structures, indexes, partitions, and other implementation details.**

 This version is designed to work both as a **practice problem set and an exam-preparation set**, with the solutions showing the reasoning rather than only giving final schemas.
