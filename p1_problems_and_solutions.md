# Data Management — Exam Problems and Solutions

Data Management 2026–27 · Exam revision · Reliability, Scalability, Performance Metrics, ER/EER Modeling and Relational Schemas

This revision sheet provides the four exam problems with model answers, including calculations, conceptual models, relationship cardinalities, primary keys (PKs), foreign keys (FKs), and relational schemas.

# Problem 1 — System Architecture & Scalability Analysis

## 1.1 Performance metrics

Question: An e-commerce platform processes 500 requests per second with latency of 40 ms and service time of 10 ms. During peak hours, traffic rises to 2,500 requests per second and response time increases to 450 ms.

Calculate the initial response time and explain the performance degradation.

### Solution

The basic formula is:

$$
\text{Response Time}=\text{Latency}+\text{Service Time}
$$

Normal operation:

$$
RT=40+10=\boxed{50\text{ ms}}
$$

During peak hours, the response time rises from 50 ms to 450 ms.

- Traffic increase: $2500/500=\boxed{5\times}$
- Response-time increase: $450/50=\boxed{9\times}$
- Absolute increase: $450-50=\boxed{400\text{ ms}}$

Why does performance degrade so significantly?

As the incoming request rate approaches or exceeds the system's processing capacity, requests must wait in queues. Queueing delay can grow non-linearly as utilization becomes high. Consequently, a fivefold increase in traffic can produce a much larger increase in response time.

Remember the distinction:

- Latency: Time associated with communication and waiting, depending on the metric's definition.
- Service time: Time spent processing a request.
- Response time: Total time experienced by the request, including relevant waiting and processing.

In this question, use the supplied formula of latency plus service time for the initial calculation. At peak load, the 450 ms response time indicates substantial additional delay, such as queueing.

## 1.2 Scaling strategies

| Strategy | Definition | Advantages | Limitations |
| --- | --- | --- | --- |
| Vertical scaling (scale up) | Add CPU, RAM or other resources to one machine. | Simple and may improve performance quickly. | Hardware limits; upgrades may require downtime. |
| Horizontal scaling (scale out) | Add more server instances. | Distributes traffic; supports greater capacity and redundancy. | Requires load balancing and coordination. |
| Diagonal scaling | Increase machine capacity and add more instances. | Combines the benefits of vertical and horizontal scaling. | More complex and potentially more expensive. |

Recommended solution: Elastic horizontal scaling.

A flash sale produces temporary and potentially unpredictable traffic spikes. Elastic horizontal scaling allows the platform to:

1. Add server instances when traffic, CPU usage or queue length increases.
2. Distribute requests using a load balancer.
3. Remove unnecessary instances when demand falls.
4. Reduce costs compared with maintaining peak capacity at all times.

### Static vs. elastic scaling

- Static scaling: Capacity is provisioned in advance and remains relatively fixed.
- Elastic scaling: Capacity automatically increases or decreases in response to demand.

Elastic scaling is the better choice for unpredictable flash-sale traffic. However, scaling policies should include suitable thresholds and warm-up time because automatic provisioning is not instantaneous.

## 1.3 Fault tolerance vs. high availability

Scenario: The primary database node crashes while a secondary replica exists.

| Feature | High availability (HA) | Fault tolerance (FT) |
| --- | --- | --- |
| Main goal | Minimize downtime. | Continue operation despite component failure, ideally without interruption. |
| During failure | A secondary is promoted through failover. | Redundant components continue service with minimal or no interruption. |
| Downtime | Brief downtime may occur. | Designed to avoid service interruption. |
| Data loss | Possible, depending on replication and recovery. | Strong designs aim to avoid losing acknowledged transactions. |
| Typical approach | Replication, monitoring and automatic failover. | Redundant components and coordinated operation, often with synchronous replication. |

Model answer: High availability aims to minimize downtime by failing over to a secondary node. A brief interruption may occur while the secondary is promoted. Fault tolerance aims to maintain service despite the failure, ideally without interruption or loss of acknowledged transactions.

Important: Replication alone does not guarantee fault tolerance. The outcome depends on replication mode, failover design, failure detection and transaction guarantees. Likewise, no architecture can guarantee zero downtime under every possible failure.

### Key exam facts

- Response time = latency + service time under the problem's stated definition.
- Queueing delay increases sharply near capacity.
- Vertical = bigger machine; horizontal = more machines; diagonal = both.
- Elastic scaling adjusts capacity to demand.
- HA minimizes downtime; FT aims to continue operation through failures.

# Problem 2 — Healthcare & Telemedicine Platform

## 2.1 Conceptual model (ER/EER)

The conceptual model identifies entities, attributes, keys and relationships before implementation in SQL.

### A. Entities and attributes

| Entity | Attributes | Primary key |
| --- | --- | --- |
| User | NIF, first\_name, last\_name, phone, email | NIF |
| Doctor | NIF, license\_number, specialty | NIF |
| Patient | NIF | NIF |
| Consultation | consultation\_id, date, time, status, fee | consultation\_id |
| Medication | drug\_code, trade\_name, active\_ingredient, stock\_quantity | drug\_code |
| MedicalHistoryEntry | entry\_id, entry\_date, diagnosis\_notes | entry\_id |

EER specialization:

- `User` is the superclass.
- `Doctor` and `Patient` are subclasses.
- Both subclasses inherit the user's NIF and common contact attributes.
- `Doctor` adds `license_number` and `specialty`.
- The license number should be unique.

This structure avoids duplicating user details across doctor and patient records.

### B. Relationships and cardinalities

1. Patient — Consultation (Schedules/Receives)

Patient

1 : N

Schedules

Consultation

- A patient may have zero or many consultations (0..N).
- Each consultation belongs to exactly one patient (1..1).

2. Doctor — Consultation (Conducts)

Doctor

1 : N

Conducts

Consultation

- A doctor may conduct zero or many consultations (0..N).
- Each consultation is conducted by exactly one doctor (1..1).

3. Consultation — Medication (Prescribes)

Consultation

M : N

Prescribes

Medication

- A consultation may prescribe zero or many medications.
- A medication may appear in zero or many consultations.
- Relationship attributes: dosage, duration\_days, instructions.

4. Patient — MedicalHistoryEntry (Has)

Patient

1 : N

Has entries

History entry

- A patient may have zero or many history entries.
- Each entry belongs to exactly one patient.

5. Doctor — MedicalHistoryEntry (Creates)

Doctor

1 : N

Creates

History entry

- A doctor may create zero or many entries.
- Each entry is created by exactly one doctor.

## 2.2 Logical model (relational schema)

Notation: `PK` = primary key; `FK` = foreign key; `UNIQUE` = values must be unique.

sql
```
Users(
    NIF PK,
    first_name,
    last_name,
    phone,
    email
)

Doctors(
    NIF PK, FK -> Users(NIF),
    license_number UNIQUE,
    specialty
)

Patients(
    NIF PK, FK -> Users(NIF)
)

Medications(
    drug_code PK,
    trade_name,
    active_ingredient,
    stock_quantity
)

Consultations(
    consultation_id PK,
    date,
    time,
    status,
    fee,
    patient_NIF FK -> Patients(NIF),
    doctor_NIF FK -> Doctors(NIF)
)

Prescriptions(
    consultation_id PK, FK -> Consultations(consultation_id),
    drug_code PK, FK -> Medications(drug_code),
    dosage,
    duration_days,
    instructions
)

MedicalHistoryEntries(
    entry_id PK,
    entry_date,
    diagnosis_notes,
    patient_NIF FK -> Patients(NIF),
    doctor_NIF FK -> Doctors(NIF)
)
```

The `Prescriptions` table uses a composite primary key `(consultation_id, drug_code)`, assuming each medication appears at most once per consultation. If the same medication can be prescribed multiple times in one consultation, add a prescription-line ID or sequence number to the key.

### Why is the Prescriptions table necessary?

A consultation can prescribe many medications, and a medication can appear in many consultations. This is an M:N relationship.

The relational solution is a junction table:

`Prescriptions(consultation_id, drug_code, dosage, duration_days, instructions)`

It stores both foreign keys and the attributes belonging to the prescription relationship.

### Exam checklist

- Use EER inheritance for Doctor and Patient.
- Put one-to-many relationships' foreign keys on the many side.
- Resolve M:N relationships using a junction table.
- Store dosage and duration in `Prescriptions`, not directly in `Medications`.
- Link every history entry to both the patient and the doctor who created it.

# Problem 3 — E-Learning & Course Platform

## 3.1 Conceptual model (ER/EER)

### A. Entities and attributes

| Entity | Attributes | Primary key |
| --- | --- | --- |
| Center | center\_id, city, address, phone, director\_name | center\_id |
| User | user\_id, full\_name, email, registration\_date | user\_id |
| Instructor | user\_id, academic\_degree, hourly\_rate | user\_id |
| Student | user\_id, education\_level | user\_id |
| Course | course\_code, title, total\_hours, fee, category | course\_code |
| LiveClass | class\_id, date, start\_time, duration\_min, meeting\_url | class\_id |

EER specialization:

- `User` is the superclass.
- `Instructor` and `Student` are subclasses.
- Both inherit `user_id`, `full_name`, `email` and `registration_date`.
- Instructor-specific attributes: `academic_degree`, `hourly_rate`.
- Student-specific attribute: `education_level`.

Assume the intended specialization is disjoint (a user is either an instructor or a student). If users can have both roles, the specialization must instead allow overlap.

### B. Relationships and cardinalities

1. Center — Course (Offers)

- One center offers one or many courses (1..N).
- Each course belongs to exactly one center (1..1).
- Implementation: `center_id` as an FK in `Courses`.

2. Course — Course (Prerequisite)

- Recursive M:N relationship (0..N to 0..N).
- A course may require several other courses.
- A course may be a prerequisite for several other courses.
- Implementation: `CoursePrerequisites`.

3. Student — Course (Enrollment)

- A student may enroll in zero or many courses.
- A course may have zero or many students.
- This is an M:N relationship.
- Relationship attributes: enrollment\_date, payment\_status, final\_grade, review\_text.
- Implementation: `Enrollments`.

4. Instructor — LiveClass (Hosts)

- An instructor may host zero or many live classes (0..N).
- Each live class has exactly one instructor (1..1).

5. Course — LiveClass (Contains)

- A course may have zero or many live classes (0..N).
- Each live class belongs to exactly one course (1..1).

## 3.2 Logical model (relational schema)

sql
```
Centers(
    center_id PK,
    city,
    address,
    phone,
    director_name
)

Users(
    user_id PK,
    full_name,
    email,
    registration_date
)

Instructors(
    user_id PK, FK -> Users(user_id),
    academic_degree,
    hourly_rate
)

Students(
    user_id PK, FK -> Users(user_id),
    education_level
)

Courses(
    course_code PK,
    title,
    total_hours,
    fee,
    category,
    center_id FK -> Centers(center_id)
)

CoursePrerequisites(
    course_code PK, FK -> Courses(course_code),
    prerequisite_code PK, FK -> Courses(course_code)
)

Enrollments(
    student_id PK, FK -> Students(user_id),
    course_code PK, FK -> Courses(course_code),
    enrollment_date,
    payment_status,
    final_grade,
    review_text
)

LiveClasses(
    class_id PK,
    date,
    start_time,
    duration_min,
    meeting_url,
    instructor_id FK -> Instructors(user_id),
    course_code FK -> Courses(course_code)
)
```

### Important modeling decisions

1. Why does `Enrollments` have a composite PK?

The pair `(student_id, course_code)` uniquely identifies a student's enrollment in a course, assuming repeat enrollment is not allowed. If repeat enrollments are allowed, use an `enrollment_id` or include an attempt number.

2. Why does `CoursePrerequisites` have two foreign keys to Courses?

Both attributes reference the same table but have different roles:

- `course_code`: the course requiring a prerequisite.
- `prerequisite_code`: the course that must be completed first.

The composite PK prevents duplicate prerequisite relationships.

3. Where should review and grade information be stored?

In `Enrollments`, because the grade and review belong to a particular student's enrollment in a particular course, not to the student or course independently.

4. How is a live class connected?

Each `LiveClasses` row stores the instructor and course foreign keys, together with its scheduling and meeting information.

### Exam checklist

- Use superclass/subclass tables for shared and specialized user attributes.
- Represent M:N enrollment with a junction table.
- Represent recursive prerequisites with two FKs to the same table.
- Store relationship attributes in the corresponding junction table.
- Put `center_id` in `Courses` because each course belongs to one center.

# Problem 4 — Logistics & Freight Delivery Network

## 4.1 Conceptual model (ER/EER)

### A. Entities and attributes

| Entity | Attributes | Primary key |
| --- | --- | --- |
| Hub | hub\_code, city, country, capacity\_m3, manager\_name | hub\_code |
| Vehicle | vehicle\_id, acquisition\_date, max\_weight\_kg, status | vehicle\_id |
| Truck | vehicle\_id, max\_road\_distance\_km | vehicle\_id |
| CargoPlane | vehicle\_id, max\_altitude\_m, wingspan\_m | vehicle\_id |
| Customer | customer\_id, name, phone, billing\_address | customer\_id |
| Shipment | shipment\_id, order\_date, total\_price | shipment\_id |
| Package | tracking\_code, weight\_kg, volume\_m3, declared\_value, priority | tracking\_code |
| TransitLog | tracking\_code, hub\_code, arrival\_time, departure\_time, vehicle\_id | Composite key; see below |

EER specialization:

- `Vehicle` is the superclass.
- `Truck` and `CargoPlane` are subclasses.
- Common attributes belong in `Vehicle`.
- Truck-specific attribute: `max_road_distance_km`.
- Cargo-plane-specific attributes: `max_altitude_m`, `wingspan_m`.

Assume every vehicle is either a truck or cargo plane, and not both. This is a total, disjoint specialization. If the fleet includes other vehicle types, the specialization should be partial instead.

### B. Relationships and cardinalities

1. Hub — Vehicle (BasedAt)

- A hub can be the home hub of one or many vehicles (1..N, assuming every hub has a vehicle).
- Each vehicle has exactly one home hub (1..1).
- Implementation: `hub_code` in `Vehicles`.

2. Customer — Shipment (Places)

- A customer may place zero or many shipments (0..N).
- Each shipment belongs to exactly one customer (1..1).
- Implementation: `customer_id` in `Shipments`.

3. Shipment — Package (Contains)

- A shipment contains one or many packages (1..N).
- Each package belongs to exactly one shipment (1..1).
- Implementation: `shipment_id` in `Packages`.

4. Package — Hub — Vehicle (TransitLog)

- A package can have zero or many transit records over time.
- Each transit record refers to exactly one package, one hub and one vehicle.
- Attributes: arrival\_time, departure\_time.
- This records a package's transit history through the network.

## 4.2 Logical model (relational schema)

sql
```
Hubs(
    hub_code PK,
    city,
    country,
    capacity_m3,
    manager_name
)

Vehicles(
    vehicle_id PK,
    acquisition_date,
    max_weight_kg,
    status,
    vehicle_type,
    hub_code FK -> Hubs(hub_code)
)

Trucks(
    vehicle_id PK, FK -> Vehicles(vehicle_id),
    max_road_distance_km
)

CargoPlanes(
    vehicle_id PK, FK -> Vehicles(vehicle_id),
    max_altitude_m,
    wingspan_m
)

Customers(
    customer_id PK,
    name,
    phone,
    billing_address
)

Shipments(
    shipment_id PK,
    order_date,
    total_price,
    customer_id FK -> Customers(customer_id)
)

Packages(
    tracking_code PK,
    weight_kg,
    volume_m3,
    declared_value,
    priority,
    shipment_id FK -> Shipments(shipment_id)
)

TransitLogs(
    tracking_code PK, FK -> Packages(tracking_code),
    hub_code PK, FK -> Hubs(hub_code),
    arrival_time PK,
    departure_time,
    vehicle_id FK -> Vehicles(vehicle_id)
)
```

### Important modeling decisions

1. Why use separate `Trucks` and `CargoPlanes` tables?

The common vehicle attributes are stored once in `Vehicles`, while each subclass table contains only its specialized attributes. The subclass PK is also an FK referencing the superclass.

2. Why is `TransitLogs` a separate table?

A package can visit multiple hubs, and a hub can be visited by many packages. Each transit event also records a vehicle and timestamps. A separate table preserves the history rather than overwriting a package's current location.

3. Is the proposed transit-log key sufficient?

The composite key `(tracking_code, hub_code, arrival_time)` distinguishes repeated visits to the same hub when their arrival times differ. An alternative is to introduce `transit_id` as the PK, which is often simpler and more robust.

4. Why is `vehicle_id` a foreign key?

It ensures every recorded transport leg references an existing vehicle. If the actual transport-leg vehicle differs from the vehicle that handled the package's arrival at a hub, the precise meaning of the field should be clarified in the requirements.

### Exam checklist

- Keep shared vehicle attributes in `Vehicles`.
- Put subclass-specific attributes in `Trucks` and `CargoPlanes`.
- Use FKs to implement one-to-many relationships.
- Use a separate transit-log table to preserve multiple visits and timestamps.
- Choose a key that uniquely identifies each transit event.

# Final exam summary — memorize these patterns

## 1. Relationship-to-schema conversion rules

| Conceptual relationship | Relational implementation |
| --- | --- |
| 1:1 | FK in one table, usually with a `UNIQUE` constraint. |
| 1:N | FK on the N (many) side. |
| M:N | New junction table with FKs to both entities. |
| Recursive M:N | Junction table with two FKs to the same entity table. |
| Superclass/subclass | Superclass table plus subclass tables whose PKs are also FKs. |
| Relationship with attributes | Store the attributes in the relationship table, often the junction table. |
| Historical events | Separate event/log table with timestamps and references to participating entities. |

## 2. Primary keys and foreign keys

- PK: Uniquely identifies a record; cannot be `NULL`.
- FK: References a candidate or primary key in another table and enforces referential integrity.
- Composite PK: Uses multiple attributes together to identify a record.
- Junction table: Converts an M:N relationship into two 1:N relationships.
- Subclasses: Usually reuse the superclass key as both their PK and FK.

## 3. Common exam mistakes

- Putting an FK on the wrong side of a 1:N relationship.
- Representing M:N relationships without a junction table.
- Forgetting attributes that belong to relationships, such as dosage or enrollment date.
- Duplicating superclass attributes in subclass tables.
- Omitting FKs from the final relational schema.
- Assuming every entity must participate in at least one relationship.
- Using a composite key that fails to distinguish repeated historical events.
- Confusing the conceptual model (entities and relationships) with the logical model (tables, PKs and FKs).

Best exam strategy: For each problem, first list entities and attributes, underline the PKs, specify cardinalities and participation, identify M:N relationships and specialization, then translate the model into tables with PKs and FKs.
