## Important corrections and clarifications

### 1\. Mock Exam 2 — Q14: Regional sales totals

The supplied answer is correct.

| Region | Completed sales | Total amount |
| --- | --- | --- |
| North | 3 | 2,200 |
| South | 2 | 1,800 |

The cancelled South sale of 200 must be excluded before grouping.

### 2\. Mock Exam 3 — Q10: Regional revenue

The supplied answer is correct.

| Region | Completed orders | Total revenue | Average order amount |
| --- | --- | --- | --- |
| North | 4 | 3,000 | 750 |
| South | 2 | 2,000 | 1,000 |

Notice that South has the higher average order amount, but North has the higher total revenue. These metrics measure different things.

### 3\. Mock Exam 3 — Q11: Category revenue

The supplied answer contains a minor presentation issue: it labels Furniture as a result even though the question asks for categories with total revenue of at least 1,500.

The correct totals before applying the threshold are:

| Category | Completed revenue | Qualifies? |
| --- | --- | --- |
| Electronics | 3,900 | Yes |
| Furniture | 1,100 | No |

The final `$match` therefore returns Electronics only.

### 4\. MongoDB field existence versus null

Remember these distinctions:

javascript
```
// Field exists, including when its value is null
db.movies.find({
  rating: { $exists: true }
})

// Field exists and is not null
db.movies.find({
  rating: { $exists: true, $ne: null }
})
```

This distinction is particularly important in Mock Exams 1 and 2 and in Mock Exam 3, Q5–Q6.

### 5\. Aggregation stage order

A reliable rule for all three exams:

- `$match` before `$group` filters individual input documents.
- `$group` calculates aggregates.
- `$match` after `$group` filters the aggregate results.
- `$sort` establishes the desired ordering.
- `$limit` selects the first documents in that order.
- `$project` controls the fields in the output.

For example:

javascript
```
db.orders.aggregate([
  { $match: { status: "completed" } },
  {
    $group: {
      _id: "$region",
      totalRevenue: { $sum: "$amount" }
    }
  },
  { $match: { totalRevenue: { $gte: 1500 } } },
  { $sort: { totalRevenue: -1 } }
])
```

This calculates completed revenue by region, removes regions below the threshold, and sorts the remaining regions by revenue.

## Recommended final revision priorities

1. MongoDB query operators: `$gt`, `$gte`, `$in`, `$all`, `$size`, `$exists`, `$ne`, `$or`.
2. CRUD operations: `updateOne()` versus `updateMany()`, deletion safety, and upsert behavior.
3. Aggregation: `$match`, `$group`, `$sum`, `$avg`, `$project`, `$sort`, and `$limit`.
4. NoSQL architecture: document, key-value, wide-column, and graph database selection.
5. Distributed systems: ACID, BASE, CAP, eventual consistency, and horizontal scaling.
6. Data modeling: RDF triples, property graphs, and wide-column versus column-oriented storage.

Best exam strategy: Attempt each mock exam in 60 minutes without consulting the solutions, mark your answers using the supplied guides, and revisit every incorrect answer by explaining why the correct answer works—not just memorizing its syntax.
