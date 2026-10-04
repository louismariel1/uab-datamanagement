## 1\. Problem 1 — System Architecture & Scalability

 ### Comparison

 | Topic | My suggested approach | Lecturer's solution | Verdict |
| --- | --- | --- | --- |
| Initial response time | Service time + latency = **50 ms** | Service time + latency = **50 ms** | ✅ Same |
| Peak degradation | Explained using queueing/congestion | Explained using non-linear queueing delay | ✅ Same |
| Vertical scaling | Increase CPU/RAM | Increase CPU/RAM | ✅ Same |
| Horizontal scaling | Add server instances | Add server instances behind load balancer | ✅ Same |
| Diagonal scaling | Increase machine size + number of machines | Same | ✅ Same |
| Recommended scaling | Elastic horizontal scaling | Elastic horizontal scaling | ✅ Same |
| HA | Failover to secondary, possible brief interruption | Same | ✅ Same |
| Fault tolerance | Redundancy designed to avoid interruption/data loss | Synchronous redundancy, essentially zero downtime/loss | ⚠️ Lecturer's definition is stricter |

### Important exam point

 The calculation is:

 $$
R = S + L
$$

 where:

 - $R$ = response time
- $S$ = service time = 10 ms
- $L$ = latency = 40 ms

 Therefore:

 $$
R = 10 + 40 = \boxed{50\text{ ms}}
$$

 At peak:

 $$
\frac{450}{50}=9
$$

 So response time becomes **9 times larger**, while traffic becomes:

 $$
\frac{2500}{500}=5
$$

 or **5 times larger**.

 The key concept is **queueing delay**. As utilization approaches system capacity, waiting time can increase dramatically and non-linearly.

 ### One nuance about the lecturer's FT answer

 The lecturer says:

 > Fault tolerance ensures 0% downtime and zero lost transactions.

 This is useful as the **course/exam definition**, but in real-world systems, "100% uptime" is an idealized definition. For your exam, however, I would follow the lecturer's distinction:

 - **High Availability:** minimize downtime through failover.
- **Fault Tolerance:** continue operating essentially without interruption despite failure, usually through redundant/synchronous components.

 **Exam priority: follow the lecturer's terminology.**

---

 # 2\. Problem 2 — Healthcare & Telemedicine

 This is where the comparison is most useful because there are several valid ways to model the system.

 ## Conceptual model

 The lecturer uses:

```
User
 ├── Doctor
 └── Patient
```

 This is an **EER specialization/generalization**.

 That is a strong choice because Doctors and Patients share:

 - NIF
- name
- phone
- email

 while having different attributes.

 ### Lecturer's entities

 - **User**
- **Doctor**
- **Patient**
- **Consultation**
- **Medication**
- **MedicalHistoryEntry**

 This matches the approach I suggested.

 ### Relationships

 The lecturer has:

```
Patient 1 ─────── N Consultation
Doctor  1 ─────── N Consultation
Consultation N ── N Medication
Patient 1 ─────── N MedicalHistoryEntry
Doctor  1 ─────── N MedicalHistoryEntry
```

 This is exactly the structure I would recommend.

 ### Particularly important: Prescription

 The lecturer correctly models prescription information as **attributes of the relationship** between Consultation and Medication:

```
Consultation ─── Prescribes ─── Medication
                    |
             dosage
             duration_days
             instructions
```

 Why?

 Because dosage and duration don't belong permanently to the medication itself.

 For example:

 > Consultation #101 prescribes Aspirin at 500 mg for 7 days.

 Another consultation might prescribe the same medication at 250 mg for 10 days.

 Therefore:

```
Prescriptions(
    consultation_id,
    drug_code,
    dosage,
    duration_days,
    instructions
)
```

 is appropriate.

 ### Logical model

 The lecturer's schema is:

```
Users(
    NIF PK,
    first_name,
    last_name,
    phone,
    email
)

Doctors(
    NIF PK/FK,
    license_number,
    specialty
)

Patients(
    NIF PK/FK
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
    patient_NIF FK,
    doctor_NIF FK
)

Prescriptions(
    consultation_id PK/FK,
    drug_code PK/FK,
    dosage,
    duration_days,
    instructions
)

MedicalHistoryEntries(
    entry_id PK,
    entry_date,
    diagnosis_notes,
    patient_NIF FK,
    doctor_NIF FK
)
```

 ### Assessment

 **Lecturer's solution: excellent and exam-safe.**

 One small observation: the original problem only says each user has a "full name", whereas the lecturer splits this into `first_name` and `last_name`. That's a reasonable refinement, but if an exam strictly asks you to preserve the supplied attributes, `full_name` would also be acceptable.

---

 # 3\. Problem 3 — E-Learning Platform

 Again, my suggested structure and the lecturer's structure are fundamentally the same.

 ## EER specialization

 The lecturer models:

```
User
 ├── Instructor
 └── Student
```

 This is appropriate because both share:

 - user ID
- name
- email
- registration date

 while their specialized attributes differ.

 ### Instructor

```
academic_degree
hourly_rate
```

 ### Student

```
education_level
```

 This is exactly the type of thing to notice: the original problem mentions "certifications" **EER specialization** you should recognize in an exam.

---

 ## Course prerequisites

 One of the most important parts is:

```
Course ─── prerequisite ─── Course
```

 This is a **recursive relationship**.

 The cardinality is:

 $$
0..N \leftrightarrow 0..N
$$

 because:

 - a course can have zero or many prerequisites;
- a course can be a prerequisite for zero or many other courses.

 The logical implementation is therefore:

```
CoursePrerequisites(
    course_code PK/FK,
    prerequisite_code PK/FK
)
```

 This is exactly correct.

---

 ## Enrollment

 The lecturer identifies:

```
Student N ─── N Course
```

 with relationship attributes:

 - enrollment date
- payment status
- final grade
- review text

 Therefore:

```
Enrollments(
    student_id PK/FK,
    course_code PK/FK,
    enrollment_date,
    payment_status,
    final_grade,
    review_text
)
```

 This is an important exam pattern:

 > **M:N relationship \+ attributes → create an associative relation.**

---

 ## Live classes

 The lecturer models:

```
Instructor 1 ─── N LiveClass
Course     1 ─── N LiveClass
```

 and therefore:

```
LiveClasses(
    class_id PK,
    date,
    start_time,
    duration_min,
    meeting_url,
    instructor_id FK,
    course_code FK
)
```

 ### Assessment

 The lecturer's solution is **correct and consistent with standard ER-to-relational transformation**.

 One thing to notice: the original problem mentions "certifications" in the introduction but provides **no certification attributes or relationships** afterward. The lecturer correctly does not invent a Certification entity.

 That's a good exam lesson:

 > **Don't invent entities merely because they are mentioned in the general description if the requirements provide no information to model them.**

---

 # 4\. Problem 4 — Logistics & Freight

 This is probably the most conceptually interesting problem.

 ## Vehicle specialization

 The lecturer uses:

```
Vehicle
 ├── Truck
 └── CargoPlane
```

 This is appropriate EER specialization because all vehicles share:

 - vehicle ID
- acquisition date
- maximum weight
- status

 but subclasses have different attributes.

 ### Truck

```
max_road_distance_km
```

 ### CargoPlane

```
max_altitude_m
wingspan_m
```

 The relational transformation:

```
Vehicles(...)
Trucks(vehicle_id PK/FK, ...)
CargoPlanes(vehicle_id PK/FK, ...)
```

 is correct.

---

 # 5\. TransitLog — the most important modeling issue

 The lecturer identifies:

```
Package ─── Hub ─── Vehicle
```

 as a **ternary relationship**:

```
TransitLog(Package, Hub, Vehicle)
```

 with:

 - arrival time
- departure time

 This is a very good interpretation of the requirements.

 A package can pass through several hubs and use different vehicles during its journey.

 For example:

```
Package P001
   ↓
Hub A
   ↓ Truck 17
Hub B
   ↓ CargoPlane 4
Hub C
```

 The database needs to record **which package was at which hub and which vehicle transported it**.

 Hence:

```
TransitLogs(
    tracking_code,
    hub_code,
    arrival_time,
    departure_time,
    vehicle_id
)
```

 with foreign keys to all three entities.

---

 # 6\. A subtle issue with the lecturer's TransitLog primary key

 The lecturer proposes:

```
TransitLogs(
    tracking_code,
    hub_code,
    arrival_time,
    departure_time,
    vehicle_id
)
```

 with:

```
PK = (tracking_code, hub_code, arrival_time)
```

 This is reasonable because the same package could potentially visit the same hub more than once, and `arrival_time` distinguishes visits.

 However, **there are other valid designs**.

 For example, you could introduce:

```
transit_id PK
```

 and then:

```
TransitLogs(
    transit_id PK,
    tracking_code FK,
    hub_code FK,
    vehicle_id FK,
    arrival_time,
    departure_time
)
```

 That is often easier to manage.

 But for an exam based on this lecturer's material, I would use the lecturer's composite key unless the question explicitly asks you to propose an alternative.

---

 # 7\. Overall comparison

 Here is the most important summary:

 | Problem | My approach vs. lecturer | Main lesson |
| --- | --- | --- |
| **1** | Essentially identical | Performance, scaling, HA vs FT |
| **2** | Essentially identical | EER specialization + M:N relationship |
| **3** | Essentially identical | Recursive relationship + associative tables |
| **4** | Essentially identical | EER specialization + ternary relationship |

So there aren't major contradictions between the solutions. The lecturer's solutions mostly **confirm the modeling principles** we used.

---

 # 8\. What you should learn for the exam

 Based on all three lecture chunks and these official solutions, I'd focus particularly on these patterns.

 ### Pattern 1 — Superclass/subclass

 When entities share common attributes:

```
User
 ├── Doctor
 └── Patient
```

 or:

```
User
 ├── Instructor
 └── Student
```

 or:

```
Vehicle
 ├── Truck
 └── CargoPlane
```

 Use **EER specialization/generalization**.

 Relational implementation:

```
Superclass(PK, common attributes...)

Subclass(PK/FK, specialized attributes...)
```

---

 ### Pattern 2 — 1:N relationship

 Example:

```
Center 1 ───── N Course
```

 Put the FK on the **N side**:

```
Courses(
    course_code PK,
    ...,
    center_id FK
)
```

---

 ### Pattern 3 — M:N relationship

 Example:

```
Student N ───── N Course
```

 Create an associative table:

```
Enrollments(
    student_id PK/FK,
    course_code PK/FK,
    ...
)
```

---

 ### Pattern 4 — M:N relationship with attributes

 For example:

```
Consultation N ─── N Medication
```

 where the relationship has:

 - dosage
- duration
- instructions

 Create:

```
Prescriptions(
    consultation_id PK/FK,
    drug_code PK/FK,
    dosage,
    duration_days,
    instructions
)
```

 The **relationship attributes belong in the associative table**, not in either entity.

---

 ### Pattern 5 — Recursive relationship

 Example:

```
Course
  ↕
prerequisite
  ↕
Course
```

 Create:

```
CoursePrerequisites(
    course_code PK/FK,
    prerequisite_code PK/FK
)
```

---

 ### Pattern 6 — Ternary relationship

 When an event depends simultaneously on three entities:

```
Package + Hub + Vehicle
```

 create a relation such as:

```
TransitLogs(
    tracking_code FK,
    hub_code FK,
    vehicle_id FK,
    arrival_time,
    departure_time
)
```

---

 # 9\. The biggest exam traps

 These are the mistakes I'd be most careful about:

 1. **Putting an M:N relationship directly into one of the entity tables.**\
    → Create a junction/associative table.
2. **Putting relationship attributes into an entity.**\
    → If dosage belongs to a particular Consultation–Medication association, put it in `Prescriptions`.
3. **Forgetting that a recursive relationship references the same table twice.**
4. **Forgetting FK constraints when converting ER/EER to relational schema.**
5. **Confusing PK and FK.**
    Example:

   ```
   Doctors(NIF PK/FK)
   ```
    `NIF` is simultaneously:
   - the Doctor's primary key;
   - a foreign key referencing `Users`.
6. **Missing specialization.**
    If Doctor and Patient share User attributes, don't unnecessarily duplicate:

   ```
   Doctor(NIF, name, phone, email...)
   Patient(NIF, name, phone, email...)
   ```
    The lecturer expects:

   ```
   Users(...)
   Doctors(NIF FK...)
   Patients(NIF FK...)
   ```
7. **Confusing HA and fault tolerance.**
    For this course:
    **HA → failover/minimize downtime**
    **FT → continue despite failure with essentially no interruption/data loss**
8. **Confusing latency with response time.**
    Remember:
    $$
   \boxed{\text{Response Time} = \text{Service Time} + \text{Latency}}
   $$
9. **Forgetting queueing effects under heavy load.**
    A 5× increase in workload does **not** necessarily produce only a 5× increase in response time.

---

 ## Bottom line

 **Your lecturer's solutions are consistent with the solutions I would recommend.** For exam preparation, I would treat the lecturer's terminology and cardinalities as the **reference answer**, especially for:

 - `User → Doctor/Patient`
- `User → Instructor/Student`
- `Vehicle → Truck/CargoPlane`
- M:N → associative relation
- recursive Course prerequisite
- ternary Package–Hub–Vehicle TransitLog
- PK/FK inheritance
- Elastic Horizontal Scaling
- HA vs Fault Tolerance
- Response Time = Service Time + Latency

 The strongest strategy is not to memorize the four solutions individually. **Memorize the ER/EER → relational transformation patterns**, because the exam can change the scenario while testing exactly the same underlying concepts.
