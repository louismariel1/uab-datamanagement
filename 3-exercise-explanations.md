Absolutely. A good way to prepare for this practical is to use **MCQs that test both the theory and the SQL you will actually write**.

Below is a practice set arranged from basic concepts → database design → SQL → more advanced queries.

# MCQ Practice — Healthcare SQL Database Practical

## 1\. What is the main purpose of a Primary Key?

A. To store duplicate values B. To uniquely identify each row in a table C. To connect two databases D. To sort the table alphabetically

**Answer: B**

**Explanation:** A Primary Key uniquely identifies each record. For example:

```
patient_id INT PRIMARY KEY
```

Two patients should not have the same `patient_id`.

---

## 2\. What is a Foreign Key used for?

A. To encrypt data B. To uniquely identify a table C. To establish a relationship between tables D. To delete duplicate records

**Answer: C**

**Explanation:** A Foreign Key references a Primary Key in another table.

For example:

```
PATIENT
patient_id (PK)
     ↑
     |
     |
ADMISSION
patient_id (FK)
```

The `patient_id` in `admissions` tells us which patient the admission belongs to.

---

## 3\. If one patient can have many admissions, what is the relationship?

A. 1:1 B. 1:N C. N:1 only D. N:N

**Answer: B**

**Explanation:**

```
PATIENT 1 ───────< ADMISSION
```

One patient can have many admissions, while each admission belongs to one patient.

This is a **one-to-many (1:N)** relationship.

---

# Normalization

## 4\. What is the main purpose of database normalization?

A. Make tables larger B. Reduce data redundancy and improve data integrity C. Make SQL queries longer D. Remove Primary Keys

**Answer: B**

**Explanation:** Normalization reduces unnecessary repetition and prevents problems such as:

- Update anomalies
- Insert anomalies
- Delete anomalies

---

## 5\. Which normal form requires atomic values?

A. 1NF B. 2NF C. 3NF D. 4NF

**Answer: A**

**Explanation:** **First Normal Form (1NF)** requires each field to contain a single, atomic value.

Bad example:

```
patient_id | allergies
1          | Peanuts, Penicillin, Latex
```

Depending on the design, allergies may need to be represented separately.

---

## 6\. What does 2NF primarily eliminate?

A. Foreign Keys B. Partial dependencies C. Primary Keys D. NULL values

**Answer: B**

**Explanation:** 2NF eliminates **partial dependencies**, which occur when a non-key attribute depends on only part of a composite key.

---

## 7\. What does 3NF primarily eliminate?

A. Primary Keys B. Foreign Keys C. Transitive dependencies D. All NULL values

**Answer: C**

**Explanation:** In 3NF, non-key attributes should depend on the key and not on another non-key attribute.

For example:

```
doctor_id → specialty_id → specialty_name
```

It may be better to put specialty information in a separate `specialties` table.

---

# CREATE TABLE

## 8\. Which SQL statement creates a table?

A. `MAKE TABLE` B. `NEW TABLE` C. `CREATE TABLE` D. `BUILD TABLE`

**Answer: C**

**Explanation:**

```
CREATE TABLE patients (
    patient_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);
```

---

## 9\. Which statement correctly defines a Primary Key?

A.

```
patient_id INT FOREIGN KEY
```

B.

```
patient_id INT PRIMARY KEY
```

C.

```
patient_id INT UNIQUE KEY ONLY
```

D.

```
patient_id INT IDENTIFIER
```

**Answer: B**

**Explanation:**

```
patient_id INT PRIMARY KEY
```

means `patient_id` uniquely identifies each patient.

---

## 10\. Which constraint is used to reference another table?

A. `CHECK` B. `DEFAULT` C. `FOREIGN KEY` D. `ORDER BY`

**Answer: C**

**Explanation:**

```
FOREIGN KEY (patient_id)
REFERENCES patients(patient_id)
```

This establishes a relationship between `admissions` and `patients`.

---

# INSERT

## 11\. Which SQL command is used to add a new patient?

A. `ADD` B. `INSERT` C. `CREATE` D. `UPDATE`

**Answer: B**

**Explanation:**

```
INSERT INTO patients
(patient_id, first_name, last_name)
VALUES
(1, 'John', 'Smith');
```

`INSERT` adds new records.

---

## 12\. What does this query do?

```
INSERT INTO patients
(patient_id, first_name)
VALUES
(10, 'Maria');
```

A. Deletes Maria B. Updates Maria C. Creates a new patient record D. Creates a new table

**Answer: C**

**Explanation:** `INSERT INTO` adds a new row to the table.

---

# SELECT and WHERE

## 13\. Which query displays all male patients?

A.

```
SELECT patients
WHERE gender = 'M';
```

B.

```
SELECT *
FROM patients
WHERE gender = 'M';
```

C.

```
GET *
FROM patients
WHERE gender = 'M';
```

D.

```
DISPLAY patients
WHERE gender = 'M';
```

**Answer: B**

**Explanation:**

```
SELECT *
FROM patients
WHERE gender = 'M';
```

means:

> Select every column from patients where gender is M.

---

## 14\. Which query displays only first name and last name?

A.

```
SELECT first_name, last_name
FROM patients;
```

B.

```
SELECT *
FROM patients;
```

C.

```
DISPLAY first_name AND last_name;
```

D.

```
GET first_name + last_name;
```

**Answer: A**

**Explanation:** You list the columns you want after `SELECT`.

---

# NULL

## 15\. How do you check whether allergies are NULL?

A.

```
WHERE allergies = NULL
```

B.

```
WHERE allergies IS NULL
```

C.

```
WHERE allergies == NULL
```

D.

```
WHERE allergies NULL
```

**Answer: B**

**Explanation:** SQL uses:

```
IS NULL
```

and:

```
IS NOT NULL
```

You should **not** use `= NULL`.

---

## 16\. Which query finds patients with no recorded allergies?

A.

```
SELECT first_name, last_name
FROM patients
WHERE allergies IS NULL;
```

B.

```
SELECT first_name, last_name
FROM patients
WHERE allergies = NULL;
```

C.

```
SELECT first_name, last_name
FROM patients
WHERE allergies = 'NULL';
```

D.

```
SELECT first_name, last_name
FROM patients
WHERE allergies = '';
```

**Answer: A**

**Explanation:** `NULL` means the value is missing/unknown, whereas an empty string `''` is an actual string value.

---

# UPDATE

## 17\. Which query changes NULL allergies to `NKA`?

A.

```
CHANGE patients
SET allergies = 'NKA';
```

B.

```
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;
```

C.

```
INSERT patients
SET allergies = 'NKA';
```

D.

```
ALTER patients
SET allergies = 'NKA';
```

**Answer: B**

**Explanation:** `UPDATE` modifies existing records.

The `WHERE` clause is extremely important because it prevents changing **every patient**.

---

## 18\. What could happen if you execute this?

```
UPDATE patients
SET allergies = 'NKA';
```

A. Only NULL allergies are changed B. No records are changed C. Every patient's allergies becomes `NKA` D. The table is deleted

**Answer: C**

**Explanation:** There is no `WHERE` condition, so the update applies to **all rows**.

This is an important practical SQL concept.

---

# COUNT

## 19\. Which query counts all admissions?

A.

```
SELECT TOTAL(*)
FROM admissions;
```

B.

```
SELECT COUNT(*)
FROM admissions;
```

C.

```
SELECT SUM(*)
FROM admissions;
```

D.

```
COUNT admissions;
```

**Answer: B**

**Explanation:**

```
COUNT(*)
```

counts the rows in the table.

---

## 20\. Which function is used to count records?

A. `SUM()` B. `TOTAL()` C. `COUNT()` D. `NUMBER()`

**Answer: C**

**Explanation:** `COUNT()` is an aggregate function used to count rows or non-NULL values.

---

# Dates

## 21\. How would you find admissions where the patient was admitted and discharged on the same date?

A.

```
WHERE admission_date != discharge_date
```

B.

```
WHERE admission_date = discharge_date
```

C.

```
WHERE admission_date IS discharge_date
```

D.

```
WHERE admission_date SAME discharge_date
```

**Answer: B**

**Explanation:**

```
SELECT *
FROM admissions
WHERE admission_date = discharge_date;
```

If both dates are identical, the patient was admitted and discharged on the same day.

---

# Conditional Aggregation

## 22\. Which technique can count male and female patients in separate columns but on the same row?

A. `ORDER BY` B. `UNION` C. Conditional aggregation using `CASE` D. `DISTINCT`

**Answer: C**

**Explanation:**

```
SELECT
    SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
    SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count
FROM patients;
```

The result could be:

```
male_count | female_count
-----------|-------------
25         | 30
```

---

# UNION

## 23\. What is `UNION` used for?

A. Joining columns using a foreign key B. Combining results from multiple SELECT statements C. Creating a database D. Updating records

**Answer: B**

**Explanation:**

```
SELECT first_name, last_name, 'Patient' AS role
FROM patients

UNION

SELECT first_name, last_name, 'Doctor' AS role
FROM doctors;
```

This combines patients and doctors into one result.

---

## 24\. In the following query, what does `'Patient' AS role` do?

```
SELECT first_name, last_name, 'Patient' AS role
FROM patients;
```

A. Searches for patients whose role is Patient B. Creates a calculated column called `role` containing "Patient" C. Changes the patient's role in the table D. Creates a new table called role

**Answer: B**

**Explanation:** It creates a result column:

```
first_name | last_name | role
-----------|-----------|--------
John       | Smith     | Patient
```

The original table isn't modified.

---

# JOIN

## 25\. Why would you use a JOIN between doctors and admissions?

A. To delete doctors B. To combine related information from both tables C. To create a backup D. To normalize a table

**Answer: B**

**Explanation:** For example:

```
FROM doctors d
JOIN admissions a
ON d.doctor_id = a.doctor_id
```

connects each admission to the doctor responsible for it.

---

## 26\. What does this condition mean?

```
ON d.doctor_id = a.doctor_id
```

A. Doctor IDs must be different B. The tables are related through doctor ID C. All doctors are deleted D. Admissions are copied

**Answer: B**

**Explanation:** The `ON` clause tells SQL **how the two tables are related**.

---

# GROUP BY

## 27\. Why is `GROUP BY` used in the yearly admissions question?

A. To delete duplicate doctors B. To group admissions by doctor and year C. To sort the database alphabetically D. To create a new table

**Answer: B**

**Explanation:**

You want something like:

```
Doctor 101 | 2025 | 15 admissions
Doctor 101 | 2026 | 20 admissions
Doctor 102 | 2025 | 10 admissions
```

Therefore, you group the records by doctor and year.

---

# CASE and BMI

## 28\. Why is `CASE` useful in the obesity question?

A. It creates a new table B. It allows SQL to return 1 or 0 depending on a condition C. It deletes patients D. It joins tables

**Answer: B**

**Explanation:**

```
CASE
    WHEN BMI >= 30 THEN 1
    ELSE 0
END AS isObese
```

means:

```
BMI >= 30 → 1
BMI < 30  → 0
```

---

## 29\. Height is stored in centimeters. A patient has a height of 180 cm. What is the height in meters?

A. 180 m B. 18 m C. 1.8 m D. 0.18 m

**Answer: C**

**Explanation:**

```
180 / 100 = 1.8 m
```

This conversion is important when calculating BMI.

---

## 30\. What is the BMI formula?

A.

```
weight × height
```

B.

```
weight / height
```

C.

```
weight / height²
```

D.

```
height / weight²
```

**Answer: C**

**Explanation:**

$$
BMI = \frac{weight(kg)}{height(m)^2}
$$

Since your database stores height in centimeters, SQL needs to convert it:

```
weight / POWER(height / 100.0, 2)
```

---

# Challenge Questions

These are closer to what you might be asked in a practical exam.

## 31\. What does the following query return?

```
SELECT COUNT(*)
FROM patients
WHERE gender = 'M';
```

A. Names of all male patients B. Number of male patients C. Number of all patients D. Number of female patients

**Answer: B**

**Explanation:** First SQL filters to male patients, then `COUNT(*)` counts those rows.

---

## 32\. Which query correctly finds patients born in 2010?

A.

```
SELECT COUNT(*)
FROM patients
WHERE date_of_birth = 2010;
```

B.

```
SELECT COUNT(*)
FROM patients
WHERE YEAR(date_of_birth) = 2010;
```

C.

```
SELECT YEAR(*)
FROM patients
WHERE date_of_birth = 2010;
```

D.

```
SELECT COUNT(YEAR)
FROM patients;
```

**Answer: B**

**Explanation:** A date contains more information than just the year. `YEAR(date_of_birth)` extracts the year.

---

## 33\. Which query correctly calculates `isObese`?

A.

```
CASE
WHEN weight > height THEN 1
ELSE 0
END
```

B.

```
CASE
WHEN weight / POWER(height / 100.0, 2) >= 30 THEN 1
ELSE 0
END
```

C.

```
CASE
WHEN weight + height >= 30 THEN 1
ELSE 0
END
```

D.

```
CASE
WHEN height / weight >= 30 THEN 1
ELSE 0
END
```

**Answer: B**

**Explanation:** The obesity condition is based on BMI:

$$
BMI = \frac{weight}{height^2}
$$

and height must first be converted from centimeters to meters.

---

## 34\. Which SQL concepts are needed for the yearly admissions question?

A. Only `SELECT` B. Only `WHERE` C. `JOIN`, `GROUP BY`, `COUNT`, and date functions D. Only `INSERT`

**Answer: C**

**Explanation:** You need to:

1. Join doctors to admissions.
2. Extract the year from the admission date.
3. Group by doctor and year.
4. Count admissions.

This is why this is one of the more advanced questions in the practical.

---

# 🧠 Quick Exam Cheat Sheet

If you see this in the question...

| Question wording | Think of |
| --- | --- |
| "display/show" | `SELECT` |
| "where..." | `WHERE` |
| "no value / missing" | `IS NULL` |
| "change existing data" | `UPDATE` |
| "add new data" | `INSERT` |
| "how many?" | `COUNT()` |
| "total" | `SUM()` / `COUNT()` |
| "same day" | compare dates with `=` |
| "combine patients and doctors" | `UNION` |
| "information from two related tables" | `JOIN` |
| "for each doctor/year" | `GROUP BY` |
| "if condition then..." | `CASE` |
| "obesity" | BMI + `CASE` |
| "yearly" | `YEAR(date)` |
| "unique identifier" | Primary Key |
| "connect tables" | Foreign Key |
| "reduce redundancy" | Normalization |
| "atomic values" | 1NF |
| "partial dependency" | 2NF |
| "transitive dependency" | 3NF |

### The 5 things I'd memorize before the practical

```
-- 1. SELECT
SELECT first_name, last_name
FROM patients
WHERE gender = 'M';

-- 2. NULL
SELECT *
FROM patients
WHERE allergies IS NULL;

-- 3. UPDATE
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;

-- 4. COUNT
SELECT COUNT(*)
FROM admissions;

-- 5. JOIN + GROUP BY
SELECT d.doctor_id, COUNT(*) AS total
FROM doctors d
JOIN admissions a
    ON d.doctor_id = a.doctor_id
GROUP BY d.doctor_id;
```

If you understand **why each of those five works**, rather than just memorizing them, you'll have a strong foundation for this practical.
