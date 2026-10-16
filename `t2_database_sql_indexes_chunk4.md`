# Chunk 4 — Exam Compression: SQL, Database Indexes & B-Trees

## 1. Entity–Relationship (ER) diagrams

- ER diagram: visual representation of entities, their attributes, and relationships.
- Used to design the database structure before creating tables.
- ER models are translated into relational tables when implementing a relational database.

## 2. SQL — Creating tables and inserting data

SQL is used to define database structures, insert records, and query data.

### Essential SQL syntax

sql
```
-- Create a table
CREATE TABLE Movie (
    id INT PRIMARY KEY,
    title VARCHAR(100),
    description VARCHAR(255)
);

-- Insert data
INSERT INTO Movie (id, title, description)
VALUES (1, 'Example Movie', 'A drama film');

-- Retrieve all columns
SELECT *
FROM Movie;
```

### Filtering with `WHERE`

The `WHERE` clause selects only rows that satisfy a condition.

sql
```
SELECT *
FROM Movie
WHERE description LIKE '%drama%';
```

- `WHERE` filters rows according to a condition.
- `LIKE` performs pattern matching.
- `%` matches zero or more characters.
- Exact equality: `WHERE genre = 'drama'`.

Exam trap: If genre is stored in a dedicated column, filter using that column rather than searching the description.

## 3. SQL — Movie ratings exercises

Assume the database contains movie information and user ratings.

### Exercise 1: Find drama movies

Return all movies whose genre is drama.

sql
```
SELECT *
FROM Movie
WHERE genre = 'drama';
```

### Exercise 2: Find movies with ratings greater than 5

Return movie titles, descriptions, and ratings above 5.

sql
```
SELECT Movie.title,
       Movie.description,
       Rating.rating
FROM Movie
JOIN Rating ON Movie.id = Rating.movie_id
WHERE Rating.rating > 5;
```

- `SELECT`: specifies output columns.
- `JOIN ... ON`: combines related rows from tables.
- `WHERE`: filters rows before aggregation.

Assumption: `Rating.movie_id` references `Movie.id`. Adapt table and column names to the actual schema.

### Exercise 3: Compute the average rating

Use the aggregate function `AVG()`.

sql
```
SELECT Movie.title,
       AVG(Rating.rating) AS average_rating
FROM Movie
JOIN Rating ON Movie.id = Rating.movie_id
GROUP BY Movie.id, Movie.title;
```

Key concepts:

| SQL construct | Purpose |
| --- | --- |
| `AVG(column)` | Calculates the arithmetic mean |
| `COUNT(column)` | Counts non-NULL values |
| `SUM(column)` | Calculates a total |
| `GROUP BY` | Aggregates rows for each group |
| `WHERE` | Filters individual rows |
| `HAVING` | Filters groups after aggregation |
| `AS` | Gives a column an alias |

Important distinction: To calculate one average per movie, group by movie. To calculate one average over all selected ratings, omit `GROUP BY`.

If the question asks for the average of ratings that are greater than 5, filter before averaging:

sql
```
SELECT AVG(rating) AS average_rating
FROM Rating
WHERE rating > 5;
```

## 4. Database indexes

### Definition

An index is a data structure built from database data to locate records efficiently without scanning the entire table.

Think of a book's index: instead of reading every page, you use the index to find the relevant page directly.

### Why indexes matter

- Speed up read queries, especially searches and lookups.
- Can improve filtering, joins, and ordering when the index matches the query.
- Require storage space.
- Slow down inserts, updates, and deletes because indexes may need maintenance.
- Must be chosen according to typical query patterns; indexing every column is not always beneficial.

### Types of indexes

- B-tree / B+ tree: ordered structure; supports equality lookups, range queries, and ordered traversal.
- Hash index: efficient for equality lookups; generally unsuitable for range queries.
- LSM tree: organizes writes efficiently using in-memory and disk-based structures; commonly used in write-intensive systems.
- Sparse index: stores entries for selected keys or data blocks rather than every record, depending on the index design.
- Map index table: an index structure that maps keys to locations or records.

Exam takeaway: Index selection depends on the database engine, data distribution, and query patterns.

## 5. B-tree indexes

### Core properties

- A B-tree is a balanced search-tree structure designed for efficient searching, insertion, and deletion.
- Keys are kept ordered.
- All leaf nodes are at the same depth.
- Nodes store multiple keys and child pointers, giving the tree a high branching factor.
- Database indexes are organized into fixed-size pages/blocks, traditionally around 4 KB in some systems; actual page sizes vary.

B-trees reduce disk I/O because each page can hold multiple keys, reducing the number of levels traversed.

### Searching

1. Start at the root page.
2. Compare the target key with the keys in that page.
3. Follow the appropriate child pointer.
4. Repeat until the target is found or the relevant leaf is reached.

Search time is logarithmic in the number of indexed entries, approximately $O(\log n)$.

### Insertion

1. Find the leaf page whose key range contains the new key.
2. Insert the key in sorted order.
3. If the page has insufficient space, split it.
4. Update the parent to reflect the new key ranges.
5. Splits may propagate upward, potentially creating a new root.

### Updating

1. Search for the page containing the existing key.
2. Modify the stored value.
3. Write the modified page back to disk.
4. If the indexed key itself changes, the index may require additional structural changes.

### Deletion

- Remove the key from its relevant page.
- If a node becomes underfull, the tree may redistribute keys with a sibling or merge nodes.
- Parent pages are updated as necessary.
- The precise procedure depends on the B-tree variant and implementation.

## 6. B-tree vs. B+ tree

| Feature | B-tree | B+ tree |
| --- | --- | --- |
| Key storage | Keys/records may occur in internal or leaf nodes | Records or record pointers are generally stored at leaves |
| Search | May finish at an internal node | Typically proceeds to a leaf |
| Leaf linkage | Not required | Leaves are commonly linked |
| Range scans | Supported | Particularly efficient due to linked leaves |
| Database use | Used in some systems | Widely used for database indexes |

Exam trap: Do not claim all database indexes are B-trees. Many relational databases use B+ tree variants, while other index structures also exist.

## 7. Quick comparison: SQL vs. indexes

| Concept | Main purpose |
| --- | --- |
| `SELECT` | Retrieve data |
| `WHERE` | Filter rows |
| `JOIN` | Combine related tables |
| `AVG()` | Compute average |
| `GROUP BY` | Aggregate by category/entity |
| Index | Accelerate data retrieval |
| B-tree | Maintain ordered keys for efficient lookup |
| Page split | Create space when a B-tree page overflows |

## 8. Likely exam questions

Q1. Why do databases use indexes?

To reduce the amount of data scanned and accelerate queries, at the cost of storage and additional write overhead.

Q2. Why are B-trees balanced?

All leaves are at the same depth, keeping search paths approximately uniform.

Q3. What happens when a B-tree page is full during insertion?

The page may split, and the parent is updated. Splits can propagate upward.

Q4. What is the difference between `WHERE` and `HAVING`?

`WHERE` filters rows before grouping; `HAVING` filters aggregated groups.

Q5. How do you calculate average ratings per movie?

Use `AVG(rating)` with `GROUP BY` on the movie identifier and relevant identifying columns.

Q6. Why not index every column?

Indexes consume storage and increase the cost of inserts, updates, and deletes.

Q7. Which index is suited to range queries?

A B-tree/B+ tree is generally suitable because its keys are ordered. Hash indexes are primarily suited to equality lookups.

## Final 60-second revision

Memorize these points:

1. `WHERE` filters rows; `HAVING` filters groups.
2. `AVG()` computes an average; `GROUP BY` calculates separate aggregates for each group.
3. `JOIN` combines related tables using a matching condition.
4. Indexes accelerate reads but add storage and write costs.
5. B-trees are balanced, ordered, multiway search trees.
6. B-tree insertion may cause page splits and parent updates.
7. B+ trees typically store records or record pointers at leaves and support efficient range scans.
8. Indexes are chosen according to query patterns, not applied indiscriminately.
