# Chunk 6 — Exam Compression: NoSQL Data Modeling

## 1\. Key-Value Databases

**Definition:** The simplest type of NoSQL database; stores data as **key-value pairs**, similar to a Python dictionary.

- **Key:** Unique identifier used to access a value.
- **Value:** Can be an integer, string, JSON object, array, etc.
- **Advantages:**
  - Simple structure → fast data storage and retrieval.
  - Integrated caching enables quick access.
  - Well suited for unrelated or independent data.
- **Basic operations:**
  - `Insert(key, value)` — add data.
  - `Fetch(key)` — retrieve data.
  - `Update(key, value)` — modify data.
  - `Delete(key)` — remove data.

**Exam tip:** Key-value databases are best when data is accessed directly using a key.

## 2\. Column-Oriented Databases

**Definition:** Store data grouped by **columns rather than rows**.

- Use a **keyspace**, similar to a schema in relational databases.
- Relational databases generally store each row's data together; column-oriented databases store each column's data together.
- **Main purpose:** Improve read efficiency for analytical queries and data warehousing.
- Particularly effective for queries that:
  - Scan or aggregate large datasets.
  - Filter or aggregate table columns.
  - Require only a few columns from a large dataset.
- Only the required columns need to be read, reducing disk I/O and the amount of data processed.

### Row-oriented vs. Column-oriented storage

| Feature | Row-oriented | Column-oriented |
| --- | --- | --- |
| Storage | Values stored row by row | Values stored column by column |
| Best suited for | Accessing complete individual records | Analytics over selected columns |
| Typical use | Transactional queries | Data analytics and warehousing |
| Example query | Retrieve one user's entire record | Calculate balances across many users |

**Example:**

- **Query 1:** Retrieve `current_balance` for many users → column-oriented storage is advantageous because only that column is needed.
- **Query 2:** Retrieve a complete record for one user → row-oriented storage may be more efficient because the row's data is stored together.

**Exam tip:** Choose column-oriented storage for analytical queries that scan or aggregate a few columns across many records; choose row-oriented storage for queries that retrieve complete individual records.

## 3\. Key Elements of Column-Based Databases

### A. Column

- The basic unit of storage.
- Contains a **column name, value, and timestamp** indicating when the value was written.

### B. Column Family

- A group of related columns stored together.
- Enables efficient access because related columns can be read as a contiguous block.
- Functions like a logical table for organizing data.

### C. Row

- A collection of columns associated with a row key.
- Different rows do **not** need to contain the same set of columns.

### D. Row Key

- A unique identifier for each row within a column family.
- Used to locate and access a row.

### Quick memory aid

- **Column** = name + value + timestamp.
- **Column family** = group of related columns.
- **Row** = collection of columns associated with a key.
- **Row key** = unique row identifier.

## 4\. Likely Exam Questions

1. **What is a key-value database?**

A NoSQL database that stores values under keys for simple, fast retrieval.

2. **List the four basic key-value operations.**

Insert, Fetch, Update, Delete.

3. **What is a keyspace?**

A namespace or organizational structure in a column-oriented database, similar to a relational database schema.

4. **Why are column-oriented databases useful for analytics?**

They read only the required columns, reducing disk I/O and speeding up analytical queries.

5. **Differentiate row-oriented and column-oriented storage.**

Row-oriented storage groups values by record; column-oriented storage groups values by attribute.

6. **Define a column family.**

A group of related columns stored together for efficient access.

7. **What is a row key?**

The unique identifier used to locate a row within a column family.

8. **Must every row in a columnar database have the same columns?**

No. Rows can contain different sets of columns.

9. **What information does a column contain?**

A name, a value, and a timestamp.

10. **Which storage model is preferable for reading an entire individual record?**

Row-oriented storage is generally preferable.

## 5\. Last-Minute Revision

- **Key-value:** Key → Value; simple, fast, caching; Insert, Fetch, Update, Delete.
- **Column-oriented:** Stores by column; optimized for analytics and data warehousing.
- **Row-oriented:** Stores by row; suited to retrieving complete records.
- **Keyspace:** Similar to a relational schema.
- **Column:** Name + value + timestamp.
- **Column family:** Group of related columns.
- **Row:** Collection of columns linked to a row key.
- **Row key:** Unique row identifier.
- **Analytical query:** Read only the necessary columns to reduce I/O.
- **Flexible rows:** Different rows may have different columns.

**Important scope note:** The supplied chunk ends at the heading “NoSQL Data Modeling: Graphs.” Graph database details were not included in the provided material, so they are not expanded here.
