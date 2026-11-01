# MongoDB — 60 Multiple-Choice Questions (MCQ)

**Topic:** MongoDB theory, system architecture, and e-commerce hands-on practice  
**Format:** GitHub-flavoured Markdown  
**Environment for practical questions:** MongoDB Shell (`mongosh`)  
**Question count:** 60  
**Structure:** Part A — Theory and System Architecture (30); Part B — E-commerce Lab Solutions (30)

**Instructions:** Select the **one best answer** for each question. The correct answer and an explanation are provided immediately after each question for revision.

---

# Part A — MongoDB Theory and System Architecture (30 MCQs)

## Q1. What type of database is MongoDB?

A. A relational database that stores data only in tables  
B. A document-oriented NoSQL database  
C. A graph database that stores only nodes and edges  
D. A key-value database that stores only strings  

**Correct answer: B**

**Explanation:** MongoDB is a document-oriented NoSQL database. It stores records as BSON documents grouped into collections rather than requiring data to be stored exclusively in relational tables.

## Q2. What is the principal data structure used to store a record in MongoDB?

A. Row  
B. Tuple  
C. Document  
D. Worksheet  

**Correct answer: C**

**Explanation:** A MongoDB record is a document made up of field-value pairs. Documents can contain nested documents and arrays.

## Q3. What is BSON?

A. A binary representation of JSON-like documents  
B. A query language used only for relational joins  
C. A text-only format that cannot represent dates  
D. A MongoDB replication protocol  

**Correct answer: A**

**Explanation:** BSON means Binary JSON. It is a binary-encoded serialization format used by MongoDB to store documents. BSON supports data types such as dates, ObjectIds, and numeric types.

## Q4. Which statement best describes a MongoDB collection?

A. It is equivalent to a database server  
B. It is a grouping of documents  
C. It is a single field inside a document  
D. It is a transaction log  

**Correct answer: B**

**Explanation:** A collection stores a group of documents. Documents in a collection can have different fields and structures, although maintaining a consistent schema is often beneficial.

## Q5. Which field is normally present in every MongoDB document?

A. `id`  
B. `primaryKey`  
C. `_id`  
D. `rowNumber`  

**Correct answer: C**

**Explanation:** MongoDB documents have an `_id` field that uniquely identifies each document within its collection. If an inserted document does not supply `_id`, MongoDB normally generates an ObjectId.

## Q6. What is the main meaning of MongoDB's flexible schema?

A. Documents in a collection must all have exactly the same fields  
B. A collection can contain documents with different fields or structures  
C. MongoDB does not support data types  
D. MongoDB automatically converts every field into a string  

**Correct answer: B**

**Explanation:** MongoDB does not require every document in a collection to have an identical structure. Schema validation can nevertheless be used to enforce rules where consistency is required.

## Q7. Which design is an example of embedding in MongoDB?

A. Storing a customer's address as a nested document inside the customer document  
B. Storing only the address document's identifier in another collection  
C. Storing every field in a separate database  
D. Creating an index on the address field  

**Correct answer: A**

**Explanation:** Embedding places related data inside a parent document. It can be useful when related information is commonly read or updated together and its size and growth are manageable.

## Q8. Which situation is a strong reason to use references rather than embedding all related data?

A. The related data is small and always read with the parent  
B. The related entity is shared by many other documents and maintained independently  
C. The data must be stored as an array  
D. The application never needs to identify related records  

**Correct answer: B**

**Explanation:** References store an identifier pointing to a document elsewhere. They are useful for shared entities, independently managed data, or relationships where embedding would create excessive duplication or document growth.

## Q9. What is the role of a replica set in MongoDB?

A. To divide one collection into unrelated tables  
B. To provide redundancy and high availability through multiple copies of data  
C. To convert BSON documents into SQL rows  
D. To replace the need for indexes  

**Correct answer: B**

**Explanation:** A replica set is a group of MongoDB servers that maintain the same data set. It typically has one primary and one or more secondary members, supporting redundancy and automatic failover.

## Q10. In a typical replica set, which member receives writes from clients by default?

A. A secondary  
B. The primary  
C. Every member independently  
D. The arbiter only  

**Correct answer: B**

**Explanation:** The primary receives write operations in the usual replica-set configuration. The primary records changes in the operation log, and secondary members replicate those changes.

## Q11. What is the main function of a secondary replica-set member?

A. To maintain a replicated copy of data and potentially serve eligible reads  
B. To act as the only member allowed to accept writes  
C. To store only index definitions  
D. To route requests between shards  

**Correct answer: A**

**Explanation:** A secondary applies replicated operations from the primary and maintains a copy of the data. Whether clients can read from secondaries depends on the configured read preference and application requirements.

## Q12. What is the MongoDB oplog?

A. A list of collection names  
B. A log of operations used by replica-set members to replicate changes  
C. A file containing only user passwords  
D. A report produced by `explain()`  

**Correct answer: B**

**Explanation:** The operation log, or oplog, records data-changing operations on the primary. Secondary members use it to replicate changes and catch up with the replica set.

## Q13. What generally happens if a replica-set primary becomes unavailable?

A. The replica set permanently deletes all data  
B. Eligible members can hold an election to choose a new primary  
C. Every secondary becomes a separate independent database automatically  
D. The oplog is converted into a collection  

**Correct answer: B**

**Explanation:** The replica set can elect a new primary from eligible members, provided the election requirements are met. This supports automatic failover, although a brief interruption to writes can occur during the election.

## Q14. What is an arbiter in a MongoDB replica set?

A. A member that stores a full copy of the data  
B. A member that participates in elections but does not store the data set  
C. A query router for a sharded cluster  
D. A special type of collection  

**Correct answer: B**

**Explanation:** An arbiter votes in replica-set elections but does not hold a copy of the data. It contributes a vote, not data redundancy, and should not be confused with a data-bearing secondary.

## Q15. What is sharding primarily used for?

A. Encrypting documents  
B. Distributing data across multiple machines to support horizontal scaling  
C. Replacing BSON with JSON  
D. Automatically correcting invalid documents  

**Correct answer: B**

**Explanation:** Sharding partitions and distributes data across multiple servers (shards). It can increase storage capacity and distribute workload as data and traffic grow.

## Q16. What is a shard in a MongoDB sharded cluster?

A. A server or replica set that stores part of the sharded data  
B. A field projection  
C. A type of aggregation accumulator  
D. A document identifier  

**Correct answer: A**

**Explanation:** Each shard stores a portion of the cluster's sharded data. In production deployments, a shard is commonly deployed as a replica set to provide redundancy.

## Q17. What is the role of `mongos` in a sharded cluster?

A. It stores all data instead of the shards  
B. It acts as a query router between clients and the sharded cluster  
C. It elects the primary of every replica set  
D. It creates BSON data types  

**Correct answer: B**

**Explanation:** `mongos` routes client operations to the appropriate shard or shards using cluster metadata. It presents the sharded cluster through a common interface to clients.

## Q18. What is the purpose of the config servers in a sharded cluster?

A. To store cluster metadata and configuration information  
B. To store only customer passwords  
C. To execute every query without contacting shards  
D. To replace the application server  

**Correct answer: A**

**Explanation:** Config servers store metadata about the sharded cluster, including information used to map data ranges or chunks to shards. They are deployed as a replica set in modern sharded-cluster architecture.

## Q19. What is a shard key?

A. The password used to connect to a shard  
B. The field or fields used to distribute documents across shards  
C. The `_id` field in every collection, regardless of configuration  
D. The name of the primary replica-set member  

**Correct answer: B**

**Explanation:** The shard key determines how documents are distributed across shards. Its choice affects data distribution and query targeting, so it should reflect the application's access patterns and growth.

## Q20. What is a potential problem with a poorly chosen shard key?

A. It can cause uneven data distribution or concentrated write traffic  
B. It automatically disables BSON  
C. It prevents documents from having an `_id` field  
D. It makes all queries transactional  

**Correct answer: A**

**Explanation:** A shard key with low cardinality, monotonically increasing values, or poor alignment with workload can create hotspots or uneven distribution. Shard-key design is therefore an important architectural decision.

## Q21. What is the primary purpose of an index in MongoDB?

A. To make every document larger  
B. To help the database find matching documents more efficiently  
C. To replace the collection  
D. To guarantee that every query returns results in insertion order  

**Correct answer: B**

**Explanation:** Indexes provide data structures that support efficient query execution. They can reduce the amount of collection data that MongoDB must scan, at the cost of storage and write-maintenance overhead.

## Q22. What is a compound index?

A. An index that includes more than one field  
B. An index that exists only in a replica-set arbiter  
C. An index that stores multiple databases  
D. An index that can be used only for text searches  

**Correct answer: A**

**Explanation:** A compound index includes multiple fields in a defined order. Field order matters because it affects which query and sort patterns the index can support efficiently.

## Q23. What does a collection scan (`COLLSCAN`) indicate in an explain plan?

A. MongoDB examined documents by scanning the collection  
B. MongoDB used a compound index  
C. MongoDB elected a new primary  
D. MongoDB distributed the query to every database  

**Correct answer: A**

**Explanation:** `COLLSCAN` indicates a collection scan. It is not automatically wrong for every query, but for a selective query on a large collection it may indicate that a suitable index could improve performance.

## Q24. What does `IXSCAN` indicate in a MongoDB query execution plan?

A. A full collection scan  
B. An index scan  
C. A transaction rollback  
D. A shard migration  

**Correct answer: B**

**Explanation:** `IXSCAN` indicates that the query plan is scanning an index. `explain("executionStats")` can help evaluate the selected plan and the amount of work performed.

## Q25. Which statement about MongoDB's WiredTiger storage engine is correct?

A. It is a storage engine used by MongoDB that supports features including document-level concurrency control and compression  
B. It is the query language used by `mongosh`  
C. It is the service that routes queries to shards  
D. It is a replacement for BSON  

**Correct answer: A**

**Explanation:** WiredTiger is MongoDB's commonly used storage engine. It provides storage and concurrency mechanisms and supports compression. Exact behavior and configuration depend on MongoDB version and deployment settings.

## Q26. What is the purpose of journaling in a storage engine?

A. To help recover committed changes after an unexpected failure  
B. To create customer documents automatically  
C. To distribute documents between shards  
D. To convert all writes into read operations  

**Correct answer: A**

**Explanation:** Journaling records information that helps the storage engine recover after a crash or unexpected shutdown. It is part of durability and recovery mechanisms, alongside MongoDB's write-concern behavior.

## Q27. What does write concern control?

A. The acknowledgement level requested for a write operation  
B. The fields returned by a projection  
C. The order of aggregation stages  
D. The number of documents embedded in an array  

**Correct answer: A**

**Explanation:** Write concern specifies the level of acknowledgement requested for write operations, such as acknowledgement by the primary or by a specified number of replica-set members. Stronger acknowledgement can improve durability guarantees but may affect latency and availability.

## Q28. What does read preference determine in a replica set?

A. Which replica-set members may be used to satisfy read operations  
B. Which fields are excluded from an update  
C. Whether an index is unique  
D. Whether BSON supports dates  

**Correct answer: A**

**Explanation:** Read preference controls where reads are directed, for example to the primary or to eligible secondary members. Reading from secondaries can distribute read load, but replicated data may lag behind the primary.

## Q29. Which statement best describes a MongoDB multi-document transaction?

A. It groups multiple operations so they can commit or abort as a unit  
B. It automatically joins every collection in a database  
C. It permanently prevents concurrent reads  
D. It is required for every single-document update  

**Correct answer: A**

**Explanation:** Multi-document transactions provide ACID transaction semantics across multiple documents and, in supported deployments, across collections. They require a suitable deployment such as a replica set or sharded cluster; a standalone server does not support multi-document transactions.

## Q30. Which statement best describes the relationship between replication and sharding?

A. Replication provides copies of data for redundancy; sharding distributes data across machines  
B. Replication and sharding are identical mechanisms  
C. Sharding creates backups, while replication only creates indexes  
D. Replication is used only for aggregation, while sharding is used only for inserts  

**Correct answer: A**

**Explanation:** Replication maintains multiple copies of data and supports availability. Sharding partitions data across machines to distribute storage and workload. A sharded cluster commonly uses replica sets for its individual shards, combining both approaches.

---

# Part B — MongoDB E-Commerce Lab Solutions (30 MCQs)

The following questions are based on the supplied T5 e-commerce lab: the `ecommerce` database, `products`, `customers`, and `orders` collections, the sample fields and documents, and the supplied `mongosh` operations and aggregation pipelines.

## Q31. What does the following command do?

```javascript
use ecommerce
```

A. Immediately creates and permanently stores an empty database  
B. Switches the current shell context to the `ecommerce` database  
C. Creates the `products` collection and inserts a document  
D. Deletes the current database and replaces it  

**Correct answer: B**

**Explanation:** `use ecommerce` selects the database context. MongoDB creates the database when data is first written to it; selecting it alone does not necessarily persist a new database.

## Q32. Which command inserts multiple product documents into the `products` collection?

A. `db.products.findMany([...])`  
B. `db.products.insertMany([...])`  
C. `db.products.updateMany([...])`  
D. `db.products.aggregate([...])`  

**Correct answer: B**

**Explanation:** `insertMany()` inserts multiple documents into a collection. In the lab, it is used to add the 11 sample products with fields such as `name`, `category`, `price`, `stock`, `brand`, and `rating`.

## Q33. Which command retrieves all documents from the `products` collection?

A. `db.products.find()`  
B. `db.products.insertMany()`  
C. `db.products.deleteMany()`  
D. `db.products.createIndex()`  

**Correct answer: A**

**Explanation:** `find()` without a filter returns all documents in the collection. The lab uses `db.products.find()` to verify the inserted products.

## Q34. Which query finds products whose category is exactly `"Electronics"`?

A. `db.products.find({ category: "Electronics" })`  
B. `db.products.find({ category: { $gt: "Electronics" } })`  
C. `db.products.updateMany({ category: "Electronics" })`  
D. `db.products.aggregate({ category: "Electronics" })`  

**Correct answer: A**

**Explanation:** A filter document with `category: "Electronics"` matches documents whose category field equals that value.

## Q35. Which query finds products with a price strictly greater than 500?

A. `db.products.find({ price: { $gte: 500 } })`  
B. `db.products.find({ price: { $gt: 500 } })`  
C. `db.products.find({ price: { $lt: 500 } })`  
D. `db.products.find({ price: 500 })`  

**Correct answer: B**

**Explanation:** `$gt` means strictly greater than. `$gte` means greater than or equal to, so it would also include a product priced exactly at 500.

## Q36. In the following query, what is the purpose of `_id: 0`?

```javascript
db.products.find(
  {},
  { _id: 0, name: 1, price: 1 }
)
```

A. It filters out products with an empty name  
B. It excludes `_id` from the returned projection  
C. It sets every product's `_id` to zero  
D. It sorts products by identifier  

**Correct answer: B**

**Explanation:** The second argument to `find()` is the projection. `name: 1` and `price: 1` include those fields, while `_id: 0` excludes the identifier. In a normal inclusion projection, `_id` is the permitted exception to mixing inclusion and exclusion.

## Q37. Which update increases the price of every Gaming product by 10%?

A. `db.products.updateMany({ category: "Gaming" }, { $inc: { price: 10 } })`  
B. `db.products.updateMany({ category: "Gaming" }, { $set: { price: 1.10 } })`  
C. `db.products.updateMany({ category: "Gaming" }, { $mul: { price: 1.10 } })`  
D. `db.products.updateMany({ category: "Gaming" }, { $gt: { price: 1.10 } })`  

**Correct answer: C**

**Explanation:** `$mul` multiplies the existing numeric value. Multiplying by `1.10` increases a price by 10%. `$inc` adds a fixed amount; it does not apply a percentage.

## Q38. Which statement correctly describes `$set`, `$inc`, and `$mul`?

A. `$set` assigns a value, `$inc` adds a value, and `$mul` multiplies a value  
B. `$set` sorts, `$inc` filters, and `$mul` deletes  
C. All three operators perform exactly the same operation  
D. `$inc` and `$mul` can operate only on strings  

**Correct answer: A**

**Explanation:** `$set` assigns a field value, `$inc` adds a specified amount to a numeric field, and `$mul` multiplies a numeric field by a specified factor. These distinctions are central to the lab's update exercises.

## Q39. Which update marks products with fewer than five units in stock as low stock?

A. `db.products.updateMany({ stock: { $lt: 5 } }, { $set: { lowStock: true } })`  
B. `db.products.updateMany({ stock: { $gt: 5 } }, { $set: { lowStock: false } })`  
C. `db.products.find({ stock: { $lt: 5 } }, { $set: { lowStock: true } })`  
D. `db.products.updateOne({ stock: 5 }, { $mul: { lowStock: true } })`  

**Correct answer: A**

**Explanation:** `$lt: 5` selects stock values strictly below five, and `$set` adds or updates the `lowStock` field. This operation does not automatically reset the flag for products whose stock later rises.

## Q40. Why does the lab use `ISODate()` for `registrationDate`?

A. It stores the date as a BSON Date value that supports date comparisons and date aggregation  
B. It converts the date into a product identifier  
C. It stores the date only as an untyped text string  
D. It automatically sorts customers by loyalty points  

**Correct answer: A**

**Explanation:** `ISODate()` creates a BSON Date value. BSON dates support date-range filtering and date aggregation operators more reliably than ordinary strings.

## Q41. Which filter finds customers with more than 1,000 loyalty points?

A. `{ loyaltyPoints: { $gte: 1000 } }`  
B. `{ loyaltyPoints: { $gt: 1000 } }`  
C. `{ loyaltyPoints: { $lt: 1000 } }`  
D. `{ loyaltyPoints: "1000" }`  

**Correct answer: B**

**Explanation:** `$gt: 1000` selects values strictly greater than 1,000. A customer with exactly 1,000 points would not match.

## Q42. Which operation marks all customers from Spain as premium customers?

A. `db.customers.updateMany({ country: "Spain" }, { $set: { premiumCustomer: true } })`  
B. `db.customers.find({ country: "Spain" }, { $set: { premiumCustomer: true } })`  
C. `db.customers.updateOne({ premiumCustomer: true }, { $inc: { country: "Spain" } })`  
D. `db.customers.insertMany({ country: "Spain", premiumCustomer: true })`  

**Correct answer: A**

**Explanation:** `updateMany()` applies the update to every document matching the country filter. `$set` creates `premiumCustomer` if it does not already exist.

## Q43. In the current-year registration query, why is the date range defined with `$gte: yearStart` and `$lt: nextYearStart`?

A. It creates a half-open interval that includes the start of the year and excludes the next year  
B. It selects only customers registered exactly at midnight  
C. It compares dates alphabetically  
D. It includes all dates after the next year begins  

**Correct answer: A**

**Explanation:** The half-open interval `[yearStart, nextYearStart)` includes the start boundary and excludes the following year's start. This avoids needing to calculate a final timestamp at the end of the year.

## Q44. In the lab's order documents, what does `customerId` contain?

A. The customer's email address  
B. A reference to the `_id` of a document in `customers`  
C. A copy of every field from the customer document  
D. The customer's country code  

**Correct answer: B**

**Explanation:** The lab retrieves a customer document and stores its `_id` in the order's `customerId` field. This is a reference pattern connecting an order to its customer.

## Q45. Why are product details such as `name`, `category`, and `unitPrice` embedded in each order's `products` array?

A. To preserve a snapshot of the purchased product details, including the price at purchase time  
B. To make it impossible to update the product catalogue  
C. To remove the need for an order identifier  
D. To ensure that every order contains exactly one product  

**Correct answer: A**

**Explanation:** The order embeds line-item details so historical reporting can use the name, category, and unit price recorded at purchase time. A later change to the product catalogue price should not rewrite the historical sale price.

## Q46. What is the total amount of Alice's sample order containing one Laptop Pro at 1299 and two Wireless Mice at 35 each?

A. 1334  
B. 1369  
C. 1399  
D. 1299  

**Correct answer: B**

**Explanation:** The total is `1299 + (2 × 35) = 1369`. The lab stores this value as `totalAmount` in Alice's sample order.

## Q47. Which query finds all Pending orders?

A. `db.orders.find({ status: "Pending" })`  
B. `db.orders.updateMany({ status: "Pending" })`  
C. `db.orders.find({ status: { $gt: "Pending" } })`  
D. `db.orders.aggregate({ status: "Pending" })`  

**Correct answer: A**

**Explanation:** `find()` with the status equality filter retrieves orders whose `status` is `"Pending"`.

## Q48. In the explicit order-status update, why does the update filter include both `_id` and `status: "Pending"`?

A. To make sure the selected order still has the expected status before changing it  
B. To update every order in the collection  
C. To create a new customer  
D. To increase the order's total amount  

**Correct answer: A**

**Explanation:** Including the expected previous status helps avoid changing an order that has already moved to another state. The `_id` identifies the specific order, while the status condition acts as a guard.

## Q49. In the customer order-count aggregation, what does the first `$group` stage calculate?

A. Revenue for each product category  
B. The number of orders for each `customerId`  
C. The number of products in the catalogue  
D. The average loyalty points by country  

**Correct answer: B**

**Explanation:** The stage groups orders by `customerId` and uses `$sum: 1` to count the number of order documents for each customer.

## Q50. In that same pipeline, what is the purpose of `$lookup` after grouping and matching?

A. To retrieve matching customer documents from the `customers` collection  
B. To multiply order totals  
C. To remove customers with more than five orders  
D. To convert the order count into a date  

**Correct answer: A**

**Explanation:** `$lookup` joins the grouped customer ID to the `_id` field in `customers`, placing matching customer documents in an array named `customer`.

## Q51. Why does the customer order-count pipeline use `$unwind: "$customer"`?

A. To expand the joined customer array into an individual customer document  
B. To delete customers who have no orders  
C. To sort customers alphabetically  
D. To split each customer's name into characters  

**Correct answer: A**

**Explanation:** `$lookup` returns an array. `$unwind` turns each array element into a separate pipeline document, making the customer fields directly accessible to later stages.

## Q52. In the category-revenue pipeline, why is `$unwind: "$products"` required?

A. Because each order may contain multiple product line items that must be processed individually  
B. Because `$group` cannot process numeric values  
C. Because it converts `totalAmount` into a date  
D. Because it removes the order status  

**Correct answer: A**

**Explanation:** Unwinding creates a pipeline document for each product line in an order. This allows the pipeline to calculate and group revenue by the category of each individual line item.

## Q53. Which expression calculates the revenue of one order product line using quantity and unit price?

A. `{ $sum: ["$products.quantity", "$products.unitPrice"] }`  
B. `{ $multiply: ["$products.quantity", "$products.unitPrice"] }`  
C. `{ $divide: ["$products.quantity", "$products.unitPrice"] }`  
D. `{ $concat: ["$products.quantity", "$products.unitPrice"] }`  

**Correct answer: B**

**Explanation:** Line revenue equals quantity multiplied by unit price. The pipeline then uses `$sum` in `$group` to add the line revenues for each category or product.

## Q54. In the top-five best-selling products pipeline, what does `unitsSold: { $sum: "$products.quantity" }` measure?

A. The total revenue for each product  
B. The total number of units sold for each product  
C. The number of customers in each country  
D. The current stock in the products collection  

**Correct answer: B**

**Explanation:** The expression adds the quantity values across the product lines grouped by product ID. The lab defines “best-selling” as the largest number of units sold, not the highest revenue.

## Q55. Why does the top-five best-selling pipeline sort by `unitsSold: -1` and then `productName: 1`?

A. To sort units sold from highest to lowest and use product name ascending as a tie-breaker  
B. To sort units sold from lowest to highest and product name descending  
C. To sort by revenue only  
D. To sort by the product's current stock  

**Correct answer: A**

**Explanation:** `-1` sorts descending and `1` sorts ascending. The second field gives a deterministic name-based tie-breaker when two products have the same units sold.

## Q56. What is the main purpose of `$facet` in the supplied dashboard pipeline?

A. To run multiple independent sub-pipelines over the same input and return their results in separate arrays  
B. To create a compound index  
C. To update all customer documents  
D. To split one order into product lines  

**Correct answer: A**

**Explanation:** `$facet` runs multiple sub-pipelines against the same input documents. In the lab, one branch calculates customer metrics and another looks up eligible orders to calculate order metrics.

## Q57. In the lab's `$lookup` example for displaying orders with customer details, what does `preserveNullAndEmptyArrays: true` do in `$unwind`?

A. It preserves an order even when the joined customer array is missing or empty  
B. It deletes orders with no matching customer  
C. It creates a customer document automatically  
D. It preserves only customers with premium status  

**Correct answer: A**

**Explanation:** With `preserveNullAndEmptyArrays: true`, documents are retained when the array is null, missing, or empty. This is useful when the pipeline should not discard an order merely because no matching customer was found.

## Q58. What query pattern does the lab's compound index `{ category: 1, price: 1 }` target?

A. Filtering by category and then applying a price range within that category  
B. Filtering only by an unrelated customer email  
C. Sorting orders by order date without filtering products  
D. Updating stock in every collection  

**Correct answer: A**

**Explanation:** The compound index places `category` first and `price` second. It is suited to the example query filtering on category and price. Because category is the leading field, the index may also support category-only queries; it is not generally as useful for a price-only query without category.

## Q59. What does the following command help verify?

```javascript
db.products.find({
  category: "Electronics",
  price: { $gt: 500 }
}).explain("executionStats")
```

A. The query's execution plan and execution statistics, including whether an index scan is used  
B. Whether every product has a rating above 4  
C. Whether the database contains exactly 11 products  
D. Whether the current user can create a new database  

**Correct answer: A**

**Explanation:** `explain("executionStats")` reports information about query planning and execution. The lab suggests looking for an index scan such as `IXSCAN` rather than assuming that the index will always be selected.

## Q60. Why does the inventory-and-order example use a transaction?

A. To ensure the stock decrement and order insertion commit together or are aborted together  
B. To guarantee that every product has the same price  
C. To make `find()` return documents in insertion order  
D. To replace the need for a customer reference  

**Correct answer: A**

**Explanation:** Creating an order and decreasing inventory are logically related operations. A transaction can ensure that both succeed or neither is committed. The supplied lab notes that multi-document transactions require a suitable deployment, such as a replica set or supported sharded cluster; a standalone server does not support them. The stock update also includes `stock: { $gte: quantity }` to prevent decrementing inventory when insufficient stock is available.

---

# Answer Key — Quick Revision

| Question | Answer | Question | Answer | Question | Answer |
|---:|:---:|---:|:---:|---:|:---:|
| 1 | B | 21 | B | 41 | B |
| 2 | C | 22 | A | 42 | A |
| 3 | A | 23 | A | 43 | A |
| 4 | B | 24 | B | 44 | B |
| 5 | C | 25 | A | 45 | A |
| 6 | B | 26 | A | 46 | B |
| 7 | A | 27 | A | 47 | A |
| 8 | B | 28 | A | 48 | A |
| 9 | B | 29 | A | 49 | B |
| 10 | B | 30 | A | 50 | A |
| 11 | A | 31 | B | 51 | A |
| 12 | B | 32 | B | 52 | A |
| 13 | B | 33 | A | 53 | B |
| 14 | B | 34 | A | 54 | B |
| 15 | B | 35 | B | 55 | A |
| 16 | A | 36 | B | 56 | A |
| 17 | B | 37 | C | 57 | A |
| 18 | A | 38 | A | 58 | A |
| 19 | B | 39 | A | 59 | A |
| 20 | A | 40 | A | 60 | A |

---

## Source and scope notes

- **Questions 1–30** cover MongoDB theory and common system-architecture concepts.
- **Questions 31–60** are based on the supplied T5 e-commerce hands-on solutions and their stated explanations.
- The practical questions preserve the lab's collection names, field names, operators, sample values, and business assumptions.
- The revenue examples use the lab's stated reporting rule: only `Shipped` and `Delivered` orders count as recognized sales revenue.
- The architecture section is general MongoDB study material; it is not represented as a quotation from the supplied lab or as lecturer-provided material.
- This is a revision resource, not a claim that these are the lecturer's actual exam questions.

**Suggested GitHub filename:** `t5-mongodb-60-mcq-theory-architecture-and-ecommerce-lab.md`
