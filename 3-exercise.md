Absolutely. This assignment is essentially asking you to do **two things**:

1. **Design a good healthcare database** from an ERD and implement it in SQL.
2. **Write SQL queries** that answer practical questions about the healthcare data.

The important part is that **Part I comes before Part II**: you first decide what the tables and relationships should look like, then create/populate them, and finally query them.

## Part I — Database Design

### 1\. Redraw the ERD with PKs, FKs, and cardinalities

You are given an initial healthcare ERD. Your first job is to redraw it more formally.

You need to clearly identify:

- **PK (Primary Key)** — uniquely identifies each row.
- **FK (Foreign Key)** — connects one table to another.
- **Cardinality** — tells you how many records can be related.

For example, suppose you have:

```
PATIENT
---------
patient_id (PK)
first_name
last_name
gender
...
```

and:

```
ADMISSION
---------
admission_id (PK)
patient_id (FK)
doctor_id (FK)
admission_date
discharge_date
...
```

Then the relationship could be:

```
PATIENT 1 ───────────< ADMISSION
```

This means:

> One patient can have many admissions, but each admission belongs to one patient.

Similarly:

```
DOCTOR 1 ───────────< ADMISSION
```

means one doctor can be responsible for many admissions.

You should put the PK/FK labels directly on your ERD.

---

# 2\. Normalize the database to 3NF

This is one of the most important parts of the exercise.

**Normalization** means organizing the tables so that you don't unnecessarily repeat information and don't create problems when inserting, updating, or deleting data.

You need to consider:

- **1NF**
- **2NF**
- **3NF**

### First Normal Form — 1NF

A table is in 1NF when:

- Each column contains a single value.
- There are no repeating groups.
- Each row can be uniquely identified.

For example, this is problematic:

```
patient_id | allergies
-----------|---------------------
1          | Penicillin, Peanuts
```

A more normalized design might use an allergy table:

```
PATIENT
patient_id
first_name
last_name
...

ALLERGY
allergy_id
allergy_name

PATIENT_ALLERGY
patient_id
allergy_id
```

Now one patient can have multiple allergies without storing multiple values in one column.

However, **don't automatically split every column into another table**. You need to look at what the original ERD actually contains.

---

### Second Normal Form — 2NF

2NF mainly matters when a table has a **composite primary key**.

For example:

```
PATIENT_DOCTOR
patient_id
doctor_id
doctor_name
```

If the PK is:

```
(patient_id, doctor_id)
```

then `doctor_name` depends only on `doctor_id`, not on the whole combination.

Therefore, `doctor_name` belongs in:

```
DOCTOR
doctor_id (PK)
doctor_name
```

and the relationship table becomes:

```
PATIENT_DOCTOR
patient_id (FK)
doctor_id (FK)
```

This eliminates partial dependencies.

---

### Third Normal Form — 3NF

3NF eliminates **transitive dependencies**.

For example:

```
DOCTOR
doctor_id
doctor_name
specialty
specialty_phone
```

If:

```
doctor_id → specialty
specialty → specialty_phone
```

then `specialty_phone` doesn't really depend directly on the doctor.

You could instead have:

```
DOCTOR
doctor_id
doctor_name
specialty_id
```

and:

```
SPECIALTY
specialty_id
specialty_name
specialty_phone
```

Now the database is better normalized.

### What your assignment wants

For Task 2, you should explain **what problems exist in the original ERD and what you changed**.

For each change, explain something like:

> The specialty information was separated into its own table because storing the same specialty information repeatedly for multiple doctors would cause redundancy and potential update anomalies. The resulting design satisfies 3NF because non-key attributes depend on the key, the whole key, and nothing but the key.

Don't just draw a new ERD—you need to **justify the changes**.

---

# 3\. Redraw the improved 3NF ERD

After deciding what needs to change, create your final ERD.

It should show all your final tables, for example:

```
PATIENT
----------------
PK patient_id
first_name
last_name
gender
date_of_birth
weight
height
...

        1
        |
        |
        N
ADMISSION
----------------
PK admission_id
FK patient_id
FK doctor_id
admission_date
discharge_date

        N
        |
        |
        1
DOCTOR
----------------
PK doctor_id
first_name
last_name
FK specialty_id

        N
        |
        |
        1
SPECIALTY
----------------
PK specialty_id
specialty_name
```

Your actual ERD should follow the entities and attributes from the provided diagram.

---

# 4\. Create the database using SQL

Once your final design is complete, translate each table into a `CREATE TABLE`.

For example:

```
CREATE TABLE patients (
    patient_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    gender CHAR(1),
    date_of_birth DATE,
    weight DECIMAL(5,2),
    height DECIMAL(5,2),
    allergies VARCHAR(255)
);
```

Then:

```
CREATE TABLE doctors (
    doctor_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    specialty_id INT,
    FOREIGN KEY (specialty_id)
        REFERENCES specialties(specialty_id)
);
```

And an admission table might look like:

```
CREATE TABLE admissions (
    admission_id INT PRIMARY KEY,
    patient_id INT,
    doctor_id INT,
    admission_date DATE,
    discharge_date DATE,
    FOREIGN KEY (patient_id)
        REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id)
        REFERENCES doctors(doctor_id)
);
```

The **order matters** when creating tables.

If `admissions` references `patients` and `doctors`, you normally create:

```
1. specialties
2. doctors
3. patients
4. admissions
```

rather than creating `admissions` first.

---

# 5\. Insert sample data

You need at least **three INSERT statements involving different tables**.

For example:

```
INSERT INTO patients
(patient_id, first_name, last_name, gender, date_of_birth, weight, height, allergies)
VALUES
(1, 'John', 'Smith', 'M', '2010-05-12', 70.0, 175.0, NULL);
```

Another table:

```
INSERT INTO doctors
(doctor_id, first_name, last_name, specialty_id)
VALUES
(101, 'Sarah', 'Jones', 1);
```

And:

```
INSERT INTO admissions
(admission_id, patient_id, doctor_id, admission_date, discharge_date)
VALUES
(1001, 1, 101, '2026-10-01', '2026-10-01');
```

The exact columns and values need to match **your actual ERD/schema**.

---

# Part II — SQL Queries

Now you use the database you created.

I'll explain what each question is testing.

## 1\. Find all male patients

> Display first name, last name, and gender where gender = `'M'`.

This tests:

- `SELECT`
- `WHERE`

```
SELECT first_name, last_name, gender
FROM patients
WHERE gender = 'M';
```

---

# 2\. Find patients with no recorded allergies

The key idea here is **NULL**.

You cannot write:

```
WHERE allergies = NULL
```

That is incorrect SQL.

You must use:

```
IS NULL
```

So:

```
SELECT first_name, last_name
FROM patients
WHERE allergies IS NULL;
```

This tests whether you understand SQL's special treatment of `NULL`.

---

# 3\. Replace NULL allergies with NKA

Now you're modifying the data.

`NKA` means:

> No Known Allergies

You need:

```
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;
```

After this query, patients who previously had:

```
NULL
```

will have:

```
NKA
```

---

# 4\. Count patients born in 2010

You need to count rows where the year of birth is 2010.

Depending on your SQL database system, one solution is:

```
SELECT COUNT(*) AS total_patients
FROM patients
WHERE YEAR(date_of_birth) = 2010;
```

This tests:

- `COUNT()`
- filtering
- extracting the year from a date

If your DBMS isn't MySQL/SQL Server-compatible, the syntax for extracting the year may differ.

---

# 5\. Count total admissions

This is straightforward aggregation:

```
SELECT COUNT(*) AS total_admissions
FROM admissions;
```

You're counting the number of rows in the `admissions` table.

---

# 6\. Admissions where admission and discharge occurred on the same day

You need to compare the two dates:

```
SELECT *
FROM admissions
WHERE admission_date = discharge_date;
```

For example:

```
admission_date | discharge_date
---------------|---------------
2026-10-01     | 2026-10-01
```

would be included.

But:

```
2026-10-01     | 2026-10-03
```

would not.

---

# 7\. Male and female counts in a single row

This question is slightly more advanced.

They want something like:

```
male_count | female_count
-----------|-------------
25         | 30
```

One way is conditional aggregation:

```
SELECT
    SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
    SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count
FROM patients;
```

The important concept here is **conditional aggregation**.

Instead of producing separate rows, you produce two calculated columns.

---

# 8\. Patients AND doctors together

The question says:

> Display first name, last name, and role of every person who appears either as a patient or as a doctor.

This is testing **`UNION`**.

For patients:

```
SELECT first_name, last_name, 'Patient' AS role
FROM patients
```

For doctors:

```
SELECT first_name, last_name, 'Doctor' AS role
FROM doctors
```

Combine them:

```
SELECT first_name, last_name, 'Patient' AS role
FROM patients

UNION

SELECT first_name, last_name, 'Doctor' AS role
FROM doctors;
```

The result might look like:

```
first_name | last_name | role
-----------|-----------|---------
John       | Smith     | Patient
Mary       | Brown     | Patient
David      | Jones     | Doctor
Sarah      | White     | Doctor
```

`UNION` combines the results of two compatible `SELECT` statements.

---

# 9\. Calculate whether a patient is obese

This is probably the most important calculated-field question.

You're given:

```
patient_id
weight
height
isObese
```

The formula for BMI is:

$$
BMI = \frac{weight\;(kg)}{height\;(m)^2}
$$

But the assignment tells you:

> Height is stored in centimeters.

So if:

```
height = 175 cm
```

you must convert it:

```
175 / 100 = 1.75 m
```

Therefore:

```
weight / POWER(height / 100.0, 2)
```

calculates BMI.

The standard obesity threshold is BMI ≥ 30.

You can therefore create a Boolean-style field using `CASE`:

```
SELECT
    patient_id,
    weight,
    height,
    CASE
        WHEN weight / POWER(height / 100.0, 2) >= 30
        THEN 1
        ELSE 0
    END AS isObese
FROM patients;
```

For example:

```
weight = 100 kg
height = 170 cm
```

BMI:

$$
100 / 1.7^2 \approx 34.6
$$

so:

```
isObese = 1
```

Whereas a BMI below 30 produces:

```
isObese = 0
```

### Why use `CASE`?

Because SQL doesn't automatically know that you want:

```
BMI >= 30 → 1
BMI < 30  → 0
```

`CASE` lets you create that calculated value.

---

# 10\. Yearly admissions for each doctor

This is the most advanced query in the assignment because it combines:

- `JOIN`
- `GROUP BY`
- date functions
- `COUNT`
- calculated fields

You need to produce:

```
doctor_id
doctor_full_name
specialty
year
total_admissions
```

Conceptually, you're joining:

```
DOCTOR
   |
   | doctor_id
   |
ADMISSION
```

Then grouping the admissions by:

```
doctor + year
```

A query could look like:

```
SELECT
    d.doctor_id,
    CONCAT(d.first_name, ' ', d.last_name) AS doctor_full_name,
    s.specialty_name AS specialty,
    YEAR(a.admission_date) AS year,
    COUNT(*) AS total_admissions
FROM doctors d
JOIN admissions a
    ON d.doctor_id = a.doctor_id
JOIN specialties s
    ON d.specialty_id = s.specialty_id
GROUP BY
    d.doctor_id,
    d.first_name,
    d.last_name,
    s.specialty_name,
    YEAR(a.admission_date)
ORDER BY
    d.doctor_id,
    year;
```

The output could look like:

```
doctor_id | doctor_full_name | specialty | year | total_admissions
----------|------------------|-----------|------|-----------------
101       | Sarah Jones      | Cardiology| 2024 | 12
101       | Sarah Jones      | Cardiology| 2025 | 18
101       | Sarah Jones      | Cardiology| 2026 | 15
102       | David Brown      | Pediatrics| 2025 | 10
102       | David Brown      | Pediatrics| 2026 | 14
```

This tells you, for example:

> Doctor 101 had 12 admissions in 2024, 18 in 2025, and 15 in 2026.

---

# The overall workflow

The easiest way to think about the whole assignment is:

```
                PART I
                  ↓
        Understand the original ERD
                  ↓
       Identify entities and attributes
                  ↓
       Identify PKs and FKs
                  ↓
       Identify relationships/cardinality
                  ↓
          Check normalization
                  ↓
             1NF → 2NF → 3NF
                  ↓
       Draw the improved 3NF ERD
                  ↓
         CREATE TABLE statements
                  ↓
            INSERT sample data
                  ↓
                PART II
                  ↓
             SELECT / WHERE
                  ↓
             NULL handling
                  ↓
              UPDATE
                  ↓
          COUNT / aggregation
                  ↓
               JOIN / UNION
                  ↓
           CASE / calculated fields
                  ↓
             GROUP BY + JOIN
```

## What your lecturer is really testing

| Task | Main concept being tested |
| --- | --- |
| Part I.1 | ERD, PK, FK, cardinality |
| Part I.2 | Normalization, 1NF/2NF/3NF |
| Part I.3 | Improved database design |
| Part I.4 | `CREATE TABLE`, constraints |
| Part I.5 | `INSERT` |
| Query 1 | `SELECT`, `WHERE` |
| Query 2 | `NULL`, `IS NULL` |
| Query 3 | `UPDATE` |
| Query 4 | `COUNT`, date functions |
| Query 5 | `COUNT` |
| Query 6 | Date comparison |
| Query 7 | Conditional aggregation |
| Query 8 | `UNION` |
| Query 9 | `CASE`, BMI calculation |
| Query 10 | `JOIN`, `GROUP BY`, `COUNT`, date functions |

### One important point

The **exact SQL for Part I cannot be finalized reliably from the text alone**, because the actual ERD is represented here as an image and its table/attribute names aren't visible in the pasted text. The assignment expects you to base your final schema on **that specific ERD**.

If you upload the **ERD image (image1)**, I can take it from there and give you the complete solution: **original ERD analysis → 1NF/2NF/3NF explanation → final ERD structure → all****`CREATE TABLE`****statements → sample****`INSERT`****s → all Part II SQL queries**, with each query explained line by line.
