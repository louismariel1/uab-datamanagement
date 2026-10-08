# Phase 4 — Database Normalization

Before applying normalization to our healthcare database, let's understand what **1NF, 2NF, and 3NF** mean. The easiest way to remember them is:

> **1NF:** One value per cell. **2NF:** Every non-key attribute depends on the whole key. **3NF:** Non-key attributes depend only on the key, not on other non-key attributes.

---

## 4.1 First Normal Form — 1NF

### Definition

A table is in **First Normal Form (1NF)** when:

1. Every column contains **atomic values** — one value per cell.
2. There are no repeating groups or lists inside a single field.
3. Each row can be uniquely identified.

### Simple example — not 1NF

Suppose we have:

| patient\_id | first\_name | allergies |
| --- | --- | --- |
| 1 | John | Penicillin, Peanuts |
| 2 | Maria | Latex |
| 3 | David | Penicillin, Latex |

The `allergies` column contains **multiple values in one cell**.

That's not a good 1NF design.

### 1NF solution

We could separate allergies into another table:

| allergy\_id | allergy\_name |
| --- | --- |
| 1 | Penicillin |
| 2 | Peanuts |
| 3 | Latex |

And create a relationship table:

| patient\_id | allergy\_id |
| --- | --- |
| 1 | 1 |
| 1 | 2 |
| 2 | 3 |
| 3 | 1 |
| 3 | 3 |

Now every cell contains one value.

### Important point for our practical

However, we shouldn't automatically create a `PATIENT_ALLERGY` table just because allergies _could_ contain multiple values.

The assignment specifically asks us to set:

```
allergies = 'NKA'
```

which strongly suggests that **the intended exercise treats****`allergies`****as one text attribute**, rather than a many-to-many allergy system.

Therefore, for this practical, we can keep `allergies` as a single attribute unless the original ERD indicates otherwise.

---

# 4.2 Second Normal Form — 2NF

### Definition

A table is in **Second Normal Form (2NF)** when:

1. It is already in 1NF.
2. Every non-key attribute depends on the **entire primary key**, not just part of it.

This is mainly an issue when the table has a **composite Primary Key**.

### Simple example

Suppose we have:

```
ENROLLMENT
--------------------------------
student_id       PK
course_id        PK
student_name
course_name
grade
```

The PK is:

```
(student_id, course_id)
```

But:

```
student_name → depends only on student_id
course_name  → depends only on course_id
grade        → depends on student_id + course_id
```

Therefore, the table is **not in 2NF**.

### 2NF solution

Separate the information:

```
STUDENT
----------------
student_id PK
student_name
```

```
COURSE
----------------
course_id PK
course_name
```

```
ENROLLMENT
----------------
student_id PK/FK
course_id PK/FK
grade
```

Now `grade` depends on the complete composite key:

```
(student_id, course_id)
```

---

# 4.3 Third Normal Form — 3NF

### Definition

A table is in **Third Normal Form (3NF)** when:

1. It is already in 2NF.
2. Non-key attributes do not depend on other non-key attributes.

In simple terms:

> Every non-key attribute should depend on **the key, the whole key, and nothing but the key**.

### Simple example

Suppose we have:

```
DOCTOR
--------------------------------
doctor_id       PK
doctor_name
specialty_id
specialty_name
```

We have:

```
doctor_id → specialty_id
specialty_id → specialty_name
```

Therefore:

```
doctor_id → specialty_id → specialty_name
```

`specialty_name` indirectly depends on `doctor_id` through `specialty_id`.

That's a **transitive dependency**, so this isn't 3NF.

### 3NF solution

Create a separate table:

```
DOCTOR
----------------------
doctor_id PK
doctor_name
specialty_id FK
```

and:

```
SPECIALTY
----------------------
specialty_id PK
specialty_name
```

Now:

```
DOCTOR.specialty_id
        ↓
SPECIALTY.specialty_id
```

The specialty name is stored only once.

---

# 4.4 Applying normalization to our healthcare database

Now let's apply those principles to the **actual tables from your practical**.

Our starting point is:

```
PATIENTS
    patient_id
    first_name
    last_name
    gender
    birth_date
    city
    province_id
    allergies
    weight

DOCTORS
    doctor_id
    first_name
    last_name
    specialty

ADMISSIONS
    patient_id
    admission_date
    discharge_date
    diagnosis
    attending_doctor_id

PROVINCE_NAMES
    province_id
    province_name
```

There are several things to examine.

---

# 4.5 Patients — normalization analysis

Current table:

```
PATIENTS
------------------------------------------------
patient_id
first_name
last_name
gender
birth_date
city
province_id
allergies
weight
```

Assuming `patient_id` uniquely identifies the patient:

```
patient_id → first_name
patient_id → last_name
patient_id → gender
patient_id → birth_date
patient_id → city
patient_id → province_id
patient_id → allergies
patient_id → weight
```

All these attributes depend directly on `patient_id`.

Therefore, there is no obvious partial dependency.

So the `PATIENTS` table can satisfy **2NF**.

### What about province?

We have:

```
patient_id → province_id
province_id → province_name
```

But notice that `province_name` isn't actually in `PATIENTS`.

Instead we already have:

```
PROVINCE_NAMES
-------------------------
province_id PK
province_name
```

This is good normalization.

Therefore:

```
PATIENTS.province_id
        ↓
PROVINCE_NAMES.province_id
```

avoids storing the province name repeatedly for every patient.

### Conclusion

Our `PATIENTS` design is already reasonably normalized, assuming the data types are corrected.

---

# 4.6 Province\_names — normalization analysis

Current:

```
PROVINCE_NAMES
-------------------------
province_id
province_name
```

If:

```
province_id → province_name
```

then `province_name` depends directly on the PK.

There is no obvious partial or transitive dependency.

Therefore, this table is already suitable for **3NF**.

---

# 4.7 Doctors — normalization analysis

Current:

```
DOCTORS
-------------------------
doctor_id
first_name
last_name
specialty
```

We need to ask:

> Is `specialty` simply a property of the doctor, or should specialties be represented as their own entity?

Suppose we have:

```
101 | John  | Smith | Cardiology
102 | Maria | Brown | Cardiology
103 | David | Jones | Pediatrics
104 | Sarah | White | Cardiology
```

The word `Cardiology` is repeated.

This isn't necessarily a **normalization violation** if `specialty` is simply an attribute of the doctor.

But if the database needs to maintain information about specialties, such as:

```
specialty_id
specialty_name
department
```

then a separate `SPECIALTIES` table is better.

For this practical, I recommend the normalized design:

```
SPECIALTIES
-------------------------
specialty_id PK
specialty_name
```

and:

```
DOCTORS
-------------------------
doctor_id PK
first_name
last_name
specialty_id FK
```

This gives:

```
SPECIALTIES 1 ───────── N DOCTORS
```

### Why?

Because specialty information is now stored once.

For example:

```
1 | Cardiology
2 | Pediatrics
3 | Neurology
```

Instead of repeatedly storing `"Cardiology"` in many doctor records.

This is a **design improvement toward 3NF**.

---

# 4.8 Admissions — the biggest normalization issue

The original table is:

```
ADMISSIONS
--------------------------------
patient_id
admission_date
discharge_date
diagnosis
attending_doctor_id
```

The problem is:

> **There is no clear Primary Key.**

Every table should have a reliable way to uniquely identify a row.

We could theoretically use:

```
(patient_id, admission_date)
```

as a composite PK.

But that's not ideal.

Imagine:

```
patient_id | admission_date
-----------|---------------
1          | 2026-10-01
1          | 2026-10-01
```

Can the same patient have two admissions beginning on the same date?

Potentially yes.

Therefore, `(patient_id, admission_date)` may not guarantee uniqueness.

### Recommended solution

Introduce:

```
admission_id
```

as a surrogate Primary Key.

The improved table becomes:

```
ADMISSIONS
------------------------------------------------
admission_id           PK
patient_id             FK
admission_date
discharge_date
diagnosis
attending_doctor_id    FK
```

Now every admission has its own unique identifier.

For example:

```
admission_id | patient_id | admission_date
-------------|------------|---------------
1001         | 1          | 2026-10-01
1002         | 1          | 2026-11-05
1003         | 2          | 2026-10-01
```

This is much safer.

---

# 4.9 Does Admissions violate 2NF?

With the new:

```
admission_id PK
```

the primary key is **not composite**.

Therefore, partial dependency isn't an issue.

Each attribute should depend directly on `admission_id`:

```
admission_id → patient_id
admission_id → admission_date
admission_id → discharge_date
admission_id → diagnosis
admission_id → attending_doctor_id
```

So this design satisfies the 2NF requirement.

---

# 4.10 Does Admissions violate 3NF?

We have:

```
admission_id → patient_id
admission_id → attending_doctor_id
```

The patient information is stored in `PATIENTS`.

The doctor information is stored in `DOCTORS`.

That's exactly what we want.

We **do not** put:

```
patient_first_name
patient_last_name
doctor_first_name
doctor_last_name
doctor_specialty
```

inside `ADMISSIONS`.

Instead we reference the appropriate entities using FKs.

Therefore, the improved `ADMISSIONS` table is suitable for 3NF.

---

# 4.11 Proposed normalized 3NF design

Based on the information you've supplied, I recommend the following final design:

### PATIENTS

```
PATIENTS
------------------------------------------------
PK  patient_id
    first_name
    last_name
    gender
    birth_date
    city
FK  province_id
    allergies
    weight
```

### PROVINCE\_NAMES

```
PROVINCE_NAMES
------------------------------------------------
PK  province_id
    province_name
```

### SPECIALTIES

```
SPECIALTIES
------------------------------------------------
PK  specialty_id
    specialty_name
```

### DOCTORS

```
DOCTORS
------------------------------------------------
PK  doctor_id
    first_name
    last_name
FK  specialty_id
```

### ADMISSIONS

```
ADMISSIONS
------------------------------------------------
PK  admission_id
FK  patient_id
    admission_date
    discharge_date
    diagnosis
FK  attending_doctor_id
```

---

# 4.12 Final relationships after normalization

The normalized design becomes:

```
                       PROVINCE_NAMES
                       ┌───────────────┐
                       │ PK province_id│
                       │ province_name │
                       └───────┬───────┘
                               │
                               │ 1
                               │
                               │ N
                               ▼
                         ┌────────────┐
                         │  PATIENTS  │
                         ├────────────┤
                         │PK patient_id
                         │ first_name │
                         │ last_name  │
                         │ gender     │
                         │ birth_date │
                         │ city       │
                         │FK province │
                         │ allergies  │
                         │ weight     │
                         └─────┬──────┘
                               │
                               │ 1
                               │
                               │ N
                               ▼
                         ┌──────────────┐
                         │  ADMISSIONS   │
                         ├──────────────┤
                         │PK admission_id│
                         │FK patient_id │
                         │ admission_date
                         │ discharge_date
                         │ diagnosis    │
                         │FK doctor_id  │
                         └───────▲──────┘
                                 │
                                 │ N
                                 │
                                 │ 1
                         ┌───────┴───────┐
                         │    DOCTORS    │
                         ├───────────────┤
                         │PK doctor_id   │
                         │ first_name    │
                         │ last_name     │
                         │FK specialty_id│
                         └───────┬───────┘
                                 │
                                 │ N
                                 │
                                 │ 1
                         ┌───────▼────────┐
                         │   SPECIALTIES  │
                         ├────────────────┤
                         │PK specialty_id │
                         │ specialty_name │
                         └────────────────┘
```

---

# 4.13 Summary of changes

This is the explanation I would use in your practical report:

| Original issue | Proposed solution | Normalization benefit |
| --- | --- | --- |
| `ADMISSIONS` has no clear PK | Add `admission_id` | Gives each admission a unique identity |
| Doctor specialty stored directly as text | Create `SPECIALTIES` table | Reduces repeated specialty data |
| `DOCTORS.specialty` | Replace with `specialty_id` FK | Establishes a normalized relationship |
| Province already separated | Keep `PROVINCE_NAMES` | Avoids repeating province names |
| Patient references province through `province_id` | Keep FK | Maintains referential integrity |
| Admission references patient | Keep `patient_id` FK | Represents Patient → Admission 1:N |
| Admission references doctor | Keep `attending_doctor_id` FK | Represents Doctor → Admission 1:N |

---

# ⚠️ Two issues to resolve before Phase 5

There are two inconsistencies in the original definitions that we should **not ignore** when we move to `CREATE TABLE`.

### 1\. `allergies DECIMAL(3,0)`

The assignment later says:

```
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;
```

Therefore, `allergies` clearly needs to support text.

I recommend changing it to something like:

```
allergies VARCHAR(100)
```

rather than `DECIMAL(3,0)`.

### 2\. `height` is missing

The obesity question explicitly asks for:

```
patient_id
weight
height
isObese
```

and says:

> Height is stored in centimeters.

But the supplied `PATIENTS` definition has no `height`.

Therefore, we should add:

```
height DECIMAL(...)
```

to `PATIENTS`.

These aren't really normalization changes; they're **schema/data-type corrections required by the later SQL questions**.

---

## Phase 4 conclusion

So our proposed **3NF solution** is:

```
PROVINCE_NAMES
        │
        │ 1:N
        ▼
    PATIENTS
        │
        │ 1:N
        ▼
   ADMISSIONS
        ▲
        │ N:1
        │
    DOCTORS
        │
        │ N:1
        ▼
   SPECIALTIES
```

And the key principle is:

> **Patient information belongs in PATIENTS, doctor information belongs in DOCTORS, province information belongs in PROVINCE\_NAMES, specialty information belongs in SPECIALTIES, and admission-specific information belongs in ADMISSIONS.**

That gives us a clean foundation for **Phase 5: writing the final****`CREATE TABLE`****statements and deciding the appropriate SQL data types and constraints**.
