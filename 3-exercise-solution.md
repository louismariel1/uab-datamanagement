# Exercise 3 - Solution Outliine

- **Phase 1:** What the original database contains.
- **Phase 2:** How the original tables are related.
- **Phase 3:** Add PKs/FKs and identify design problems.

# Phase 1 — Current Database Structure

Based strictly on your definitions, the original database has these four tables.

## 1\. PATIENTS

Current definition:

```
PATIENTS
--------------------------------------------------
Patient_id       INTEGER
first_name       VARCHAR(30)
last_name        VARCHAR(30)
gender           CHAR(1)
birth_date       DATE
city             VARCHAR(30)
province_id      CHAR(2)
allergies        DECIMAL(3,0)
weight           DECIMAL(4,0)
```

### Interpretation

`Patient_id` should uniquely identify each patient.

`province_id` appears to identify the province where the patient lives, because there is a separate `Province_names` table containing the province information.

Therefore:

```
patient_id  → Primary Key
province_id → Foreign Key
```

---

## 2\. DOCTORS

Current definition:

```
DOCTORS
--------------------------------------------------
doctor_id       INTEGER
first_name      VARCHAR(30)
last_name       VARCHAR(30)
specialty       VARCHAR(25)
```

### Interpretation

`doctor_id` should uniquely identify each doctor.

Therefore:

```
doctor_id → Primary Key
```

At this stage, `specialty` is simply an attribute of the doctor.

This is important because **we should not automatically create a SPECIALTY table yet**. Your current definition doesn't show one, so we first treat `specialty` as a regular attribute and later decide whether normalization requires separating it.

---

## 3\. ADMISSIONS

Current definition:

```
ADMISSIONS
--------------------------------------------------
patient_id             INTEGER
admission_date         DATE
discharge_date         DATE
diagnosis              VARCHAR(50)
attending_doctor_id    INTEGER
```

This table has an important issue:

> There is currently no obvious single `admission_id`.

We therefore need to determine what uniquely identifies an admission.

A likely candidate is:

```
(patient_id, admission_date)
```

because a patient could have multiple admissions, but this assumes that a patient cannot have two admissions beginning on the same date.

Alternatively, the better design would introduce:

```
admission_id
```

as a surrogate Primary Key.

We'll discuss this under normalization.

The obvious foreign keys are:

```
patient_id
attending_doctor_id
```

with:

```
ADMISSIONS.patient_id
        ↓
PATIENTS.patient_id
```

and:

```
ADMISSIONS.attending_doctor_id
        ↓
DOCTORS.doctor_id
```

---

## 4\. PROVINCE\_NAMES

Current definition:

```
PROVINCE_NAMES
--------------------------------------------------
province_id       CHAR(2)
province_name     VARCHAR(30)
```

`province_id` should uniquely identify each province.

Therefore:

```
province_id → Primary Key
```

And because `PATIENTS` contains `province_id`, we have:

```
PATIENTS.province_id
        ↓
PROVINCE_NAMES.province_id
```

So `PATIENTS.province_id` is a Foreign Key.

---

# Phase 2 — Relationships

Now we can identify the relationships more confidently.

## Relationship 1: Province → Patients

One province can have many patients.

```
PROVINCE_NAMES 1 ───────────< PATIENTS
```

Therefore:

```
PROVINCE_NAMES
     1
     │
     │
     N
  PATIENTS
```

The FK is:

```
PATIENTS.province_id
```

referencing:

```
PROVINCE_NAMES.province_id
```

---

## Relationship 2: Patient → Admissions

One patient can have multiple admissions.

```
PATIENTS 1 ───────────< ADMISSIONS
```

The FK is:

```
ADMISSIONS.patient_id
```

referencing:

```
PATIENTS.patient_id
```

---

## Relationship 3: Doctor → Admissions

One doctor can attend multiple admissions.

```
DOCTORS 1 ───────────< ADMISSIONS
```

The FK is:

```
ADMISSIONS.attending_doctor_id
```

referencing:

```
DOCTORS.doctor_id
```

---

# Phase 3 — Add PKs and FKs

Now let's produce the table you asked for, with the missing keys filled in.

## Final Phase 3 key structure

| Table | Column | Key | References |
| --- | --- | --- | --- |
| **PATIENTS** | `patient_id` | **PK** | — |
|  | `first_name` | — | — |
|  | `last_name` | — | — |
|  | `gender` | — | — |
|  | `birth_date` | — | — |
|  | `city` | — | — |
|  | `province_id` | **FK** | `PROVINCE_NAMES.province_id` |
|  | `allergies` | — | — |
|  | `weight` | — | — |
| **DOCTORS** | `doctor_id` | **PK** | — |
|  | `first_name` | — | — |
|  | `last_name` | — | — |
|  | `specialty` | — | — |
| **ADMISSIONS** | `patient_id` | **FK** | `PATIENTS.patient_id` |
|  | `admission_date` | **PK candidate** | — |
|  | `discharge_date` | — | — |
|  | `diagnosis` | — | — |
|  | `attending_doctor_id` | **FK** | `DOCTORS.doctor_id` |
| **PROVINCE\_NAMES** | `province_id` | **PK** | — |
|  | `province_name` | — | — |

### Important issue with ADMISSIONS

I would **not yet mark****`admission_date`****as a PK by itself**.

The current definition gives us:

```
(patient_id, admission_date, discharge_date, diagnosis, attending_doctor_id)
```

but no `admission_id`.

A reasonable original-design interpretation could be:

```
PK = (patient_id, admission_date)
```

However, this has an assumption:

> A patient cannot have two admissions beginning on the same date.

That's not necessarily guaranteed.

For a robust database design, I'd recommend adding:

```
admission_id INTEGER PRIMARY KEY
```

to `ADMISSIONS`.

That would give us:

```
ADMISSIONS
--------------------------------------------------
admission_id          PK
patient_id            FK → PATIENTS.patient_id
admission_date
discharge_date
diagnosis
attending_doctor_id   FK → DOCTORS.doctor_id
```

This is likely the cleaner solution for your final 3NF design.

---

# Updated ERD — Current Design

Based on your definitions, the relationships are:

```
                         ┌─────────────────────────┐
                         │     PROVINCE_NAMES      │
                         ├─────────────────────────┤
                         │ PK province_id          │
                         │    province_name        │
                         └────────────┬────────────┘
                                      │
                                      │ 1
                                      │
                                      │ N
                         ┌────────────▼────────────┐
                         │        PATIENTS         │
                         ├─────────────────────────┤
                         │ PK patient_id           │
                         │    first_name           │
                         │    last_name            │
                         │    gender               │
                         │    birth_date           │
                         │    city                 │
                         │ FK province_id          │
                         │    allergies            │
                         │    weight               │
                         └────────────┬────────────┘
                                      │
                                      │ 1
                                      │
                                      │ N
                         ┌────────────▼────────────┐
                         │       ADMISSIONS        │
                         ├─────────────────────────┤
                         │ PK admission_id*        │
                         │ FK patient_id           │
                         │    admission_date      │
                         │    discharge_date      │
                         │    diagnosis             │
                         │ FK attending_doctor_id │
                         └────────────▲────────────┘
                                      │
                                      │ N
                                      │
                                      │ 1
                         ┌────────────┴────────────┐
                         │         DOCTORS         │
                         ├─────────────────────────┤
                         │ PK doctor_id            │
                         │    first_name           │
                         │    last_name            │
                         │    specialty            │
                         └─────────────────────────┘

* Recommended addition for the improved design
```

So the cardinalities are:

```
PROVINCE_NAMES  1 ───── N  PATIENTS

PATIENTS        1 ───── N  ADMISSIONS

DOCTORS         1 ───── N  ADMISSIONS
```

---

# One correction from our previous Phase 1–3

With the definitions you've now supplied, **we should remove****`SPECIALTY`****from our current Phase 1–3 model**.

Your original table explicitly says:

```
DOCTORS:
doctor_id
first_name
last_name
specialty
```

There is no `specialty_id` and no `SPECIALTY` table.

So for the **original/current ERD**, we should represent:

```
DOCTORS
---------------------
PK doctor_id
first_name
last_name
specialty
```

We can consider whether `specialty` should become a separate table during **Phase 2 normalization / Phase 3 improved 3NF design**.

---

# Potential normalization issues we'll investigate next

Now that we have the real definitions, there are several things worth investigating in the next phase.

### 1\. `ADMISSIONS` has no clear identifier

This is the biggest structural issue.

We should probably introduce:

```
admission_id PK
```

rather than relying on `(patient_id, admission_date)`.

### 2\. `specialty` may be redundant

Currently:

```
DOCTORS
doctor_id | first_name | last_name | specialty
```

If 20 doctors are cardiologists, `"Cardiology"` gets stored 20 times.

We need to decide whether:

```
SPECIALTIES
specialty_id
specialty_name
```

should be introduced.

### 3\. `province_id` is already reasonably normalized

This is actually a good example of normalization:

```
PATIENTS
province_id FK
       ↓
PROVINCE_NAMES
province_id PK
province_name
```

Instead of storing:

```
province_name = "Barcelona"
```

repeatedly for every patient, the province name is stored once.

### 4\. `allergies` needs investigation

Your definition says:

```
allergies DECIMAL(3,0)
```

That looks unusual.

The practical later asks:

> patients whose allergies field is NULL

and:

> change NULL allergies to `'NKA'`

But a `DECIMAL(3,0)` column cannot appropriately store the string `'NKA'`.

So there is almost certainly a **data-type inconsistency in the supplied definition**.

It would make much more sense for `allergies` to be something like:

```
allergies VARCHAR(...)
```

if it stores values such as:

```
Penicillin
Peanuts
NKA
```

We'll need to resolve this before writing the final `CREATE TABLE`.

### 5\. `weight` also needs checking

You gave:

```
weight DECIMAL(4,0)
```

This can represent whole-number weights, but the obesity calculation in the assignment suggests weight is a numerical measurement.

A more precise definition might be:

```
DECIMAL(5,2)
```

depending on the expected data.

Also, the original assignment mentions **height**, but your current `PATIENTS` definition doesn't contain a `height` column. That's another important inconsistency we should resolve before Phase 4.

---

## Where we are now

We can consider **Phases 1–3 provisionally complete**:

```
Phase 1
✓ Identified 4 current tables
✓ Identified attributes

Phase 2
✓ Identified relationships
✓ Identified cardinalities

Phase 3
✓ Added PKs
✓ Added FKs
✓ Identified missing admission identifier
✓ Identified data-model inconsistencies
```

The next logical step is **Phase 4: normalization (1NF → 2NF → 3NF)**. There we'll take these current tables and decide exactly what should change, particularly **`ADMISSIONS`****,****`specialty`****,****`allergies`****, and the missing****`height`****field**.
