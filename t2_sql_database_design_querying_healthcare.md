# Practice Session 2 — SQL Database Design and Querying in a Healthcare System

This solution includes the proposed normalized database design, SQL implementation, sample data, solutions to all nine SQL exercises, and additional exam-practice questions with worked solutions.

Important assumption: The ERD image and the obesity formula in the supplied document are not readable in the extracted text. I will use a standard healthcare schema with `patients`, `doctors`, and `admissions`, and explicitly state the assumptions so you can adapt the solution to your lecturer’s exact diagram.

The SQL below uses MySQL 8.0+ syntax. A few functions, especially date extraction and Boolean expressions, may differ in PostgreSQL or SQLite.

## Part I — Database design

### Exercise 1. Redraw the ERD with primary keys, foreign keys, and cardinalities

A reasonable starting design contains three entities.

### PATIENTS

- `patient_id` — PK
- `first_name`
- `last_name`
- `gender`
- `date_of_birth`
- `allergies`
- `weight`
- `height`

One patient can have many admissions

### ADMISSIONS

- `admission_id` — PK
- `patient_id` — FK → patients.patient\_id
- `doctor_id` — FK → doctors.doctor\_id
- `admission_date`
- `discharge_date`

One doctor can initiate many admissions

### DOCTORS

- `doctor_id` — PK
- `first_name`
- `last_name`
- `specialty`

The two relationships are:

- PATIENTS 1 : N ADMISSIONS: each admission belongs to one patient; a patient may have zero, one, or many admissions.
- DOCTORS 1 : N ADMISSIONS: each admission is associated with one doctor; a doctor may initiate zero, one, or many admissions.

The `patient_id` and `doctor_id` columns belong in `admissions` because each admission references the patient and doctor involved.

### Exercise 2. Normalize the design up to Third Normal Form (3NF)

Normalization reduces duplicated information and prevents insertion, update, and deletion anomalies.

#### First Normal Form (1NF)

A table satisfies 1NF when each field contains one atomic value and there are no repeating groups.

- Store one patient's details per patient row.
- Store one doctor's details per doctor row.
- Store one admission per admission row.
- Do not store multiple doctors or admissions in a single column.

For example, a value such as `"Penicillin, Latex"` in an allergies field is not a fully normalized representation if individual allergies must be searched or managed independently. In that case, create separate allergy tables.

#### Second Normal Form (2NF)

A table is in 2NF when it is in 1NF and every non-key attribute depends on the whole candidate key, not just part of a composite key.

With the proposed schema, each table uses a single-column primary key. Therefore, there are no partial dependencies on a composite primary key to eliminate.

#### Third Normal Form (3NF)

A table is in 3NF when it is in 2NF and has no inappropriate transitive dependencies of non-key attributes on a key.

For example, do not store the doctor's name and specialty repeatedly in `admissions`. Store them once in `doctors` and reference the doctor through `doctor_id`.

Likewise, do not repeat patient details in every admission row. Store them in `patients`.

An improved design can also normalize allergies:

- `patients` stores patient demographics.
- `allergies` stores each allergy once.
- `patient_allergies` associates patients with allergies, allowing multiple allergies per patient.

This is preferable when allergy information needs structured management. For a minimal exercise schema, a nullable `allergies` field is sufficient to answer the lecturer's queries, but it is less flexible.

### Exercise 3. Improved 3NF ERD

For the more normalized version, use five tables:

PATIENTS

`patient_id` PK · first\_name · last\_name · gender · date\_of\_birth · weight · height

Many-to-many through patient\_allergies

PATIENT\_ALLERGIES

`patient_id` PK, FK · `allergy_id` PK, FK

ALLERGIES

`allergy_id` PK · allergy\_name UNIQUE

DOCTORS

`doctor_id` PK · first\_name · last\_name · specialty

ADMISSIONS

`admission_id` PK · `patient_id` FK · `doctor_id` FK · admission\_date · discharge\_date

Each patient and doctor can be associated with multiple admissions. Patient-allergy associations use a composite primary key to prevent duplicate assignments.

Design decision: The five-table design is more extensible, while the three-table design is simpler for this exercise. The following SQL uses the normalized five-table design. Queries that need to identify patients with no recorded allergies use `NOT EXISTS`, rather than checking for a NULL value.

### Exercise 4. Implement the schema using SQL `CREATE TABLE`

sql
```
CREATE TABLE patients (
    patient_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    gender CHAR(1),
    date_of_birth DATE,
    weight DECIMAL(5,2),
    height DECIMAL(5,2),
    CHECK (gender IN ('M', 'F') OR gender IS NULL),
    CHECK (weight IS NULL OR weight > 0),
    CHECK (height IS NULL OR height > 0)
);

CREATE TABLE doctors (
    doctor_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    specialty VARCHAR(100)
);

CREATE TABLE admissions (
    admission_id INT PRIMARY KEY,
    patient_id INT NOT NULL,
    doctor_id INT NOT NULL,
    admission_date DATE NOT NULL,
    discharge_date DATE,
    FOREIGN KEY (patient_id)
        REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id)
        REFERENCES doctors(doctor_id),
    CHECK (
        discharge_date IS NULL
        OR discharge_date >= admission_date
    )
);

CREATE TABLE allergies (
    allergy_id INT PRIMARY KEY,
    allergy_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE patient_allergies (
    patient_id INT NOT NULL,
    allergy_id INT NOT NULL,
    PRIMARY KEY (patient_id, allergy_id),
    FOREIGN KEY (patient_id)
        REFERENCES patients(patient_id),
    FOREIGN KEY (allergy_id)
        REFERENCES allergies(allergy_id)
);
```

Why use a composite primary key in `patient_allergies`?

The pair `(patient_id, allergy_id)` uniquely identifies an association. The same allergy cannot accidentally be assigned twice to the same patient.

### Exercise 5. Insert sample data

The following statements populate all five tables and provide data for testing the queries.

sql
```
INSERT INTO patients
    (patient_id, first_name, last_name, gender,
     date_of_birth, weight, height)
VALUES
    (1, 'Alice', 'Tan', 'F', '2010-03-12', 80.0, 160.0),
    (2, 'Ben', 'Lim', 'M', '2010-08-22', 60.0, 170.0),
    (3, 'Chloe', 'Lee', 'F', '1990-05-01', 55.0, 165.0),
    (4, 'David', 'Ng', 'M', '1985-11-15', 95.0, 175.0);

INSERT INTO doctors
    (doctor_id, first_name, last_name, specialty)
VALUES
    (101, 'James', 'Wong', 'Cardiology'),
    (102, 'Sarah', 'Chen', 'General Medicine');

INSERT INTO admissions
    (admission_id, patient_id, doctor_id,
     admission_date, discharge_date)
VALUES
    (1001, 1, 101, '2024-01-10', '2024-01-10'),
    (1002, 2, 102, '2024-02-15', '2024-02-18'),
    (1003, 1, 101, '2025-03-01', '2025-03-03'),
    (1004, 3, 102, '2025-06-20', '2025-06-20');

INSERT INTO allergies (allergy_id, allergy_name)
VALUES
    (1, 'Penicillin'),
    (2, 'Latex');

INSERT INTO patient_allergies (patient_id, allergy_id)
VALUES
    (1, 1),
    (3, 2);
```

These are illustrative records, not data from the lecturer's original ERD.

## Part II — SQL query solutions

### Question 1. Display the first name, last name, and gender of male patients

sql
```
SELECT first_name, last_name, gender
FROM patients
WHERE gender = 'M';
```

Explanation: `WHERE` filters the rows to patients whose gender is `'M'`.

Expected sample results: Ben Lim and David Ng.

### Question 2. Display patients who have no recorded allergies (NULL)

If the original schema has a nullable `allergies` column, the direct answer is:

sql
```
SELECT first_name, last_name
FROM patients
WHERE allergies IS NULL;
```

Remember: use `IS NULL`, not `= NULL`. SQL uses three-valued logic, so ordinary equality comparisons do not correctly test for NULL.

For the normalized five-table design above, a patient has no allergy associations when no corresponding row exists:

sql
```
SELECT p.first_name, p.last_name
FROM patients AS p
WHERE NOT EXISTS (
    SELECT 1
    FROM patient_allergies AS pa
    WHERE pa.patient_id = p.patient_id
);
```

This distinction is important for exams: a NULL allergy field and an absent relationship are different database representations.

### Question 3. Update NULL allergy fields to `NKA`

For the original three-table schema:

sql
```
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;
```

This update applies only to rows where the allergy field is NULL. Existing values are preserved.

In the normalized five-table schema, there is no `patients.allergies` field to update. A patient with no allergy association should not automatically be assigned an allergy record named `NKA`; that would confuse an absence of recorded allergies with a real allergy.

### Question 4. Count patients born in 2010

sql
```
SELECT COUNT(*) AS patients_born_in_2010
FROM patients
WHERE date_of_birth >= '2010-01-01'
  AND date_of_birth <  '2011-01-01';
```

Why use a date range? It works even if the birth-date column is changed to a datetime type, and it avoids applying a function to the column in the filter.

With the sample data, the result is `2`.

### Question 5. Determine the total number of admissions

sql
```
SELECT COUNT(*) AS total_admissions
FROM admissions;
```

`COUNT(*)` counts all admission records, including records whose nullable fields contain NULL.

Sample result: `4`.

### Question 6. Retrieve admissions with admission and discharge on the same day

sql
```
SELECT *
FROM admissions
WHERE admission_date = discharge_date;
```

This returns admissions where both dates are equal. Admissions with a NULL discharge date are not included, because a comparison with NULL does not evaluate to true.

Sample result: admissions `1001` and `1004`.

### Question 7A. Count male and female patients in a single row

sql
```
SELECT
    SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END)
        AS male_patients,
    SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END)
        AS female_patients
FROM patients;
```

Concept: Conditional aggregation uses `CASE` to assign 1 to matching rows and 0 to all others, then `SUM` counts them.

Sample result:

| male\_patients | female\_patients |
| --- | --- |
| 2 | 2 |

This returns one row with two columns.

### Question 7B. Display every person who appears as a patient or doctor, with their role

sql
```
SELECT first_name, last_name, 'Patient' AS role
FROM patients

UNION ALL

SELECT first_name, last_name, 'Doctor' AS role
FROM doctors;
```

`UNION ALL` combines the two result sets and preserves duplicate rows. That is appropriate if the same person can appear in both roles and both roles should be displayed.

Use `UNION` instead when you specifically want to eliminate duplicate result rows. The role column means the same individual may correctly appear twice, once for each role.

### Question 8. Display weight, height, and Boolean `isObese`

The formula in the provided document is not readable in the extracted text. A common assumption is that obesity means BMI ≥ 30 kg/m².

BMI is calculated as:

$$
\text{BMI}=\frac{\text{weight in kg}}
{(\text{height in metres})^2}
$$

Since height is stored in centimeters:

$$
\text{BMI}=\frac{\text{weight}\times 10000}
{\text{height}_{cm}^{2}}
$$

SQL solution:

sql
```
SELECT
    patient_id,
    weight,
    height,
    CASE
        WHEN weight IS NOT NULL
         AND height IS NOT NULL
         AND height > 0
         AND weight * 10000 / (height * height) >= 30
        THEN 1
        ELSE 0
    END AS isObese
FROM patients;
```

The result is `1` when the assumed BMI criterion is satisfied and `0` otherwise.

For example, Alice weighs 80 kg and is 160 cm tall:

$$
\text{BMI}=\frac{80\times10000}{160^2}=31.25
$$

Therefore, her `isObese` value is `1`.

Exam caution: If the missing formula in your lecturer's diagram uses a different threshold or rule, use that exact formula instead. The example above is an assumption, not a recovered lecturer-provided rule.

### Question 9. Produce yearly admission totals for each doctor

Required columns:

- `doctor_id`
- `doctor_full_name`
- `specialty`
- `year`
- `total_admissions`

sql
```
SELECT
    d.doctor_id,
    CONCAT(d.first_name, ' ', d.last_name)
        AS doctor_full_name,
    d.specialty,
    YEAR(a.admission_date) AS year,
    COUNT(*) AS total_admissions
FROM doctors AS d
JOIN admissions AS a
    ON a.doctor_id = d.doctor_id
GROUP BY
    d.doctor_id,
    d.first_name,
    d.last_name,
    d.specialty,
    YEAR(a.admission_date)
ORDER BY
    d.doctor_id,
    year;
```

Explanation:

1. `JOIN` associates each admission with its doctor.
2. `YEAR()` extracts the year from the admission date.
3. `COUNT(*)` counts admissions in each doctor-year group.
4. `GROUP BY` produces one row per doctor and year.

Expected results for the sample data:

| doctor\_id | doctor\_full\_name | specialty | year | total\_admissions |
| --- | --- | --- | --- | --- |
| 101 | James Wong | Cardiology | 2024 | 1 |
| 101 | James Wong | Cardiology | 2025 | 1 |
| 102 | Sarah Chen | General Medicine | 2024 | 1 |
| 102 | Sarah Chen | General Medicine | 2025 | 1 |

This query omits doctors with no admissions. If the question requires every doctor, including those with zero admissions, use a `LEFT JOIN` and group accordingly.

## Part III — Additional exam-practice questions with solutions

These are new practice questions based on the exercises, not questions extracted from the lecturer. They are designed for closed-book exam preparation.

### Practice Question 1 — Database normalization (8 marks)

A hospital stores the following information in one table:

`Admission(admission_id, patient_id, patient_name, doctor_id, doctor_name, specialty, admission_date)`

Identify two normalization problems and propose a 3NF design.

Solution

Problems:

- Patient names are repeated across admissions, causing update anomalies.
- Doctor names and specialties are repeated across admissions, causing redundant data.

Improved design:

sql
```
CREATE TABLE patients (
    patient_id INT PRIMARY KEY,
    patient_name VARCHAR(100) NOT NULL
);

CREATE TABLE doctors (
    doctor_id INT PRIMARY KEY,
    doctor_name VARCHAR(100) NOT NULL,
    specialty VARCHAR(100)
);

CREATE TABLE admissions (
    admission_id INT PRIMARY KEY,
    patient_id INT NOT NULL,
    doctor_id INT NOT NULL,
    admission_date DATE NOT NULL,
    FOREIGN KEY (patient_id)
        REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id)
        REFERENCES doctors(doctor_id)
);
```

Marking guide:

- Identify redundant patient information: 2 marks.
- Identify redundant doctor information: 2 marks.
- Correctly separate entities: 2 marks.
- Define PKs and FKs: 2 marks.

### Practice Question 2 — Find patients with multiple admissions (6 marks)

Write an SQL query that displays each patient's ID, first name, last name, and number of admissions, but only for patients with more than one admission.

Solution

sql
```
SELECT
    p.patient_id,
    p.first_name,
    p.last_name,
    COUNT(a.admission_id) AS admission_count
FROM patients AS p
JOIN admissions AS a
    ON a.patient_id = p.patient_id
GROUP BY
    p.patient_id,
    p.first_name,
    p.last_name
HAVING COUNT(a.admission_id) > 1;
```

Key exam concept: `WHERE` filters individual rows before grouping; `HAVING` filters groups after aggregation.

### Practice Question 3 — Find doctors who have no admissions (5 marks)

Solution

sql
```
SELECT
    d.doctor_id,
    d.first_name,
    d.last_name
FROM doctors AS d
LEFT JOIN admissions AS a
    ON a.doctor_id = d.doctor_id
WHERE a.admission_id IS NULL;
```

Explanation: A `LEFT JOIN` retains every doctor. A NULL admission ID indicates that no matching admission exists.

An alternative is to use `NOT EXISTS`.

### Practice Question 4 — Calculate the average BMI of patients (6 marks)

Display each patient's ID and BMI, rounded to two decimal places.

Solution

sql
```
SELECT
    patient_id,
    ROUND(weight * 10000 / (height * height), 2) AS BMI
FROM patients
WHERE weight IS NOT NULL
  AND height IS NOT NULL
  AND height > 0;
```

For the average BMI of all patients with valid measurements:

sql
```
SELECT
    ROUND(
        AVG(weight * 10000 / (height * height)),
        2
    ) AS average_BMI
FROM patients
WHERE weight IS NOT NULL
  AND height IS NOT NULL
  AND height > 0;
```

Key exam concept: Convert centimeters into meters, or use the equivalent factor of 10,000 in the formula.

### Practice Question 5 — Count admissions per year (6 marks)

Display each year and its total number of admissions, but show only years with at least two admissions.

Solution

sql
```
SELECT
    YEAR(admission_date) AS admission_year,
    COUNT(*) AS total_admissions
FROM admissions
GROUP BY YEAR(admission_date)
HAVING COUNT(*) >= 2
ORDER BY admission_year;
```

The sample dataset produces no rows because neither year has two or more admissions.

### Practice Question 6 — Update patient information safely (5 marks)

A patient with ID `3` has an incorrect weight. Change it to `58.5` kg.

Solution

sql
```
UPDATE patients
SET weight = 58.5
WHERE patient_id = 3;
```

Important: Without the `WHERE` clause, every patient's weight would be updated.

### Practice Question 7 — Find admissions lasting more than three days (6 marks)

Display the admission ID, patient ID, admission date, discharge date, and length of stay in days.

Solution

sql
```
SELECT
    admission_id,
    patient_id,
    admission_date,
    discharge_date,
    DATEDIFF(discharge_date, admission_date)
        AS length_of_stay_days
FROM admissions
WHERE discharge_date IS NOT NULL
  AND DATEDIFF(discharge_date, admission_date) > 3;
```

Exam caution: `DATEDIFF()` measures the difference between dates. If the hospital counts both the admission day and discharge day as occupied calendar days, the calculation may need `DATEDIFF(...) + 1`.

### Practice Question 8 — Explain `UNION` versus `UNION ALL` (4 marks)

What is the difference between `UNION` and `UNION ALL`?

Solution

- `UNION` combines result sets and removes duplicate rows.
- `UNION ALL` combines result sets while preserving duplicate rows.
- `UNION ALL` is generally faster when duplicate elimination is unnecessary because it avoids the extra deduplication step.

Both queries require compatible column counts and compatible data types in corresponding columns.

### Practice Question 9 — Find patients without any admission (5 marks)

Solution

sql
```
SELECT
    p.patient_id,
    p.first_name,
    p.last_name
FROM patients AS p
WHERE NOT EXISTS (
    SELECT 1
    FROM admissions AS a
    WHERE a.patient_id = p.patient_id
);
```

Key exam concept: `NOT EXISTS` is a reliable way to express an anti-join: find rows in one table with no matching rows in another.

### Practice Question 10 — Compare lazy loading and write-through caching (4 marks)

Although this is a database exercise, caching was covered in the preceding chapter. Explain the difference between lazy loading and write-through caching.

Solution

| Lazy loading | Write-through |
| --- | --- |
| Data is loaded into the cache when requested and missing. | Data is written to the database and then the cache is updated. |
| Avoids caching data that is never requested. | Helps keep cached data up to date after writes. |
| Cache misses can increase response time. | Can cache data that is never subsequently requested. |

This question is useful for cumulative exam preparation, but it is an additional question, not an item found in this practice session.

## Part IV — Closed-book exam compression sheet

Memorize the following patterns because they cover most of the SQL skills tested in this session.

| Task | Essential pattern |
| --- | --- |
| Filter rows | `WHERE condition` |
| Test for NULL | `IS NULL` / `IS NOT NULL` |
| Count rows | `COUNT(*)` |
| Conditional counts | `SUM(CASE WHEN ... THEN 1 ELSE 0 END)` |
| Group and count | `GROUP BY ... COUNT(*)` |
| Filter groups | `HAVING` |
| Combine tables | `JOIN ... ON ...` |
| Keep unmatched left rows | `LEFT JOIN` |
| Find no matching rows | `NOT EXISTS` |
| Combine result sets | `UNION` / `UNION ALL` |
| Update selected rows | `UPDATE ... SET ... WHERE ...` |
| Calculate yearly totals | `YEAR(date_column)` + `GROUP BY` |
| Calculate BMI | `weight * 10000 / (height * height)` |

### Common exam mistakes to avoid

1. Writing `= NULL` instead of `IS NULL`.
2. Using `WHERE COUNT(*) > 1` instead of `HAVING COUNT(*) > 1`.
3. Forgetting the `ON` condition when joining tables.
4. Grouping by an incomplete set of non-aggregated columns.
5. Forgetting that `COUNT(column)` excludes NULL values, whereas `COUNT(*)` counts rows.
6. Using `UPDATE` without a restrictive `WHERE` clause.
7. Treating a patient without a recorded allergy as necessarily having a confirmed absence of allergies.
8. Forgetting to convert height from centimeters to meters when calculating BMI.

Lecturer review-question extraction: No explicit lecturer review questions were found in the supplied Practice Session 2 material. The tasks and questions above are the supplied exercises and newly created exam practice, respectively; they are kept separate.
