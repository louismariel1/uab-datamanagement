Here is a condensed, exam-focused revision sheet covering all the supplied material: system principles and performance metrics, scalability, reliability, ER/EER conceptual modeling, cardinalities, specialization, and relational schema design. It includes the worked calculation and model patterns you can reuse in exam questions.

Writing

# EXAM COMPRESSION — DATA MANAGEMENT

**Topics: System Principles, ER/EER Conceptual Modeling & Relational Schema**

## 1\. System Architecture & Performance Metrics

### 1.1 Essential performance metrics

| Metric | Definition |
| --- | --- |
| Request throughput | Number of requests processed per second (RPS) |
| Latency | Time associated with communication and waiting, depending on the metric definition |
| Service time | Time the system spends processing a request |
| Response time | Total time experienced from request submission to response |
| Queueing delay | Time a request waits before being processed |

**Simplified exam formula:**

Response time = Latency + Service time

### 1.2 Worked example: Flash sale

Given:

- Normal throughput = 500 requests/second
- Latency = 40 ms
- Service time = 10 ms
- Peak throughput = 2,500 requests/second
- Peak response time = 450 ms

**Initial response time:**

Response time = 40 + 10 = **50 ms**

**Performance degradation:**

- Traffic increases by $2500/500 = 5$ times.
- Response time increases by $450/50 = 9$ times.
- The response-time increase is 400 ms, or 800%.

**Why does performance degrade disproportionately?**

When the incoming request rate approaches or exceeds system capacity, requests accumulate in queues. Queueing delay increases non-linearly, so a relatively moderate increase in traffic can cause a much larger increase in response time.

**Exam takeaway:** Response time is not determined by service time alone. Under heavy load, queueing delay can dominate.

Note: In real systems, latency definitions vary. If latency already includes queueing, avoid counting queueing twice.

## 2\. Scalability

Scalability is a system's ability to handle increased workload by adding resources while maintaining acceptable performance.

### 2.1 Scaling strategies

| Strategy | Meaning | Advantages | Limitations |
| --- | --- | --- | --- |
| Vertical scaling (scale up) | Add CPU, RAM or other resources to an existing server | Simple; may require fewer architectural changes | Hardware limits; possible downtime; expensive upgrades |
| Horizontal scaling (scale out) | Add more server instances | Distributes workload; supports traffic spikes | Requires load balancing and coordination |
| Diagonal scaling | Increase individual server capacity and add more instances | Combines both approaches | More complex and potentially costly |

### 2.2 Static vs. elastic scaling

- **Static scaling:** Resources are provisioned in advance and remain relatively fixed.
- **Elastic scaling:** Resources are automatically added or removed as demand changes.

**Flash-sale recommendation: Elastic horizontal scaling.**

Why?

1. Flash-sale traffic is high and temporary.
2. Additional instances distribute requests across multiple servers.
3. Automatic scaling responds to demand or utilization thresholds.
4. Resources can be removed after the peak to reduce costs.

Diagonal scaling may also be appropriate if individual instances need more capacity as well as additional replicas.

## 3\. Reliability, Fault Tolerance & High Availability

### 3.1 Definitions

| Concept | Meaning |
| --- | --- |
| Reliability | Probability that a system operates correctly over a specified period |
| Availability | Proportion of time a system is operational and accessible |
| High Availability (HA) | Design that minimizes downtime and restores service quickly after failures |
| Fault Tolerance (FT) | Ability to continue operating correctly despite specified component failures |
| Failover | Switching service from a failed component to a functioning replacement |
| Replication | Maintaining copies of data on multiple database nodes |

### 3.2 Primary-secondary replication

- **Primary:** Usually handles writes and coordinates replication.
- **Secondary/replica:** Maintains a copy of the primary's data and may serve eligible reads.
- **Failover:** A secondary is promoted when the primary fails.

### 3.3 Primary-node crash: HA vs. FT

**High Availability:**

- Detects the failure and promotes a secondary.
- A brief interruption may occur during detection and failover.
- Some committed data may be lost if replication is asynchronous and the replica is behind.

**Fault Tolerance:**

- Aims to continue service without interruption when a supported failure occurs.
- Requires sufficient redundancy and coordinated recovery mechanisms.
- Strong synchronous replication can help prevent acknowledged transactions from being lost, but synchronous replication alone does not guarantee zero downtime.

**Exam distinction:** HA minimizes downtime; FT aims to maintain correct operation despite failures.

Avoid treating 100% uptime or zero data loss as automatic guarantees. These depend on the architecture and failure model.

## 4\. Conceptual Modeling: ER/EER

### 4.1 ER model

An Entity–Relationship (ER) model represents entities, attributes, keys and relationships before the database is implemented.

- **Entity:** A distinguishable real-world object, such as a Patient or Course.
- **Attribute:** A property, such as email or date.
- **Key:** Attribute(s) that identify an entity instance uniquely.
- **Relationship:** An association between entities.
- **Cardinality:** How many instances may participate in a relationship.
- **Participation/optionality:** Whether participation is mandatory or optional.

### 4.2 Cardinalities

| Notation | Meaning | Example |
| --- | --- | --- |
| 1:1 | One instance relates to at most one instance on the other side | A hypothetical one-to-one assignment |
| 1:N | One instance can relate to many instances | One doctor conducts many consultations |
| M:N | Many instances relate to many instances | Students enroll in courses |
| 0..N | Zero or many | A patient may have no consultations yet |
| 1..1 | Exactly one | Every consultation has one patient |

**How to identify cardinality:**

Ask two questions independently:

1. For one instance of A, how many instances of B are allowed?
2. For one instance of B, how many instances of A are allowed?

Do not confuse maximum cardinality with minimum participation.

### 4.3 EER: Specialization and generalization

EER extends ER modeling with concepts such as superclasses and subclasses.

- **Superclass:** Stores common attributes.
- **Subclass:** Inherits the superclass's attributes and adds specialized attributes.
- **Specialization:** Divides a general entity into more specific subtypes.
- **Generalization:** Combines similar entity types into a common superclass.

Examples:

- `User` → `Doctor`, `Patient`
- `User` → `Instructor`, `Student`
- `Vehicle` → `Truck`, `CargoPlane`

When modeling specialization, consider:

- **Disjoint:** An instance can belong to only one subtype.
- **Overlapping:** An instance can belong to multiple subtypes.
- **Total:** Every superclass instance must belong to a subtype.
- **Partial:** Some superclass instances may belong to no subtype.

The scenario determines whether these constraints are appropriate.

## 5\. Mapping ER/EER to a Relational Schema

A relational schema defines tables, columns, primary keys and foreign keys.

### 5.1 Primary and foreign keys

- **PK:** Uniquely identifies a row.
- **FK:** References a key in another table.
- **Composite PK:** Uses multiple columns to identify a row.
- **Referential integrity:** Prevents invalid references between related tables.

### 5.2 Standard mapping rules

**Rule 1: Strong entity → table**

Create one table for each entity, with its attributes and primary key.

Example:

`Patients(NIF PK, first_name, last_name, phone, email)`

**Rule 2: One-to-many relationship → FK on the many side**

Example: One doctor conducts many consultations.

`Consultations(consultation_id PK, doctor_NIF FK, ...)`

**Rule 3: Many-to-many relationship → junction table**

Example: Students enroll in courses.

`Enrollments(student_id PK/FK, course_code PK/FK, enrollment_date, ...)`

The two foreign keys form a composite primary key if each student can enroll in a given course only once.

**Rule 4: Relationship attributes → relationship table**

If a relationship has its own attributes, store them in the junction table.

Example: `dosage` belongs to a particular consultation-medication prescription, not to the medication itself.

**Rule 5: Superclass/subclass → shared table plus subtype tables**

Example:

- `Users(user_id PK, full_name, email, registration_date)`
- `Students(user_id PK/FK, education_level)`
- `Instructors(user_id PK/FK, academic_degree, hourly_rate)`

Each subtype uses the superclass key as both its primary key and a foreign key to the superclass.

**Rule 6: Recursive relationship → self-referencing FK or junction table**

For course prerequisites, one course can require several other courses, and a course can be a prerequisite for several courses. Use a junction table with two foreign keys to `Courses`.

**Rule 7: Ternary relationship → separate table**

When a relationship involves three entities and records their combined participation, create a relationship table containing the foreign keys of all three entities plus the relationship attributes.

## 6\. Problem 2 — Healthcare & Telemedicine

### 6.1 Conceptual model

**Entities and attributes**

- **User:** NIF (PK), first\_name, last\_name, phone, email
- **Doctor (subclass of User):** license\_number (unique), specialty
- **Patient (subclass of User):** inherits User attributes
- **Consultation:** consultation\_id (PK), date, time, status, fee
- **Medication:** drug\_code (PK), trade\_name, active\_ingredient, stock\_quantity
- **MedicalHistoryEntry:** entry\_id (PK), entry\_date, diagnosis\_notes

**Relationships and cardinalities**

- Patient — Consultation: Patient `0..N`; Consultation `1..1`.
- Doctor — Consultation: Doctor `0..N`; Consultation `1..1`.
- Consultation — Medication: M:N, with `dosage`, `duration_days`, `instructions` on the relationship.
- Patient — MedicalHistoryEntry: Patient `0..N`; each entry belongs to `1..1` patient.
- Doctor — MedicalHistoryEntry: Doctor `0..N`; each entry is created by `1..1` doctor.

### 6.2 Logical model

```
Users(
    NIF PK,
    first_name,
    last_name,
    phone,
    email
)

Doctors(
    NIF PK/FK -> Users.NIF,
    license_number UNIQUE,
    specialty
)

Patients(
    NIF PK/FK -> Users.NIF
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
    patient_NIF FK -> Patients.NIF,
    doctor_NIF FK -> Doctors.NIF
)

Prescriptions(
    consultation_id PK/FK -> Consultations.consultation_id,
    drug_code PK/FK -> Medications.drug_code,
    dosage,
    duration_days,
    instructions
)

MedicalHistoryEntries(
    entry_id PK,
    entry_date,
    diagnosis_notes,
    patient_NIF FK -> Patients.NIF,
    doctor_NIF FK -> Doctors.NIF
)
```

**Important:** The composite key `(consultation_id, drug_code)` allows each medication to appear at most once per consultation. If the same medication can be prescribed multiple times in one consultation, add a prescription-line ID or sequence number to the key.

## 7\. Problem 3 — E-Learning & Course Platform

### 7.1 Conceptual model

**Entities and attributes**

- **Center:** center\_id (PK), city, address, phone, director\_name
- **User:** user\_id (PK), full\_name, email, registration\_date
- **Instructor (subclass):** academic\_degree, hourly\_rate
- **Student (subclass):** education\_level
- **Course:** course\_code (PK), title, total\_hours, fee, category
- **LiveClass:** class\_id (PK), date, start\_time, duration\_min, meeting\_url

**Relationships and cardinalities**

- Center — Course: Center `0..N` courses; each course belongs to exactly one primary center.
- Course — Course prerequisite: recursive M:N, optional on both sides.
- Student — Course: M:N through Enrollment.
- Enrollment attributes: enrollment\_date, payment\_status, final\_grade, review\_text and, if required, rating.
- Instructor — LiveClass: Instructor `0..N`; each class has exactly one instructor.
- Course — LiveClass: Course `0..N`; each class belongs to exactly one course.

### 7.2 Logical model

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
    user_id PK/FK -> Users.user_id,
    academic_degree,
    hourly_rate
)

Students(
    user_id PK/FK -> Users.user_id,
    education_level
)

Courses(
    course_code PK,
    title,
    total_hours,
    fee,
    category,
    center_id FK -> Centers.center_id
)

CoursePrerequisites(
    course_code PK/FK -> Courses.course_code,
    prerequisite_code PK/FK -> Courses.course_code
)

Enrollments(
    student_id PK/FK -> Students.user_id,
    course_code PK/FK -> Courses.course_code,
    enrollment_date,
    payment_status,
    final_grade,
    review_text,
    rating
)

LiveClasses(
    class_id PK,
    date,
    start_time,
    duration_min,
    meeting_url,
    instructor_id FK -> Instructors.user_id,
    course_code FK -> Courses.course_code
)
```

**Exam notes:**

- `CoursePrerequisites` resolves the recursive M:N relationship.
- `Enrollments` resolves the Student–Course M:N relationship and stores relationship attributes.
- The enrollment composite key assumes one enrollment per student per course.
- If multiple attempts or re-enrollments are allowed, use a separate `enrollment_id` or include an attempt identifier.
- The brief does not establish that a center director must be a registered user, so `director_name` is modeled as an attribute.

## 8\. Problem 4 — Logistics & Freight Delivery Network

### 8.1 Conceptual model

**Entities and attributes**

- **Hub:** hub\_code (PK), city, country, capacity\_m3, manager\_name
- **Vehicle (superclass):** vehicle\_id (PK), acquisition\_date, max\_weight\_kg, status
- **Truck (subclass):** max\_road\_distance\_km
- **CargoPlane (subclass):** max\_altitude\_m, wingspan\_m
- **Customer:** customer\_id (PK), name, phone, billing\_address
- **Shipment:** shipment\_id (PK), order\_date, total\_price
- **Package:** tracking\_code (PK), weight\_kg, volume\_m3, declared\_value, priority

**Relationships and cardinalities**

- Hub — Vehicle: Hub `0..N`; each vehicle has exactly one home hub.
- Customer — Shipment: Customer `0..N`; each shipment is placed by exactly one customer.
- Shipment — Package: Shipment `1..N` packages; each package belongs to exactly one shipment.
- Package — Hub — Vehicle: ternary transit relationship recording package movement through hubs using vehicles.

Transit attributes:

- arrival\_time
- departure\_time

### 8.2 Logical model

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
    hub_code FK -> Hubs.hub_code
)

Trucks(
    vehicle_id PK/FK -> Vehicles.vehicle_id,
    max_road_distance_km
)

CargoPlanes(
    vehicle_id PK/FK -> Vehicles.vehicle_id,
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
    customer_id FK -> Customers.customer_id
)

Packages(
    tracking_code PK,
    weight_kg,
    volume_m3,
    declared_value,
    priority,
    shipment_id FK -> Shipments.shipment_id
)

TransitLogs(
    tracking_code PK/FK -> Packages.tracking_code,
    hub_code PK/FK -> Hubs.hub_code,
    arrival_time PK,
    departure_time,
    vehicle_id FK -> Vehicles.vehicle_id
)
```

**Important design refinement:** The primary key `(tracking_code, hub_code, arrival_time)` allows a package to visit the same hub multiple times, provided each visit has a distinct arrival time. An alternative is a generated `transit_id` primary key.

**Modeling assumption:** Each transit-log row records a package's visit to a hub and the vehicle associated with that leg. If the system must represent both the departure hub and destination hub for every transport leg, use explicit `origin_hub_code` and `destination_hub_code` foreign keys instead.

### 8.3 Vehicle specialization

`Vehicles` stores attributes common to trucks and cargo planes. The subtype tables store only their specific attributes.

For a disjoint, total specialization:

- Every vehicle must be either a Truck or CargoPlane.
- A vehicle cannot be both.

These constraints should be enforced if the business rules require them; they are not guaranteed by the basic table structure alone.

## 9\. High-Priority Exam Traps

1. **Conceptual vs. logical model:** ER/EER describes entities and relationships; relational schema defines tables, PKs and FKs.
2. **1:N relationship:** Usually place the FK on the N side.
3. **M:N relationship:** Create a junction table.
4. **Relationship attributes:** Store them in the junction table, not arbitrarily in either entity.
5. **Specialization:** Subtype tables inherit the superclass key as their PK and FK.
6. **Recursive M:N:** Use a junction table containing two FKs to the same table.
7. **Ternary relationship:** Preserve all participating entities using a separate relationship table.
8. **Optionality:** `0..N` means participation may be absent; `1..1` means exactly one.
9. **Unique identifiers:** A business identifier such as NIF or a drug code can be a PK; a generated ID is another valid design if uniqueness is enforced.
10. **Composite keys:** Choose them carefully based on whether duplicate relationships or repeated events are allowed.
11. **Foreign keys:** Reference a valid key in the referenced table.
12. **Scaling:** Horizontal adds instances; vertical upgrades a machine; diagonal combines both.
13. **Elasticity:** Adds and removes resources as demand changes.
14. **HA vs. FT:** HA minimizes downtime; FT continues operating through specified failures.
15. **Performance:** High request volume can cause non-linear queueing delays.

## 10\. Exam Method: How to Solve Any ER-to-Relational Question

Use this order under exam conditions:

1. **Identify entities** from important nouns in the scenario.
2. **List attributes** for every entity.
3. **Choose primary keys** that uniquely identify instances.
4. **Identify superclasses and subclasses** where entities share common attributes.
5. **Determine relationship cardinalities** by asking how many instances can participate on each side.
6. **Place relationship attributes** on the relationship they describe.
7. **Create one table per strong entity** and map specialization consistently.
8. **Add foreign keys** for 1:N relationships.
9. **Create junction tables** for M:N and recursive M:N relationships.
10. **Create relationship tables** for ternary relationships.
11. **Check optionality, uniqueness and referential integrity.**
12. **State any assumptions** about repeated enrollments, prescriptions, vehicle types or transit events.

## Final 60-Second Revision

- Response time = latency + service time under the simplified problem definition.
- Heavy traffic can increase queueing delay non-linearly.
- Vertical = scale up; horizontal = scale out; diagonal = both.
- Elastic scaling adapts resources to changing demand.
- HA minimizes downtime; FT aims to maintain correct service despite failures.
- ER = entities, attributes, relationships and cardinalities.
- EER = ER plus specialization/generalization.
- 1:N → FK on the many side.
- M:N → junction table.
- Relationship attributes → junction/relationship table.
- Subclass → superclass key as PK/FK.
- Recursive M:N → two FKs referencing the same entity table.
- Ternary relationship → separate table containing all participating FKs.
- Always identify PKs, FKs, minimum participation and maximum cardinality before finalizing the schema.
