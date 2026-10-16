# Chunk 4 — Exam Compression

NoSQL Data Modeling: Document Database (MongoDB)

This is the high-yield exam summary of your Chunk 4. Focus on syntax, differences between methods, and the aggregation pipeline stages.

## 1. `sort()` — Sorting documents

- Sorts documents by one or multiple fields.
- `1` = ascending order (lowest to highest, A–Z, oldest to newest).
- `-1` = descending order (highest to lowest, Z–A, newest to oldest).
- Multiple fields can be sorted in sequence.

javascript
```
// Sort by release date ascending
db.movies_ack_ack.find().sort({ released: 1 })

// Genre ascending, then release date descending
db.movies_ack_ack.find().sort({ genre: 1, released: -1 })
```

Exam trap: Remember `1 = ascending` and `-1 = descending`.

## 2. `deleteOne()` vs `deleteMany()`

| Method | Purpose |
| --- | --- |
| `deleteOne(filter)` | Deletes only the first matching document |
| `deleteMany(filter)` | Deletes all matching documents |

Syntax:

javascript
```
db.collection.deleteOne(<filter>)
db.collection.deleteMany(<filter>)
```

Examples:

javascript
```
// Delete the first movie titled "The Matrix"
db.movies_ack_ack.deleteOne({ title: "The Matrix" })

// Delete all Comedy movies
db.movies_ack_ack.deleteMany({ genre: "Comedy" })
```

Exam trap: `deleteOne()` does not delete every match; `deleteMany()` does.

## 3. `aggregate()` — Aggregation pipeline

Definition: `aggregate()` processes documents through a sequence of pipeline stages. Each stage performs an operation and passes its result to the next stage.

General syntax:

javascript
```
db.collection.aggregate([
  { <stage1> },
  { <stage2> },
  { <stage3> }
])
```

### Five important pipeline stages

| Stage | Function |
| --- | --- |
| `$match` | Filters documents according to criteria |
| `$group` | Groups documents and calculates aggregate values |
| `$project` | Includes, excludes, or reshapes fields |
| `$sort` | Sorts documents |
| `$limit` | Restricts the number of documents passed onward |

Memory trick: Match → Group → Project → Sort → Limit.

Note: This is a memorization order, not a mandatory order for pipeline stages.

## 4. Important aggregation examples

### Example A — Count movies by genre

Goal: Find the number of movies in each genre.

javascript
```
db.movies_ack_ack.aggregate([
  {
    $group: {
      _id: "$genre",
      count: { $sum: 1 }
    }
  }
])
```

- `_id: "$genre"` groups movies by genre.
- `$sum: 1` counts each document in its group.
- `count` stores the calculated count.

Must remember: In `$group`, `_id` identifies the grouping key.

### Example B — Find the top 3 highest-rated Sci-Fi movies

javascript
```
db.movies_ack_ack.aggregate([
  { $match: { genre: "Sci-Fi" } },
  { $sort: { rating: -1 } },
  { $limit: 3 }
])
```

Pipeline logic:

1. `$match` selects Sci-Fi movies.
2. `$sort` puts the highest-rated movies first.
3. `$limit` returns only the first three results.

This assumes the rating field is named `rating`.

## 5. Final exam revision table

| Question | Answer to memorize |
| --- | --- |
| How to sort ascending? | `.sort({ field: 1 })` |
| How to sort descending? | `.sort({ field: -1 })` |
| Delete one matching document? | `deleteOne()` |
| Delete all matching documents? | `deleteMany()` |
| Apply sequential operations to documents? | `aggregate()` |
| Filter documents in a pipeline? | `$match` |
| Group documents? | `$group` |
| Count documents in a group? | `$sum: 1` |
| Reshape fields? | `$project` |
| Sort inside a pipeline? | `$sort` |
| Restrict results to N documents? | `$limit` |

## 6. Quick exam practice

## Test yourself

0/6 answered

1. Which value sorts documents in descending order?

0

1

-1

$sort

2. Which method deletes all documents matching a filter?

deleteOne()

deleteMany()

find()

sort()

3. Which aggregation stage filters documents?

$group

$project

$match

$limit

4. What does $sum: 1 commonly do inside $group?

Sorts documents

Counts documents in each group

Deletes documents

Limits results

5. Which stage limits output to three documents?

$limit: 3

$match: 3

$sum: 3

$project: 3

6. What does deleteOne() remove?

All matching documents

The first matching document

The entire collection

Only the oldest document

 Reset quiz

Last-minute priority: Memorize the `sort()` values, the difference between `deleteOne()` and `deleteMany()`, all five aggregation stages, and the two aggregation examples. These cover the main examinable concepts in the supplied chunk.
