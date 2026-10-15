# T3 — NoSQL Data Modeling: MongoDB (Chunk 3)

## Exam Compression Notes

### 1\. Insert Documents

MongoDB stores data as documents inside collections.

**Insert one document**

```
db.collectionName.insertOne({
  key1: value1,
  key2: value2
})
```

**Insert multiple documents**

```
db.collectionName.insertMany([
  { key1: value1, key2: value2 },
  { key1: value1, key2: value2 }
])
```

- `insertOne()` — inserts a single document.
- `insertMany()` — inserts multiple documents in one operation.
- These commands can create a collection if it does not already exist.

### 2\. Count Documents

```
db.collectionName.countDocuments()
```

Counts all documents in a collection.

To count only documents matching a condition:

```
db.collectionName.countDocuments({ year: { $gt: 2000 } })
```

### 3\. Retrieve Documents — `find()`

**Retrieve all documents**

```
db.collectionName.find()
```

**Format output for readability**

```
db.collectionName.find().pretty()
```

- `find()` without a filter returns all documents in the collection.
- `find(filter)` returns documents matching the specified filter.
- `pretty()` formats the displayed output; it does not change the stored data.

### 4\. Conditional Queries

**General syntax**

```
db.collectionName.find({
  key: { $operator: value }
})
```

Example: Find movies released after 2000.

```
db.movies.find({
  year: { $gt: 2000 }
})
```

Example: Find movies not directed by Christopher Nolan.

```
db.movies.find({
  director: { $ne: "Christopher Nolan" }
})
```

### 5\. Common Comparison Operators

| Operator | Meaning |
| --- | --- |
| `$gt` | Greater than |
| `$gte` | Greater than or equal to |
| `$lt` | Less than |
| `$lte` | Less than or equal to |
| `$eq` | Equal to |
| `$ne` | Not equal to |
| `$in` | Matches any value in a specified array |
| `$nin` | Does not match any value in a specified array |

Examples:

```
// Year greater than 2000
db.movies.find({ year: { $gt: 2000 } })

// Year at least 2000
db.movies.find({ year: { $gte: 2000 } })

// Director is not Christopher Nolan
db.movies.find({ director: { $ne: "Christopher Nolan" } })

// Year is 1990 or 2000
db.movies.find({ year: { $in: [1990, 2000] } })
```

**Exam trap:** `$gt` excludes the boundary; `$gte` includes it. Likewise, `$lt` excludes the boundary and `$lte` includes it.

### 6\. Logical Operators

Logical operators combine or negate query conditions.

| Operator | Meaning |
| --- | --- |
| `$and` | All conditions must be true |
| `$or` | At least one condition must be true |
| `$not` | Negates a condition |
| `$nor` | None of the specified conditions may be true |

**General syntax**

```
db.collectionName.find({
  $and: [
    { key1: { $operator1: value1 } },
    { key2: { $operator2: value2 } }
  ]
})
```

Example: Find movies released after 1990 and directed by The Wachowskis.

```
db.movies.find({
  $and: [
    { year: { $gt: 1990 } },
    { director: "The Wachowskis" }
  ]
})
```

Equivalent query using implicit AND:

```
db.movies.find({
  year: { $gt: 1990 },
  director: "The Wachowskis"
})
```

Example: Find movies released before 1990 or after 2000.

```
db.movies.find({
  $or: [
    { year: { $lt: 1990 } },
    { year: { $gt: 2000 } }
  ]
})
```

Example: Exclude movies with a year equal to 2000.

```
db.movies.find({
  year: { $not: { $eq: 2000 } }
})
```

### 7\. Quick Exam Recall

- **Insert one:** `insertOne()`
- **Insert many:** `insertMany()`
- **Count documents:** `countDocuments()`
- **Retrieve all:** `find()`
- **Improve display formatting:** `pretty()`
- **Compare values:** `$gt`, `$gte`, `$lt`, `$lte`, `$eq`, `$ne`
- **Match values in a list:** `$in`, `$nin`
- **Combine conditions:** `$and`, `$or`
- **Negate a condition:** `$not`
- **Require none of several conditions:** `$nor`

### 8\. Self-Test

1. Which method inserts several documents at once?
2. Which method counts documents in a collection?
3. What is the difference between `$gt` and `$gte`?
4. How do you retrieve all documents in a collection?
5. Which logical operator requires at least one condition to be true?
6. Write a query to find movies released after 2010.
7. Write a query to find movies released after 1990 and before 2020.
8. Write a query to find movies directed by someone other than Christopher Nolan.

**Answers**

1. `insertMany()`
2. `countDocuments()`
3. `$gt` excludes the boundary; `$gte` includes it.
4. `db.collectionName.find()`
5. `$or`
6. `db.movies.find({ year: { $gt: 2010 } })`
7. `db.movies.find({ year: { $gt: 1990, $lt: 2020 } })`
8. `db.movies.find({ director: { $ne: "Christopher Nolan" } })`

Scope note: These notes cover the readable material in Chunk 3. The source included embedded images whose contents were not available as text, so any additional operator examples present only in those images may be missing.
