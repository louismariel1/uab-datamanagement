## Phase 1 — Identify the entities and attributes

From the practical, the core entities we need are **Patient, Doctor, Admission**, and potentially **Specialty** if specialty is stored as a separate entity in the improved design.

A useful first-pass table is:

| Entity | Attributes | Description |
| --- | --- | --- |
| **Patient** | `patient_id`, `first_name`, `last_name`, `gender`, `date_of_birth`, `weight`, `height`, `allergies` | Stores information about patients |
| **Doctor** | `doctor_id`, `first_name`, `last_name`, `specialty` / `specialty_id` | Stores information about doctors |
| **Admission** | `admission_id`, `patient_id`, `doctor_id`, `admission_date`, `discharge_date` | Records a patient's hospital admission |
| **Specialty** | `specialty_id`, `specialty_name` | Stores medical specialties if separated during normalization |

The assignment itself confirms several of these attributes because the later queries require:

- `patients.first_name`
- `patients.last_name`
- `patients.gender`
- `patients.allergies`
- `patients.date_of_birth`
- `patients.weight`
- `patients.height`
- `admissions.admission_date`
- `admissions.discharge_date`
- `doctor_id`
- doctor name
- doctor specialty

So these are the important attributes we need to preserve.

---

# Phase 2 — Identify the relationships

Now we ask how these entities relate.

### Patient → Admission

A patient can be admitted to the hospital multiple times.

```
PATIENT 1 ───────────< ADMISSION
```

Therefore:

**Patient : Admission = 1:N**

One patient can have many admissions, but each admission belongs to one patient.

---

### Doctor → Admission

A doctor can be responsible for many admissions.

```
DOCTOR 1 ───────────< ADMISSION
```

Therefore:

**Doctor : Admission = 1:N**

One doctor can be associated with many admissions, but each admission is associated with one doctor.

---

### Specialty → Doctor

If we normalize specialty into its own table:

```
SPECIALTY 1 ───────────< DOCTOR
```

Therefore:

**Specialty : Doctor = 1:N**

One specialty can have many doctors, while each doctor belongs to one specialty.

For example:

```
CARDIOLOGY
    │
    ├── Dr. Smith
    ├── Dr. Jones
    └── Dr. Brown
```

---

# Phase 3 — Add PKs and FKs

Now we can produce the table structure with the **Primary Keys and Foreign Keys explicitly identified**.

## 3.1 Patient table

```
┌─────────────────────────────────┐
│            PATIENT              │
├─────────────────────────────────┤
│ PK  patient_id                  │
│     first_name                  │
│     last_name                   │
│     gender                      │
│     date_of_birth               │
│     weight                      │
│     height                      │
│     allergies                   │
└─────────────────────────────────┘
```

### Primary Key

```
patient_id
```

`patient_id` uniquely identifies each patient.

There is **no FK** in this table in the basic design.

---

# 3.2 Doctor table

If specialty is kept as a separate table, the Doctor table becomes:

```
┌─────────────────────────────────┐
│             DOCTOR              │
├─────────────────────────────────┤
│ PK  doctor_id                   │
│     first_name                  │
│     last_name                   │
│ FK  specialty_id                │
└─────────────────────────────────┘
```

### Primary Key

```
doctor_id
```

### Foreign Key

```
specialty_id
```

It references:

```
SPECIALTY.specialty_id
```

So:

```
DOCTOR.specialty_id
          ↓
SPECIALTY.specialty_id
```

---

# 3.3 Specialty table

If specialty is normalized into its own entity:

```
┌─────────────────────────────────┐
│           SPECIALTY             │
├─────────────────────────────────┤
│ PK  specialty_id                │
│     specialty_name              │
└─────────────────────────────────┘
```

### Primary Key

```
specialty_id
```

There is no FK here.

---

# 3.4 Admission table

This is the important linking table.

```
┌─────────────────────────────────┐
│           ADMISSION             │
├─────────────────────────────────┤
│ PK  admission_id                │
│ FK  patient_id                  │
│ FK  doctor_id                   │
│     admission_date              │
│     discharge_date              │
└─────────────────────────────────┘
```

### Primary Key

```
admission_id
```

### Foreign Keys

```
patient_id → PATIENT.patient_id
doctor_id  → DOCTOR.doctor_id
```

So we have:

```
ADMISSION.patient_id
        ↓
PATIENT.patient_id
```

and:

```
ADMISSION.doctor_id
        ↓
DOCTOR.doctor_id
```

---

# Complete PK/FK Table

This is probably the table you're looking for for **Phase 3**:

| Table | Column | Key | References |
| --- | --- | --- | --- |
| **PATIENT** | `patient_id` | **PK** | — |
|  | `first_name` | — | — |
|  | `last_name` | — | — |
|  | `gender` | — | — |
|  | `date_of_birth` | — | — |
|  | `weight` | — | — |
|  | `height` | — | — |
|  | `allergies` | — | — |
| **DOCTOR** | `doctor_id` | **PK** | — |
|  | `first_name` | — | — |
|  | `last_name` | — | — |
|  | `specialty_id` | **FK** | `SPECIALTY.specialty_id` |
| **SPECIALTY** | `specialty_id` | **PK** | — |
|  | `specialty_name` | — | — |
| **ADMISSION** | `admission_id` | **PK** | — |
|  | `patient_id` | **FK** | `PATIENT.patient_id` |
|  | `doctor_id` | **FK** | `DOCTOR.doctor_id` |
|  | `admission_date` | — | — |
|  | `discharge_date` | — | — |

---

# Complete relationship diagram

Putting the PKs, FKs, and cardinalities together:

```
                    ┌──────────────────────┐
                    │      SPECIALTY       │
                    ├──────────────────────┤
                    │ PK specialty_id      │
                    │    specialty_name    │
                    └──────────┬───────────┘
                               │
                              1│
                               │
                              N│
                    ┌──────────▼───────────┐
                    │        DOCTOR        │
                    ├──────────────────────┤
                    │ PK doctor_id         │
                    │    first_name        │
                    │    last_name         │
                    │ FK specialty_id      │
                    └──────────┬───────────┘
                               │
                              1│
                               │
                              N│
                    ┌──────────▼───────────┐
                    │      ADMISSION       │
                    ├──────────────────────┤
                    │ PK admission_id      │
                    │ FK patient_id        │
                    │ FK doctor_id         │
                    │    admission_date    │
                    │    discharge_date    │
                    └──────────▲───────────┘
                               │
                              N│
                               │
                              1│
                    ┌──────────┴───────────┐
                    │       PATIENT        │
                    ├──────────────────────┤
                    │ PK patient_id        │
                    │    first_name        │
                    │    last_name         │
                    │    gender            │
                    │    date_of_birth     │
                    │    weight            │
                    │    height            │
                    │    allergies         │
                    └──────────────────────┘
```

The relationships can therefore be summarized as:

```
SPECIALTY 1 ─────── N DOCTOR
DOCTOR    1 ─────── N ADMISSION
PATIENT   1 ─────── N ADMISSION
```

## One important caveat

I would **not yet treat this as the final answer for your assignment** until we verify it against the actual ERD image. In particular, we need to check whether the original ERD has additional entities/attributes or represents `specialty` differently.

**Next step:** if you upload the original **ERD image (image1.png)**, we can compare it against this structure and then do **Phase 2 normalization properly (1NF → 2NF → 3NF)** rather than making assumptions.
