# Chunk 4 — NoSQL Data Modeling: MongoDB Exam Compression

## 1\. `$exists` — Check Field Presence

**Purpose:** Find documents based on whether a field exists, regardless of its value.

```
// Field exists (even if its value is null)
db.collection.find({ key: { $exists: true } })

// Field does not exist
db.collection.find({ key: { $exists: false } })
```

**Exam trap:** `$exists: true` matches a field whose value is `null`, provided the field is present.

## 2\. Array Query Operators

**General syntax:**

```
db.collection.find({
  key: { $operator: value }
})
```

| Operator | Purpose | Example |
| --- | --- | --- |
| `$in` | Matches if an array contains **at least one** specified value | `{ genres: { $in: ["Action", "Drama"] } }` |
| `$all` | Matches if an array contains **every** specified value | `{ genres: { $all: ["Action", "Drama"] } }` |
| `$size` | Matches arrays with an **exact number** of elements | `{ genres: { $size: 2 } }` |

**Memorize:**

- `$in` = ANY of the listed values.
- `$all` = ALL of the listed values.
- `$size` = exact array length.

Examples:

- Find movies with either Action or Drama: use `$in`.
- Find movies containing both Action and Drama: use `$all`.
- Find movies with exactly two genres: use `$size: 2`.

## 3\. `updateOne()` — Update One Document

**Purpose:** Updates the first document matching the filter.

**Syntax:**

```
db.collection.updateOne(
  { filterField: filterValue },
  { $set: { field: newValue } },
  { upsert: true } // Optional
)
```

- **Filter:** Identifies the document to update.
- **Update:** Specifies the changes, commonly using `$set`.
- **Options:** Optional settings, such as `upsert`.

Example — change the director of _The Matrix_:

```
db.movies.updateOne(
  { title: "The Matrix" },
  { $set: { director: "New Director" } }
)
```

### `upsert`

- `upsert: false` (default): If no document matches, no new document is inserted.
- `upsert: true`: If no document matches, MongoDB inserts a new document based on the filter and update.

**Exam trap:** `updateOne()` does not update every matching document. It updates at most one.

## 4\. `updateMany()` — Update Multiple Documents

**Purpose:** Updates **all documents** matching the filter.

**Syntax:**

```
db.collection.updateMany(
  { filterField: filterValue },
  { $set: { field: newValue } }
)
```

Example — change the genre from Adventure to Action for all matching movies:

```
db.movies.updateMany(
  { genre: "Adventure" },
  { $set: { genre: "Action" } }
)
```

- Matching documents are all updated.
- If no documents match, no documents are modified.
- `upsert: true` can insert a document if no match exists.

**Key difference:**

| Method | Number of documents updated |
| --- | --- |
| `updateOne()` | At most one matching document |
| `updateMany()` | All matching documents |

## 5\. `sort()` — Sort Query Results

**Purpose:** Orders documents by a specified field.

**Syntax:**

```
db.collection.find().sort({ field: 1 })   // Ascending
db.collection.find().sort({ field: -1 })  // Descending
```

- `1` = ascending order.
- `-1` = descending order.
- Can be used with `find()`; sorting can also be performed in aggregation pipelines using `$sort`.

Example:

```
// Movies sorted by year, oldest first
db.movies.find().sort({ year: 1 })

// Movies sorted by year, newest first
db.movies.find().sort({ year: -1 })
```

## Last-Minute Exam Recall

| Question clue | Correct answer |
| --- | --- |
| Check whether a field exists | `$exists: true` |
| Check whether a field is missing | `$exists: false` |
| Match any listed array value | `$in` |
| Require all listed array values | `$all` |
| Match an exact array length | `$size` |
| Update one matching document | `updateOne()` |
| Update all matching documents | `updateMany()` |
| Insert if no update match exists | `upsert: true` |
| Ascending sort | `1` |
| Descending sort | `-1` |

**Most important exam distinctions:** `$in` vs. `$all`, `$exists: true` vs. a non-null value, `updateOne()` vs. `updateMany()`, and ascending `1` vs. descending `-1`.
