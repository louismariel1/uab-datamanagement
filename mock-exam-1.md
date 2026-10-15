# Proposed 120-minute exam — Data Management (UAB 106566)

Universitat Autònoma de Barcelona · Bachelor's Degree in Artificial Intelligence · 2026/2027

This is a proposed practice exam, not an official UAB examination. It is based on the published course topics: data management fundamentals, data warehousing and OLAP, ETL, Redis, Apache Spark, DataFrames and MLlib.

The exam is designed to test conceptual understanding, practical problem-solving, data-system design and the ability to justify technical choices within two hours.

Duration

# 120 min

Total score

# 100 points

Questions

## 5 sections

Suggested pass mark

## 50/100

Suggested time allocation: 15 + 20 + 25 + 25 + 35 = 120 minutes.

## Exam paper

Instructions

- Answer all five questions.
- Explain your reasoning; unsupported answers may receive partial credit only.
- Write SQL and PySpark code where requested.
- You may state reasonable assumptions if a question is ambiguous.
- Unless specified otherwise, focus on correctness and design rather than syntax minutiae.

## Section 1 — Data management fundamentals

15 minutes12 points

### Question 1. Concepts and trade-offs

A. Short answers — 6 points

Answer each in 2–4 sentences.

1. Explain the difference between vertical scaling and horizontal scaling. Give one situation in which horizontal scaling is preferable. (2 points)
2. Explain the difference between a transactional database (OLTP) and an analytical data warehouse (OLAP). (2 points)
3. Define data partitioning and replication. Explain how each can help a distributed data system. (2 points)

B. System design — 6 points

An online shopping platform records 10 million product-view events daily. It needs to:

- Store events for several years.
- Generate daily sales and traffic reports.
- Display a user's recently viewed products in under 50 milliseconds.

Recommend a suitable storage or processing technology for each of the three requirements. Explain your choices and identify one trade-off for each.

## Section 2 — SQL and relational data

20 minutes18 points

### Question 2. Querying and data integrity

Consider these tables:

Customers

| customer\_id | name | country |
| --- | --- | --- |
| 1 | Ana | Spain |
| 2 | Ben | France |
| 3 | Carla | Spain |
| 4 | David | Italy |

Orders

| order\_id | customer\_id | amount | status |
| --- | --- | --- | --- |
| 101 | 1 | 120 | completed |
| 102 | 1 | 80 | completed |
| 103 | 2 | 200 | cancelled |
| 104 | 3 | 150 | completed |
| 105 | 3 | 50 | pending |

A. Write SQL queries — 12 points

1. Return each customer's name and total amount from completed orders, including customers with no completed orders. Show zero instead of `NULL` for the total. (4 points)
2. Return the countries whose completed-order revenue exceeds 200, sorted by revenue in descending order. (4 points)
3. Find customers who have never placed a completed order. (4 points)

B. Data integrity — 6 points

1. Identify a suitable primary key and foreign key in the schema. (2 points)
2. Explain what happens if an order references a nonexistent customer and referential integrity is enforced. (2 points)
3. Explain why `SUM(amount)` may produce an incorrect result if a query joins orders to a separate table containing multiple rows per order. Suggest a remedy. (2 points)

## Section 3 — Data warehousing, dimensional modelling and ETL

25 minutes22 points

### Question 3. Design an analytical data warehouse

A retailer operates online and physical stores. Management wants to analyse sales by product, store, customer, date and sales channel.

A. Dimensional modelling — 8 points

Design a star schema for this scenario.

1. Identify the central fact table and at least four dimension tables. (3 points)
2. Specify the grain of the fact table precisely. (2 points)
3. Propose at least four measures and identify which should be additive, semi-additive or non-additive, where relevant. (3 points)

B. OLAP operations — 6 points

Explain each operation and give a concrete example using your schema:

- Roll-up (2 points)
- Drill-down (2 points)
- Slice or dice (2 points)

C. ETL pipeline — 8 points

Sales data arrives daily from three systems. Some records contain missing product IDs, duplicated transactions and inconsistent date formats.

Describe an ETL pipeline that addresses all three problems. Your answer must explain:

1. How data is extracted and staged. (2 points)
2. How missing values, duplicates and dates are handled. (3 points)
3. How failed records are logged and how the pipeline avoids inserting the same transactions twice when rerun. (3 points)

## Section 4 — Redis and low-latency data access

25 minutes22 points

### Question 4. Implementing a fast product-view service

An e-commerce application uses Redis to store recently viewed products. Each user's list should contain at most five distinct product IDs, with the most recently viewed product first. Entries should expire after 24 hours without further activity.

A. Data structures — 6 points

1. Recommend a Redis data structure for this requirement and justify your choice. (2 points)
2. Explain why a simple string or set might not fully satisfy the ordering and uniqueness requirements on its own. (2 points)
3. Explain the purpose of a time-to-live (TTL). (2 points)

B. Redis commands — 8 points

Write Redis commands or a clearly described atomic operation to:

1. Add product `P42` to user `U7`'s recently viewed list at the front. (2 points)
2. Ensure `P42` appears only once. (2 points)
3. Keep only the first five entries. (2 points)
4. Refresh the key's 24-hour expiry. (2 points)

C. Reliability and scalability — 8 points

1. Explain why a sequence of separate Redis commands may cause race conditions when multiple requests update the same user's list concurrently. (3 points)
2. Suggest a way to make the update atomic. (2 points)
3. Explain one benefit and one limitation of using Redis as the only permanent store for customer activity. (3 points)

## Section 5 — Apache Spark and data processing

35 minutes26 points

### Question 5. PySpark analytics and machine learning

A retailer has a dataset containing these columns:

`transaction_id, customer_id, product_id, quantity, unit_price, transaction_date, country`

The data is too large to process efficiently on a single machine.

A. PySpark DataFrames — 10 points

Write PySpark code to:

1. Remove duplicate transactions using `transaction_id`. (2 points)
2. Create a `revenue` column equal to `quantity * unit_price`. (2 points)
3. Calculate total revenue by country, sorted from highest to lowest. (3 points)
4. Filter out rows with missing `customer_id` or invalid, non-positive quantities. (3 points)

You may assume the input DataFrame is called `transactions`.

B. Spark execution model — 6 points

1. Explain the difference between a transformation and an action in Spark, giving one example of each. (2 points)
2. What is lazy evaluation, and why can it improve execution efficiency? (2 points)
3. Explain the difference between a narrow transformation and a shuffle-inducing wide transformation, giving an example of each. (2 points)

C. Performance and machine learning — 10 points

1. A Spark job joins a very large transactions dataset with a small product-category table. Recommend a join optimisation and explain when it is appropriate. (3 points)
2. Explain how data skew can slow down a distributed Spark job and give one mitigation strategy. (3 points)
3. A retailer wants to predict whether a customer will make a purchase next week using customer activity features. Outline a basic Spark MLlib pipeline, identifying the target variable, features, train/test split and one suitable evaluation metric. (4 points)

## Marking scheme

| Section | Topic | Points |
| --- | --- | --- |
| 1 | Data management fundamentals | 12 |
| 2 | SQL and relational data | 18 |
| 3 | Data warehouse, OLAP and ETL | 22 |
| 4 | Redis and low-latency systems | 22 |
| 5 | Spark and MLlib | 26 |
| Total |  | 100 |

## What a strong answer should demonstrate

- SQL: correct joins, aggregation, handling of `NULL`, and filtering with `HAVING`.
- Data warehousing: a clearly defined fact-table grain, appropriate dimensions and valid ETL quality controls.
- Redis: correct use of lists, ordering, uniqueness, expiration and atomic updates.
- Spark: correct DataFrame operations, understanding of lazy evaluation and shuffles, and sensible performance optimisation.
- System design: justified technology choices that acknowledge trade-offs rather than simply naming tools.

## Suggested grading bands

| Score | Interpretation |
| --- | --- |
| 85–100 | Excellent — technically correct and well justified |
| 70–84 | Good — strong understanding with minor errors |
| 50–69 | Satisfactory — core concepts understood |
| Below 50 | More preparation needed |

These are suggested practice-exam bands, not verified UAB grading rules.

## Recommended next step

I can also prepare a complete model answer and marking rubric for all five sections, including executable SQL examples, Redis commands, PySpark solutions and explanations of the expected reasoning. That would let you use this as a timed mock exam and then mark your own answers.
