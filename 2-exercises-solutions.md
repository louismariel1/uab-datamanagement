# Lecture Summary: Solving a lecture Mystery with SQL

 This lecture introduces **SQL and relational databases through a murder-mystery investigation**. The main idea is to use SQL to retrieve, filter, connect, and analyze data stored across multiple tables.

 ## 1\. Relational Databases

 A **relational database** stores information in **tables**, which consist of:

 - **Rows** — individual records.
- **Columns** — attributes describing each record.
- **Tables** — collections of related records.

 The investigation uses tables such as:

 - `person`
- `crime_scene_report`
- `interview`
- `drivers_license`
- `get_fit_now_member`
- `get_fit_now_check_in`

 The investigator's task is to determine which information is needed, find the appropriate table(s), write SQL queries, and interpret the results.

---

 ## 2\. Basic SQL: SELECT and FROM

 The basic structure for retrieving data is:

```
SELECT column_name
FROM table_name;
```

 To retrieve **all columns and all records**:

```
SELECT *
FROM person;
```

 `*` means **all columns**.

 The `person` table contains:

 - `id`
- `name`
- `license_id`
- `address_number`
- `address_street_name`
- `ssn`

 The database contains **10,011 records** in the `person` table.

---

 ## 3\. COUNT()

 `COUNT()` is an **aggregate function** used to count records.

 Example:

```
SELECT COUNT(*)
FROM person;
```

 It can be used to determine how many people or reports are stored in a table.

 Important aggregate functions include:

 | Function | Purpose |
| --- | --- |
| `COUNT()` | Counts records |
| `SUM()` | Calculates a total |
| `AVG()` | Calculates an average |
| `MIN()` | Finds the smallest value |
| `MAX()` | Finds the largest value |

Examples:

```
SELECT MIN(age)
FROM drivers_license;
```

```
SELECT AVG(age)
FROM drivers_license;
```

---

 ## 4\. WHERE: Filtering Records

 The `WHERE` clause restricts the records returned by a query.

 For example:

```
SELECT *
FROM crime_scene_report
WHERE city = "SQL City";
```

 This retrieves only crime reports from **SQL City**.

 Multiple conditions can be combined using `AND`:

```
SELECT *
FROM crime_scene_report
WHERE city = "SQL City"
AND type = "murder";
```

 This requires **both conditions** to be true.

 The murder report revealed that there were **two witnesses**:

 - The first lived at the **last house on Northwestern Dr**.
- The second was named **Annabel** and lived somewhere on **Franklin Ave**.

---

 ## 5\. LIKE and Wildcards

 Sometimes we don't know the complete value we are searching for.

 SQL provides `LIKE` for pattern matching.

 The `%` wildcard represents **zero or more characters**.

 Example:

```
SELECT *
FROM crime_scene_report
WHERE city LIKE "%City";
```

 This can match values ending in `"City"`, such as:

 - `SQL City`
- `Salt Lake City`

 For the Franklin Avenue investigation, we could use:

```
SELECT *
FROM person
WHERE address_street_name LIKE "%Franklin Ave%";
```

 The key distinction is:

 - `=` → exact match
- `LIKE` → pattern/partial match

---

 ## 6\. Numeric Comparisons and BETWEEN

 SQL can compare numerical values using operators such as:

 - `<` — less than
- `>` — greater than
- `<=` — less than or equal to
- `>=` — greater than or equal to

 `BETWEEN` can be used to search for values within a range.

 Example:

```
SELECT *
FROM drivers_license
WHERE age BETWEEN 20 AND 30;
```

 The lecture also demonstrates `BETWEEN` with text values:

```
SELECT DISTINCT city
FROM crime_scene_report
WHERE city BETWEEN 'W%' AND 'Z%';
```

---

 ## 7\. DISTINCT

 `DISTINCT` eliminates duplicate values from the result.

 Example:

```
SELECT DISTINCT city
FROM crime_scene_report;
```

 Instead of returning every crime report, this returns each city only once.

---

 ## 8\. ORDER BY and LIMIT

 `ORDER BY` sorts query results.

 Ascending order is the default:

```
SELECT *
FROM drivers_license
ORDER BY age;
```

 Descending order:

```
SELECT *
FROM drivers_license
ORDER BY age DESC;
```

 `LIMIT` restricts the number of records returned:

```
SELECT *
FROM drivers_license
ORDER BY age
LIMIT 10;
```

 This returns the **10 records with the lowest ages**.

 To find the oldest people, you could use:

```
SELECT *
FROM drivers_license
ORDER BY age DESC
LIMIT 10;
```

---

 # 9\. Joining Multiple Tables

 A major concept introduced in the lecture is the **JOIN**.

 Sometimes the information needed to answer a question is distributed across several tables.

 For example:

 - `person` contains the person's **name**.
- `drivers_license` contains the person's **age**.

 Neither table alone provides everything needed to answer:

 > Who is the oldest person?

 The tables can be connected using a common attribute.

 In this database:

```
person.license_id
        ↓
drivers_license.id
```

 The relationship is expressed as:

```
WHERE person.license_id = drivers_license.id;
```

 A complete query is:

```
SELECT person.name, drivers_license.age
FROM person, drivers_license
WHERE person.license_id = drivers_license.id;
```

 ### Three steps for constructing a join

 1. **Identify the information needed.**
2. **Identify the tables containing that information.**
3. **Identify the columns that relate the tables.**

 This is a very important problem-solving method for SQL.

---

 # 10\. Joining Person and Interview

 The `interview` table contains:

 - `person_id`
- `transcript`

 The task is to combine the `person` and `interview` tables so that we can retrieve:

 - the person's name
- their interview transcript

 The relationship is based on the person's identifier:

```
person.id ↔ interview.person_id
```

 A suitable query is:

```
SELECT person.name, interview.transcript
FROM person
JOIN interview
ON person.id = interview.person_id;
```

 This illustrates how a database can connect information stored in separate tables.

---

 # 11\. The Investigation Workflow

 The lecture teaches a general approach that can be applied to database investigations:

 **Question → Identify data → Identify table(s) → Write SQL → Execute → Interpret → Continue investigation**

 For example:

 1. Find the crime report.
2. Filter it to SQL City.
3. Filter it further to murder.
4. Read the description for clues.
5. Identify the witnesses.
6. Search the `person` table using their addresses.
7. Connect witnesses to their interviews.
8. Follow the clues into other tables.
9. Use `drivers_license`, `get_fit_now_member`, and `get_fit_now_check_in`.
10. Narrow down the suspects until the murderer is identified.

---

 # MCQ Practice Questions

 ### 1\. What does SQL stand for?

 A. Structured Query Language\
 B. Simple Question Language\
 C. System Query Logic\
 D. Structured Question Logic

 **Answer: A**

 **Explanation:** SQL stands for **Structured Query Language** and is used to interact with relational databases.

---

 ### 2\. What does a row in a database table generally represent?

 A. A database\
 B. A single record\
 C. A column name\
 D. A SQL command

 **Answer: B**

 **Explanation:** A row represents an individual **record**, while columns represent attributes of that record.

---

 ### 3\. Which query retrieves all columns from the `person` table?

 A.

```
GET ALL FROM person;
```

 B.

```
SELECT ALL person;
```

 C.

```
SELECT * FROM person;
```

 D.

```
SHOW person.*;
```

 **Answer: C**

 **Explanation:** `SELECT * FROM person;` retrieves every column and every record from `person`.

---

 ### 4\. How many records does the lecture state are in the `person` table?

 A. 1,001\
 B. 10,011\
 C. 11,010\
 D. 100,011

 **Answer: B**

 **Explanation:** The lecture states that the `person` table contains **10,011 records**.

---

 ### 5\. Which SQL function counts records?

 A. `SUM()`\
 B. `COUNT()`\
 C. `TOTAL()`\
 D. `NUMBER()`

 **Answer: B**

 **Explanation:** `COUNT()` counts records or values depending on how it is used.

---

 ### 6\. Which clause is used to filter records?

 A. `FILTER`\
 B. `ORDER BY`\
 C. `WHERE`\
 D. `LIMIT`

 **Answer: C**

 **Explanation:** `WHERE` specifies conditions that records must satisfy.

---

 ### 7\. Which query finds murder reports in SQL City?

 A.

```
SELECT *
FROM crime_scene_report
WHERE city = "SQL City"
AND type = "murder";
```

 B.

```
SELECT *
FROM crime_scene_report
WHERE city OR type;
```

 C.

```
SELECT murder
FROM SQL City;
```

 D.

```
SELECT *
FROM crime_scene_report
FILTER murder;
```

 **Answer: A**

 **Explanation:** The query uses `WHERE` and `AND` to require both `city = "SQL City"` and `type = "murder"`.

---

 ### 8\. What does the `%` wildcard represent in a `LIKE` pattern?

 A. Exactly one character\
 B. A number only\
 C. Zero or more characters\
 D. A space

 **Answer: C**

 **Explanation:** `%` can represent **any sequence of characters, including no characters**.

---

 ### 9\. Which operator should normally be used with wildcard patterns?

 A. `LIKE`\
 B. `=`\
 C. `MATCHES`\
 D. `SEARCH`

 **Answer: A**

 **Explanation:** SQL uses `LIKE` for pattern matching with wildcards such as `%`.

---

 ### 10\. Which query searches for people whose address contains Franklin Ave?

 A.

```
SELECT *
FROM person
WHERE address_street_name LIKE "%Franklin Ave%";
```

 B.

```
SELECT *
FROM person
WHERE address_street_name = "%Franklin Ave%";
```

 C.

```
SELECT *
FROM person
WHERE street = Franklin;
```

 D.

```
SEARCH person Franklin Ave;
```

 **Answer: A**

 **Explanation:** `LIKE` is required when using `%` as a wildcard.

---

 ### 11. What does `MIN()` return?

 A. The average value\
 B. The largest value\
 C. The smallest value\
 D. The number of records

 **Answer: C**

 **Explanation:** `MIN()` returns the smallest value in the selected column.

---

 ### 12\. What does `AVG()` calculate?

 A. The total\
 B. The average\
 C. The minimum\
 D. The maximum

 **Answer: B**

 **Explanation:** `AVG()` calculates the arithmetic average of a numeric column.

---

 ### 13\. Which function finds the largest value?

 A. `MAX()`\
 B. `HIGH()`\
 C. `TOP()`\
 D. `LARGE()`

 **Answer: A**

 **Explanation:** `MAX()` returns the largest value in a column.

---

 ### 14\. Which clause sorts query results?

 A. `SORT BY`\
 B. `ORDER BY`\
 C. `ARRANGE BY`\
 D. `GROUP BY`

 **Answer: B**

 **Explanation:** `ORDER BY` sorts the returned records.

---

 ### 15\. What is the default ordering of `ORDER BY`?

 A. Descending\
 B. Random\
 C. Ascending\
 D. Alphabetical only

 **Answer: C**

 **Explanation:** `ASC` is the default ordering.

---

 ### 16\. Which keyword sorts results from highest to lowest?

 A. `ASC`\
 B. `DESC`\
 C. `HIGH`\
 D. `REVERSE`

 **Answer: B**

 **Explanation:** `DESC` means descending order.

---

 ### 17\. What does `LIMIT 10` do?

 A. Deletes 10 records\
 B. Returns at most 10 records\
 C. Searches for the number 10\
 D. Sorts 10 columns

 **Answer: B**

 **Explanation:** `LIMIT` restricts the number of rows returned.

---

 ### 18\. Why are joins necessary?

 A. To delete duplicate tables\
 B. To combine related information from different tables\
 C. To increase the number of databases\
 D. To replace SQL

 **Answer: B**

 **Explanation:** A join connects related tables so information distributed across them can be queried together.

---

 ### 19\. Which columns connect `person` and `drivers_license` in the lecture?

 A. `person.id` and `drivers_license.age`\
 B. `person.name` and `drivers_license.gender`\
 C. `person.license_id` and `drivers_license.id`\
 D. `person.ssn` and `drivers_license.plate_num`

 **Answer: C**

 **Explanation:** The relationship is established through `person.license_id = drivers_license.id`.

---

 ### 20\. Which query correctly joins `person` and `drivers_license`?

 A.

```
SELECT person.name, drivers_license.age
FROM person, drivers_license
WHERE person.license_id = drivers_license.id;
```

 B.

```
SELECT person.name
FROM person
WHERE drivers_license.age;
```

 C.

```
JOIN person AND drivers_license;
```

 D.

```
SELECT person.name, drivers_license.age
WHERE person.name = drivers_license.age;
```

 **Answer: A**

 **Explanation:** The query selects the required columns and connects the tables through their related identifier columns.

---

 ### 21\. What column in the `interview` table identifies the interviewed person?

 A. `id`\
 B. `person_id`\
 C. `license_id`\
 D. `name`

 **Answer: B**

 **Explanation:** The `interview` table contains `person_id` and `transcript`.

---

 ### 22\. How would you connect `person` and `interview`?

 A.

```
person.id = interview.person_id
```

 B.

```
person.name = interview.transcript
```

 C.

```
person.ssn = interview.transcript
```

 D.

```
person.license_id = interview.person_id
```

 **Answer: A**

 **Explanation:** `person.id` identifies the person, while `interview.person_id` references that person.

---

 ### 23\. Which table contains witness statements?

 A. `person`\
 B. `drivers_license`\
 C. `interview`\
 D. `crime_scene_report`

 **Answer: C**

 **Explanation:** The witness statements are stored in the `interview` table in the `transcript` column.

---

 ### 24\. According to the murder report, where does the first witness live?

 A. Franklin Ave\
 B. Northwestern Dr\
 C. Banhall Ave\
 D. Wood Glade St

 **Answer: B**

 **Explanation:** The report states that the first witness lives at the **last house on Northwestern Dr**.

---

 ### 25\. What is known about the second witness?

 A. Their name is Christopher\
 B. They live on Northwestern Dr\
 C. Their name is Annabel and they live on Franklin Ave\
 D. They are a member of a gym

 **Answer: C**

 **Explanation:** The murder report identifies the second witness as **Annabel**, who lives somewhere on **Franklin Ave**.

---

 ### 26\. Which tables are specifically identified as relevant to the final murderer investigation?

 A. `person` only\
 B. `crime_scene_report` only\
 C. `person`, `drivers_license`, `get_fit_now_member`, and `get_fit_now_check_in`\
 D. `interview` only

 **Answer: C**

 **Explanation:** Exercise 8 directs the investigator to follow the evidence through these four tables.

---

 ### 27\. Which SQL operator checks whether a value falls within a specified range?

 A. `RANGE`\
 B. `BETWEEN`\
 C. `WITHIN`\
 D. `INRANGE`

 **Answer: B**

 **Explanation:** `BETWEEN` is used to search for values within a specified range.

---

 ### 28\. What is the main purpose of `DISTINCT`?

 A. Sort values\
 B. Remove duplicate values from the result\
 C. Delete duplicate records permanently\
 D. Count values

 **Answer: B**

 **Explanation:** `DISTINCT` returns unique values in the query result; it does not delete records from the database.

---

 ### 29\. What is the best general approach when solving a database investigation problem?

 A. Search every table manually\
 B. Guess the answer first\
 C. Identify the required information, locate the relevant table(s), write the query, and interpret the result\
 D. Delete irrelevant records

 **Answer: C**

 **Explanation:** The lecture emphasizes a systematic process of analyzing the question, identifying data sources, querying them, and interpreting the results.

---

 ### 30\. What is the central skill demonstrated by the murder mystery?

 A. Database deletion\
 B. Translating real-world questions into SQL queries\
 C. Programming a graphical interface\
 D. Creating databases from scratch

 **Answer: B**

 **Explanation:** The central learning objective is using SQL to translate investigative questions into queries that retrieve and analyze relevant data.

---

 ## Quick Exam Revision Sheet

 Remember these core patterns:

```
-- Retrieve everything
SELECT *
FROM table_name;
```

```
-- Filter
SELECT *
FROM table_name
WHERE condition;
```

```
-- Multiple conditions
SELECT *
FROM table_name
WHERE condition1
AND condition2;
```

```
-- Partial matching
SELECT *
FROM table_name
WHERE column_name LIKE "%value%";
```

```
-- Count
SELECT COUNT(*)
FROM table_name;
```

```
-- Aggregate functions
SELECT AVG(age), MIN(age), MAX(age)
FROM drivers_license;
```

```
-- Sort
SELECT *
FROM table_name
ORDER BY column_name DESC;
```

```
-- Limit results
SELECT *
FROM table_name
LIMIT 10;
```

```
-- Join
SELECT person.name, interview.transcript
FROM person
JOIN interview
ON person.id = interview.person_id;
```

 ### Most important concepts to memorize

 **SELECT** → what information you want\
 **FROM** → where the information comes from\
 **WHERE** → which records you want\
 **LIKE** → partial/pattern matching\
 **%** → zero or more characters\
 **AND** → both conditions must be true\
 **BETWEEN** → values within a range\
 **DISTINCT** → unique results\
 **COUNT()** → number of records\
 **AVG()** → average\
 **MIN()** → smallest\
 **MAX()** → largest\
 **SUM()** → total\
 **ORDER BY** → sort results\
 **ASC** → ascending\
 **DESC** → descending\
 **LIMIT** → restrict number of results\
 **JOIN** → combine related tables\
 **ON** → specifies how tables are related
