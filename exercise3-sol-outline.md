# Solution Roadmap for the Healthcare SQL Practical

The best way to approach this practical is **not to start writing SQL immediately**. Follow the assignment in a fixed sequence:

```
Original ERD
    ↓
Identify entities & attributes
    ↓
Identify PKs & FKs
    ↓
Identify relationships/cardinality
    ↓
Check normalization
    ↓
Create improved 3NF design
    ↓
Write CREATE TABLE
    ↓
Insert sample data
    ↓
Test the database
    ↓
Solve SQL questions 1–9
    ↓
Check results
```

## Phase 1 — Understand the given ERD

Start by examining the provided healthcare ERD.

Make a table like this for yourself:

| Entity/Table | Important attributes | PK | FK |
| --- | --- | --- | --- |
| Patient | patient_id, first_name, last\_name, gender, etc. | patient\_id | — |
| Doctor | doctor_id, first_name, last\_name, specialty, etc. | doctor\_id | — |
| Admission | admission_id, admission_date, discharge\_date, etc. | admission\_id | patient_id, doctor_id |

**Don't assume these exact tables/columns** until you inspect the actual ERD.

Your first goal is simply:

> **What are the entities, and what information belongs to each entity?**

---

# Phase 2 — Identify relationships

For every relationship, ask:

> "How many records of A can be associated with one record of B?"

For example:

```
PATIENT 1 ───────────< ADMISSION
```

means:

> One patient can have many admissions.

And:

```
DOCTOR 1 ───────────< ADMISSION
```

means:

> One doctor can be responsible for many admissions.

Write down the cardinalities:

```
Patient → Admission     1:N
Doctor  → Admission     1:N
```

If the original ERD has many-to-many relationships, you'll normally need an **associative/junction table**.

---

# Phase 3 — Identify Primary and Foreign Keys

Go through every table and mark:

### Primary Key

Ask:

> "What uniquely identifies one record?"

Example:

```
PATIENT
----------------
PK patient_id
first_name
last_name
gender
```

### Foreign Key

Ask:

> "Which column points to a record in another table?"

Example:

```
ADMISSION
----------------
PK admission_id
FK patient_id
FK doctor_id
admission_date
discharge_date
```

You should be able to draw:

```
PATIENT
PK patient_id
      ↑
      |
FK patient_id
ADMISSION
```

---

# Phase 4 — Normalize to 3NF

Now inspect the original design for unnecessary duplication.

Use this checklist.

### 1NF

Ask:

> Does every field contain a single value?

Look for things such as:

```
allergies = "Peanuts, Penicillin, Latex"
```

or multiple values stored in one field.

---

### 2NF

Ask:

> If there is a composite key, does every non-key attribute depend on the entire key?

If the answer is no, move that attribute to the appropriate table.

---

### 3NF

Ask:

> Does a non-key attribute depend on another non-key attribute?

For example:

```
doctor_id → specialty_id → specialty_name
```

This suggests that specialty information belongs in a separate table.

---

## Phase 5 — Create your final 3NF design

At this point, produce your **final ERD**.

Your final design should show:

- Tables/entities
- Attributes
- PKs
- FKs
- Relationships
- Cardinalities

A simplified example might look like:

```
             ┌──────────────┐
             │  SPECIALTY   │
             │--------------│
             │ PK specialty │
             │ specialty... │
             └───────┬──────┘
                     │
                     │ 1:N
                     ↓
             ┌──────────────┐
             │    DOCTOR    │
             │--------------│
             │ PK doctor_id │
             │ first_name   │
             │ last_name    │
             │ FK specialty │
             └───────┬──────┘
                     │
                     │ 1:N
                     ↓
┌──────────────┐    ┌──────────────┐
│   PATIENT    │    │  ADMISSION   │
│--------------│    │--------------│
│ PK patient_id│←───│ FK patient_id│
│ first_name   │ 1:N│ FK doctor_id │
│ last_name    │    │ PK admission │
│ gender       │    │ admission_date
│ height       │    │ discharge_date
│ weight       │    └──────────────┘
└──────────────┘
```

Again, use your actual ERD rather than blindly copying this example.

---

# Phase 6 — Convert ERD → SQL

Now translate each table into `CREATE TABLE`.

Use this order:

```
1. Independent/reference tables
             ↓
2. Main entities
             ↓
3. Tables containing foreign keys
```

For example:

```
CREATE TABLE specialties (...);

CREATE TABLE doctors (
    ...
    FOREIGN KEY (specialty_id)
        REFERENCES specialties(specialty_id)
);

CREATE TABLE patients (...);

CREATE TABLE admissions (
    ...
    FOREIGN KEY (patient_id)
        REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id)
        REFERENCES doctors(doctor_id)
);
```

### Check before moving on

Make sure:

- Every table has a PK.
- Every FK references an existing PK.
- Data types make sense.
- Names are consistent.
- Required fields aren't unnecessarily nullable.

---

# Phase 7 — Insert sample data

Now populate the database.

Don't insert just three rows total. For testing the later queries, create enough data to produce meaningful results.

I'd recommend having:

```
Patients:       8–15
Doctors:        3–5
Specialties:    3–5
Admissions:     10–20
```

Make sure your sample data deliberately includes:

- Male patients
- Female patients
- Patients born in 2010
- Patients with `NULL` allergies
- Patients with allergies
- Same-day admissions
- Multi-day admissions
- Multiple admissions for the same doctor
- Admissions across different years
- Patients with BMI ≥ 30
- Patients with BMI \< 30

This is important because otherwise you can't properly test the queries.

---

# Phase 8 — Test basic SQL first

Before attempting the assignment questions, verify that your database works.

Run:

```
SELECT * FROM patients;
```

Then:

```
SELECT * FROM doctors;
```

Then:

```
SELECT * FROM admissions;
```

Then check relationships:

```
SELECT *
FROM admissions
JOIN patients
    ON admissions.patient_id = patients.patient_id;
```

If these work, you're ready for Part II.

---

# Phase 9 — Solve Part II in increasing difficulty

Don't jump directly to Question 9.

Use this progression:

### Level 1 — Basic filtering

Questions 1–3:

```
SELECT
WHERE
IS NULL
UPDATE
```

### Level 2 — Aggregation

Questions 4–5:

```
COUNT()
YEAR()
```

### Level 3 — Conditions

Question 6:

```
WHERE admission_date = discharge_date
```

### Level 4 — Conditional aggregation

Question 7:

```
CASE
SUM()
```

### Level 5 — UNION

Question 8:

```
UNION
```

### Level 6 — Calculated fields

Obesity:

```
BMI
POWER()
CASE
```

### Level 7 — Multi-table analysis

Final question:

```
JOIN
YEAR()
COUNT()
GROUP BY
ORDER BY
```

---

# Phase 10 — Build each query using a template

For every question, ask yourself four things:

### 1\. What table contains the information?

For example:

> Male patients → `patients`

### 2\. Which columns do I need?

For example:

```
first_name
last_name
gender
```

### 3\. Do I need a condition?

For example:

```
WHERE gender = 'M'
```

### 4\. Do I need aggregation or another table?

For example:

```
"how many" → COUNT
"for each doctor" → GROUP BY
"doctor information + admission information" → JOIN
"patients OR doctors" → UNION
"if..." → CASE
```

This approach makes the questions much easier.

---

# A useful decision tree

When you read a practical question, mentally translate the wording:

```
"Display..."
       ↓
    SELECT

"Only those where..."
       ↓
    WHERE

"No recorded..."
       ↓
    IS NULL

"Change..."
       ↓
    UPDATE

"How many?"
       ↓
    COUNT

"Total..."
       ↓
    COUNT / SUM

"From two tables..."
       ↓
    JOIN

"Either A or B..."
       ↓
    UNION

"For each..."
       ↓
    GROUP BY

"If condition..."
       ↓
    CASE

"Calculate..."
       ↓
    Expression / formula
```

---

# Final submission structure

I'd organize your practical/report in exactly this order:

## Part I — Database Design

### 1\. Original ERD analysis

Show the original ERD and identify:

- Entities
- Attributes
- PKs
- FKs
- Relationships
- Cardinalities

### 2\. Normalization

Explain:

- 1NF
- 2NF
- 3NF
- Problems in the original design
- Changes you made
- Why the changes improve the design

### 3\. Final 3NF ERD

Include:

- PKs
- FKs
- Cardinalities

### 4\. SQL Schema

Provide all:

```
CREATE TABLE ...
```

statements.

### 5\. Sample Data

Provide at least three:

```
INSERT INTO ...
```

statements, preferably more so you can test the queries properly.

---

# Part II — SQL Queries

Then present:

```
1. Male patients
2. Patients with NULL allergies
3. Update NULL allergies
4. Patients born in 2010
5. Number of admissions
6. Same-day admissions
7. Male/female counts
8. Patients + doctors
9. BMI/isObese
10. Yearly admissions by doctor
```

For each one, ideally provide:

```
Question
↓
SQL query
↓
Short explanation
↓
Expected/result example
```

---

# ⭐ The most important strategy

Think of the practical as **three separate skills**:

```
        DATABASE DESIGN
              │
       ┌──────┴──────┐
       ↓             ↓
    ERD/3NF       SQL Schema
       │             │
       └──────┬──────┘
              ↓
          SQL QUERIES
```

**First:** make sure your database design is correct.

**Second:** make sure your SQL tables correctly implement that design.

**Third:** write queries against those tables.

If you get the **ERD → PK/FK → 3NF → CREATE TABLE** sequence right, most of the SQL questions become much easier because you already understand how the tables are related.
