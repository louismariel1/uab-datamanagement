Below is a **comprehensive MCQ practice set covering the full Chunk 1–6 material**, including NoSQL fundamentals, ACID/BASE, CAP, all four NoSQL models, MongoDB CRUD and querying, column-family databases, graph databases, RDF, and property graphs.

 ## Part A — NoSQL Fundamentals

 ### 1\. What is the main characteristic of a traditional relational database?

 A. Flexible schema\
 B. Tables with rows and columns\
 C. Nodes and edges\
 D. Key-value pairs

 **Answer: B. Tables with rows and columns**

 **Explanation:** Relational databases organize structured data into tables consisting of rows and columns, usually with a predefined schema.

---

 ### 2\. Which characteristic is most strongly associated with NoSQL databases?

 A. Fixed schemas only\
 B. Horizontal scalability\
 C. Mandatory joins\
 D. Only structured data

 **Answer: B. Horizontal scalability**

 **Explanation:** NoSQL databases are commonly designed to scale horizontally by adding more servers or nodes.

---

 ### 3\. What does horizontal scaling mean?

 A. Increasing the CPU of one server\
 B. Adding more servers/nodes\
 C. Reducing database size\
 D. Increasing table columns

 **Answer: B. Adding more servers/nodes**

 **Explanation:** Horizontal scaling distributes workload across additional machines, while vertical scaling increases the resources of an existing machine.

---

 ### 4\. Which type of data is NoSQL particularly well suited for?

 A. Only highly structured data\
 B. Semi-structured and unstructured data\
 C. Only financial transactions\
 D. Only spreadsheet data

 **Answer: B. Semi-structured and unstructured data**

 **Explanation:** NoSQL systems are designed to handle diverse data structures, including semi-structured and unstructured information.

---

 ### 5\. Which statement best describes NoSQL schema design?

 A. Schema must always be fixed\
 B. Schema cannot change after deployment\
 C. Schema is generally more flexible\
 D. Schema must contain foreign keys

 **Answer: C. Schema is generally more flexible**

 **Explanation:** NoSQL databases commonly support flexible or dynamic schemas, allowing different records to have different structures.

---

 ### 6\. Which is an example of a traditional SQL database use case?

 A. Highly structured banking data\
 B. Social-network relationship traversal\
 C. Distributed caching\
 D. Flexible product documents

 **Answer: A. Highly structured banking data**

 **Explanation:** Relational databases are often appropriate for structured data requiring strong consistency and transactional correctness.

---

 ### 7\. Which statement correctly compares SQL and NoSQL?

 A. SQL always scales horizontally and NoSQL vertically\
 B. SQL generally uses fixed schemas, while NoSQL often uses flexible schemas\
 C. Both require identical schemas\
 D. NoSQL cannot store structured data

 **Answer: B. SQL generally uses fixed schemas, while NoSQL often uses flexible schemas**

 **Explanation:** A major distinction is the flexibility of NoSQL data models compared with traditional relational schemas.

---

 ## Part B — ACID, BASE, and CAP

 ### 8\. What does ACID stand for?

 A. Availability, Consistency, Isolation, Data\
 B. Atomicity, Consistency, Isolation, Durability\
 C. Atomicity, Connectivity, Integration, Distribution\
 D. Availability, Isolation, Durability, Efficiency

 **Answer: B. Atomicity, Consistency, Isolation, Durability**

 **Explanation:** ACID properties emphasize reliable and correct transaction processing.

---

 ### 9\. Which ACID property means that a transaction is treated as an indivisible unit?

 A. Consistency\
 B. Isolation\
 C. Atomicity\
 D. Durability

 **Answer: C. Atomicity**

 **Explanation:** Atomicity means a transaction either completes fully or does not occur as a whole.

---

 ### 10\. Which ACID property ensures committed data survives failures?

 A. Atomicity\
 B. Durability\
 C. Isolation\
 D. Consistency

 **Answer: B. Durability**

 **Explanation:** Durability means committed changes remain stored even after a system failure.

---

 ### 11\. What does BASE emphasize?

 A. Strong consistency only\
 B. Availability, scalability, and eventual consistency\
 C. Relational schemas\
 D. Transaction isolation only

 **Answer: B. Availability, scalability, and eventual consistency**

 **Explanation:** BASE stands for **Basically Available, Soft State, Eventual Consistency**.

---

 ### 12\. What does the "E" in BASE represent?

 A. Efficiency\
 B. Elasticity\
 C. Eventual Consistency\
 D. Execution

 **Answer: C. Eventual Consistency**

 **Explanation:** Eventual consistency means replicas may temporarily differ but converge to a consistent state over time.

---

 ### 13\. Which three concepts form the CAP theorem?

 A. Consistency, Availability, Partition Tolerance\
 B. Consistency, Atomicity, Performance\
 C. Caching, Availability, Processing\
 D. Connectivity, Authentication, Partitioning

 **Answer: A. Consistency, Availability, Partition Tolerance**

 **Explanation:** CAP describes trade-offs in distributed systems when network partitions occur.

---

 ### 14\. In CAP, what does partition tolerance mean?

 A. The system never experiences network problems\
 B. The system continues operating despite network partitioning\
 C. The database has no partitions\
 D. Data is stored in one location

 **Answer: B. The system continues operating despite network partitioning**

 **Explanation:** Partition tolerance concerns the system's ability to function when communication between nodes is disrupted.

---

 ### 15. Which combination is commonly associated with many NoSQL systems?

 A. Availability and scalability\
 B. Fixed schemas and joins\
 C. Strong consistency at all costs\
 D. Single-server architecture

 **Answer: A. Availability and scalability**

 **Explanation:** Many NoSQL systems prioritize availability and scalability and may accept eventual consistency where appropriate.

---

 ## Part C — NoSQL Data Models

 ### 16\. Which NoSQL model stores data as JSON-like documents?

 A. Graph\
 B. Document\
 C. Key-value\
 D. Column-family

 **Answer: B. Document**

 **Explanation:** Document databases store self-contained documents containing field-value pairs.

---

 ### 17\. Which database is an example of the document model?

 A. Redis\
 B. Cassandra\
 C. MongoDB\
 D. Neo4j

 **Answer: C. MongoDB**

 **Explanation:** MongoDB is a document-oriented NoSQL database.

---

 ### 18\. Which model is best suited for caching and sessions?

 A. Graph\
 B. Document\
 C. Key-value\
 D. RDF

 **Answer: C. Key-value**

 **Explanation:** Key-value databases provide very fast retrieval using a unique key and are commonly used for caching and session storage.

---

 ### 19\. Which database is commonly associated with the key-value model?

 A. Redis\
 B. MongoDB\
 C. Neo4j\
 D. Cassandra

 **Answer: A. Redis**

 **Explanation:** Redis is widely used as a high-performance key-value data store.

---

 ### 20\. Which NoSQL model is particularly suitable for highly connected data?

 A. Key-value\
 B. Document\
 C. Graph\
 D. Column

 **Answer: C. Graph**

 **Explanation:** Graph databases explicitly model entities and relationships, making relationship traversal efficient.

---

 ### 21\. Which database is an example of a graph database?

 A. Neo4j\
 B. Redis\
 C. MongoDB\
 D. Cassandra

 **Answer: A. Neo4j**

 **Explanation:** Neo4j is a graph database designed for connected data.

---

 ### 22\. Which NoSQL model is particularly useful for analytics and large-scale column access?

 A. Column-based\
 B. Key-value\
 C. Graph\
 D. Document

 **Answer: A. Column-based**

 **Explanation:** Column-oriented storage can reduce the amount of data read when queries require only particular columns.

---

 ### 23\. Which database is associated with the column-family model?

 A. MongoDB\
 B. Cassandra\
 C. Neo4j\
 D. Redis

 **Answer: B. Cassandra**

 **Explanation:** Cassandra is a well-known distributed column-family database.

---

 ### 24\. Which model is most appropriate for a social-network application with many relationships?

 A. Key-value\
 B. Graph\
 C. Column-family\
 D. Relational only

 **Answer: B. Graph**

 **Explanation:** Social networks involve many interconnected entities, making graph databases a natural choice.

---

 ### 25\. A product catalog has products whose attributes vary significantly from product to product. Which model is especially suitable?

 A. Document\
 B. Key-value only\
 C. Graph only\
 D. Column-family only

 **Answer: A. Document**

 **Explanation:** Document databases provide flexible schemas, allowing different documents to contain different fields.

---

 ## Part D — Document Databases and MongoDB

 ### 26\. In MongoDB, documents are stored in what format?

 A. CSV\
 B. BSON\
 C. XML only\
 D. Plain text

 **Answer: B. BSON**

 **Explanation:** MongoDB stores documents internally using **BSON (Binary JSON)**.

---

 ### 27\. What is the MongoDB equivalent of a relational database table?

 A. Document\
 B. Collection\
 C. Field\
 D. Database key

 **Answer: B. Collection**

 **Explanation:** A MongoDB collection is roughly analogous to a relational table.

---

 ### 28\. What is a MongoDB document roughly analogous to?

 A. Database\
 B. Table\
 C. Row\
 D. Schema

 **Answer: C. Row**

 **Explanation:** A document is roughly comparable to a row, although MongoDB documents can have much more flexible structures.

---

 ### 29\. Which statement about documents in the same MongoDB collection is correct?

 A. They must have exactly the same fields\
 B. They cannot contain arrays\
 C. They can have different fields\
 D. They must have identical structures

 **Answer: C. They can have different fields**

 **Explanation:** MongoDB supports flexible schemas, so documents within a collection do not have to contain identical fields.

---

 ### 30\. Which MongoDB operation inserts one document?

 A. `insertMany()`\
 B. `insertOne()`\
 C. `addOne()`\
 D. `createDocument()`

 **Answer: B. `insertOne()`**

 **Explanation:** `insertOne()` adds a single document.

---

 ### 31\. Which operation inserts multiple documents?

 A. `insertMany()`\
 B. `insertAll()`\
 C. `createMany()`\
 D. `addMany()`

 **Answer: A. `insertMany()`**

 **Explanation:** `insertMany()` inserts multiple documents in one operation.

---

 ### 32\. What does the following command do?

```
db.movies.find()
```

 A. Deletes all movies\
 B. Updates all movies\
 C. Retrieves documents from `movies`\
 D. Counts movies only

 **Answer: C. Retrieves documents from `movies`**

 **Explanation:** `find()` retrieves documents. Without criteria, it returns all documents in the collection.

---

 ### 33\. What is the purpose of `countDocuments()`?

 A. Delete documents\
 B. Count documents\
 C. Sort documents\
 D. Update documents

 **Answer: B. Count documents**

 **Explanation:** `countDocuments()` returns the number of documents matching the specified criteria.

---

 ### 34\. What is the purpose of `find().pretty()`?

 A. Encrypt documents\
 B. Format output for readability\
 C. Delete duplicate documents\
 D. Sort documents

 **Answer: B. Format output for readability**

 **Explanation:** `pretty()` formats MongoDB query output in a more readable form.

---

 ## Part E — MongoDB Querying

 ### 35\. Which operator requires all specified conditions to be true?

 A. `$or`\
 B. `$and`\
 C. `$not`\
 D. `$all`

 **Answer: B. `$and`**

 **Explanation:** `$and` matches documents satisfying all specified conditions.

---

 ### 36\. Which operator matches when at least one condition is true?

 A. `$or`\
 B. `$and`\
 C. `$not`\
 D. `$exists`

 **Answer: A. `$or`**

 **Explanation:** `$or` matches documents satisfying at least one of the supplied conditions.

---

 ### 37\. Which operator negates a condition?

 A. `$or`\
 B. `$all`\
 C. `$not`\
 D. `$in`

 **Answer: C. `$not`**

 **Explanation:** `$not` reverses the result of a specified condition.

---

 ### 38\. What does `$exists: true` test?

 A. Whether a field's value is non-null\
 B. Whether a field is present\
 C. Whether a field is an array\
 D. Whether a field is numeric

 **Answer: B. Whether a field is present**

 **Explanation:** `$exists: true` matches documents where the field exists, even if its value is `null`.

---

 ### 39\. Which query finds documents where `age` exists?

```
db.users.find({
  age: { ______: true }
})
```

 A. `$present`\
 B. `$exists`\
 C. `$available`\
 D. `$field`

 **Answer: B. `$exists`**

 **Explanation:** `$exists` checks whether a field is present.

---

 ### 40\. What does `$exists: false` mean?

 A. The field exists with a null value\
 B. The field does not exist\
 C. The field contains false\
 D. The field contains zero

 **Answer: B. The field does not exist**

 **Explanation:** `$exists: false` matches documents where the specified field is absent.

---

 ### 41\. What does `$in` generally do?

 A. Requires all listed values\
 B. Matches any of the specified values\
 C. Counts array elements\
 D. Removes array elements

 **Answer: B. Matches any of the specified values**

 **Explanation:** `$in` matches a field against any value in a supplied list.

---

 ### 42\. What does `$all` do with an array field?

 A. Requires all specified values to be present\
 B. Requires exactly one value\
 C. Counts values\
 D. Removes all values

 **Answer: A. Requires all specified values to be present**

 **Explanation:** `$all` matches arrays containing every specified value.

---

 ### 43\. What does `$size` test?

 A. The size of a document in bytes\
 B. The number of elements in an array\
 C. The number of fields in a document\
 D. The database size

 **Answer: B. The number of elements in an array**

 **Explanation:** `$size` matches arrays containing a specified number of elements.

---

 ### 44\. A movie has:

```
genres: ["Action", "Drama", "Thriller"]
```

 Which operator would be appropriate for finding movies containing both `"Action"` and `"Drama"`?

 A. `$size`\
 B. `$all`\
 C. `$exists`\
 D. `$not`

 **Answer: B. `$all`**

 **Explanation:** `$all` requires all specified values to occur in the array.

---

 ## Part F — MongoDB Updates

 ### 45\. Which MongoDB method updates the first matching document?

 A. `updateMany()`\
 B. `updateOne()`\
 C. `modifyOne()`\
 D. `changeOne()`

 **Answer: B. `updateOne()`**

 **Explanation:** `updateOne()` updates a single matching document.

---

 ### 46\. Which method updates all matching documents?

 A. `updateAll()`\
 B. `updateOne()`\
 C. `updateMany()`\
 D. `modifyMany()`

 **Answer: C. `updateMany()`**

 **Explanation:** `updateMany()` applies the update to every document matching the filter.

---

 ### 47\. What does `$set` do?

 A. Deletes a field\
 B. Assigns a value to a field\
 C. Sorts documents\
 D. Counts documents

 **Answer: B. Assigns a value to a field**

 **Explanation:** `$set` changes or creates the specified field with the supplied value.

---

 ### 48\. What does `upsert: true` do?

 A. Deletes unmatched documents\
 B. Sorts the result\
 C. Inserts a document if no document matches\
 D. Updates every document

 **Answer: C. Inserts a document if no document matches**

 **Explanation:** An upsert performs an update if a match exists; otherwise, it inserts a new document.

---

 ### 49\. Which statement is correct?

 A. `updateOne()` always updates all matching documents\
 B. `updateMany()` updates only the first match\
 C. `updateOne()` updates one matching document, while `updateMany()` updates all matches\
 D. Both methods always insert new documents

 **Answer: C. `updateOne()` updates one matching document, while `updateMany()` updates all matches**

 **Explanation:** This is the fundamental distinction between the two update methods.

---

 ## Part G — MongoDB Sorting and Deletion

 ### 50\. What does `sort({ age: 1 })` mean?

 A. Age descending\
 B. Age ascending\
 C. Age randomly\
 D. Age is ignored

 **Answer: B. Age ascending**

 **Explanation:** In MongoDB sorting, `1` means ascending order.

---

 ### 51\. What does `sort({ age: -1 })` mean?

 A. Age ascending\
 B. Age descending\
 C. Age is excluded\
 D. Age is counted

 **Answer: B. Age descending**

 **Explanation:** `-1` specifies descending order.

---

 ### 52\. Consider:

```
db.movies.find().sort({
  genre: 1,
  released: -1
})
```

 What happens first?

 A. `released` is sorted ascending\
 B. `genre` is sorted ascending\
 C. Both are sorted descending\
 D. Documents are deleted

 **Answer: B. `genre` is sorted ascending**

 **Explanation:** MongoDB first sorts by `genre` ascending. For documents with the same genre, it sorts `released` descending.

---

 ### 53\. Which method deletes the first matching document?

 A. `deleteMany()`\
 B. `deleteOne()`\
 C. `removeAll()`\
 D. `dropOne()`

 **Answer: B. `deleteOne()`**

 **Explanation:** `deleteOne()` removes one matching document.

---

 ### 54\. Which method deletes all matching documents?

 A. `deleteOne()`\
 B. `deleteMany()`\
 C. `removeOne()`\
 D. `dropMany()`

 **Answer: B. `deleteMany()`**

 **Explanation:** `deleteMany()` removes every document matching the filter.

---

 ## Part H — MongoDB CRUD

 ### 55\. Which operation represents the "C" in CRUD?

 A. Create\
 B. Count\
 C. Connect\
 D. Compare

 **Answer: A. Create**

 **Explanation:** CRUD stands for **Create, Read, Update, Delete**.

---

 ### 56\. Which set contains only MongoDB Create operations?

 A. `find()`, `countDocuments()`\
 B. `updateOne()`, `updateMany()`\
 C. `insertOne()`, `insertMany()`\
 D. `deleteOne()`, `deleteMany()`

 **Answer: C. `insertOne()`, `insertMany()`**

 **Explanation:** Insert operations create documents.

---

 ### 57\. Which set contains MongoDB Read operations?

 A. `find()`, `countDocuments()`\
 B. `insertOne()`, `insertMany()`\
 C. `updateOne()`, `updateMany()`\
 D. `deleteOne()`, `deleteMany()`

 **Answer: A. `find()`, `countDocuments()`**

 **Explanation:** These operations retrieve or count documents.

---

 ### 58\. Which set contains MongoDB Delete operations?

 A. `find()`, `sort()`\
 B. `insertOne()`, `insertMany()`\
 C. `deleteOne()`, `deleteMany()`\
 D. `updateOne()`, `updateMany()`

 **Answer: C. `deleteOne()`, `deleteMany()`**

 **Explanation:** These methods remove documents.

---

 ## Part I — Column-Based Databases

 ### 59\. Why can column-oriented storage be efficient for analytical queries?

 A. It always reads every column\
 B. It can read only the required columns\
 C. It eliminates all indexes\
 D. It requires every row to have identical fields

 **Answer: B. It can read only the required columns**

 **Explanation:** If a query needs only a few columns from a large dataset, column-oriented storage can avoid reading unnecessary data.

---

 ### 60\. A query needs `current_balance` from millions of records but does not need the other fields. Which storage model can be particularly efficient?

 A. Column-oriented\
 B. Row-oriented only\
 C. Graph\
 D. Key-value only

 **Answer: A. Column-oriented**

 **Explanation:** Column-oriented storage can access the required column without reading every field of every row.

---

 ### 61\. Row-oriented storage is generally advantageous when:

 A. A query accesses most or all fields of a particular row\
 B. A query needs one column across millions of rows\
 C. Relationships are the primary concern\
 D. Data must be represented as RDF triples

 **Answer: A. A query accesses most or all fields of a particular row**

 **Explanation:** Row-oriented storage keeps fields belonging to a record together, making complete-record access efficient.

---

 ### 62\. What is a column family?

 A. A group of related columns\
 B. A MongoDB document\
 C. A graph edge\
 D. A database server

 **Answer: A. A group of related columns**

 **Explanation:** A column family organizes related columns and acts somewhat like a logical table.

---

 ### 63\. What uniquely identifies a row in a column-family database?

 A. Column name\
 B. Row key\
 C. Timestamp\
 D. Column value

 **Answer: B. Row key**

 **Explanation:** The row key uniquely identifies a row within a column family.

---

 ### 64\. What information is associated with a column in the described column-family model?

 A. Name, value, and timestamp\
 B. Only a name\
 C. Only a timestamp\
 D. Node, edge, and URI

 **Answer: A. Name, value, and timestamp**

 **Explanation:** A column contains a name and value together with an associated timestamp indicating when it was written.

---

 ### 65\. Which statement about rows in a column-family database is correct?

 A. Every row must contain exactly the same columns\
 B. Rows may have different sets of columns\
 C. Rows cannot have keys\
 D. Rows are always JSON documents

 **Answer: B. Rows may have different sets of columns**

 **Explanation:** Column-family databases can support flexible rows where different rows contain different columns.

---

 ### 66\. Which is NOT a core element described for a column-family database?

 A. Row key\
 B. Column family\
 C. Column\
 D. RDF predicate

 **Answer: D. RDF predicate**

 **Explanation:** RDF predicates belong to the RDF graph model, not the column-family model.

---

 ## Part J — Graph Databases

 ### 67\. What are the basic elements of a graph database?

 A. Tables, rows, foreign keys\
 B. Nodes, relationships, properties\
 C. Keys, values, timestamps\
 D. Documents, collections, fields

 **Answer: B. Nodes, relationships, properties**

 **Explanation:** Graph databases represent entities as nodes and connections as relationships, with properties describing them.

---

 ### 68\. In a graph database, what does a node represent?

 A. An entity or object\
 B. A relationship only\
 C. A timestamp\
 D. A database schema

 **Answer: A. An entity or object**

 **Explanation:** Nodes can represent people, products, companies, cities, and other entities.

---

 ### 69\. What does an edge/relationship represent?

 A. A field name\
 B. A connection between nodes\
 C. A database table\
 D. A row key

 **Answer: B. A connection between nodes**

 **Explanation:** Relationships express how entities are connected.

---

 ### 70\. Which scenario is best suited to a graph database?

 A. Simple session lookup by ID\
 B. Social-network friend relationships\
 C. Storing independent configuration values\
 D. Simple numerical aggregation only

 **Answer: B. Social-network friend relationships**

 **Explanation:** Graph databases excel at highly connected data and relationship traversal.

---

 ### 71\. Why are graph databases useful for recommendation systems?

 A. Recommendations often involve relationships between entities\
 B. Recommendations never require relationships\
 C. Graph databases only store numbers\
 D. Graph databases cannot traverse relationships

 **Answer: A. Recommendations often involve relationships between entities**

 **Explanation:** Recommendation systems can analyze connections such as users → products → categories → other users.

---

 ### 72\. Which is another common graph-database use case?

 A. Fraud detection\
 B. Simple static text files\
 C. Spreadsheet formatting\
 D. Word processing

 **Answer: A. Fraud detection**

 **Explanation:** Fraud detection often involves identifying complex relationships and patterns among accounts, transactions, devices, and people.

---

 ## Part K — RDF

 ### 73\. What does RDF stand for?

 A. Relational Data Format\
 B. Resource Description Framework\
 C. Random Data Framework\
 D. Resource Database Format

 **Answer: B. Resource Description Framework**

 **Explanation:** RDF is a standard for representing and integrating data, particularly on the Web.

---

 ### 74\. What is the basic unit of RDF?

 A. Document\
 B. Triple\
 C. Row\
 D. Column family

 **Answer: B. Triple**

 **Explanation:** RDF represents information using subject-predicate-object triples.

---

 ### 75\. What is the structure of an RDF triple?

 A. Node–Edge–Node\
 B. Key–Value–Timestamp\
 C. Subject–Predicate–Object\
 D. Row–Column–Value

 **Answer: C. Subject–Predicate–Object**

 **Explanation:** An RDF statement has the form `(subject, predicate, object)`.

---

 ### 76\. Given:

```
(Alice, knows, Bob)
```

 What is the predicate?

 A. Alice\
 B. knows\
 C. Bob\
 D. The entire triple

 **Answer: B. knows**

 **Explanation:** In RDF, the middle element is the predicate describing the relationship between subject and object.

---

 ### 77\. In the following RDF triple:

```
(Alice, knows, Bob)
```

 What is the subject?

 A. Alice\
 B. knows\
 C. Bob\
 D. URI

 **Answer: A. Alice**

 **Explanation:** The first element of an RDF triple is the subject.

---

 ### 78\. What is the object in:

```
(Alice, knows, Bob)
```

 A. Alice\
 B. knows\
 C. Bob\
 D. Relationship

 **Answer: C. Bob**

 **Explanation:** The third element is the object.

---

 ### 79\. What do URIs provide in RDF?

 A. Unique identifiers for resources\
 B. Database passwords\
 C. Sorting instructions\
 D. Array lengths

 **Answer: A. Unique identifiers for resources**

 **Explanation:** URIs provide unique, globally identifiable references for resources.

---

 ### 80\. RDF is particularly associated with which concept?

 A. Semantic Web and Web of Data\
 B. Session caching\
 C. MongoDB CRUD\
 D. Column compression

 **Answer: A. Semantic Web and Web of Data**

 **Explanation:** RDF focuses on representing data with explicit meaning and integrating linked data.

---

 ## Part L — Property Graphs

 ### 81\. Which components make up a property graph?

 A. Tables and foreign keys\
 B. Nodes, relationships, and properties\
 C. Subjects, predicates, and objects only\
 D. Keys and values only

 **Answer: B. Nodes, relationships, and properties**

 **Explanation:** Property graphs represent entities as nodes, connections as relationships, and descriptive information as properties.

---

 ### 82\. What can a property graph node contain?

 A. Labels and key-value properties\
 B. Only a URI\
 C. Only a timestamp\
 D. Only a relationship

 **Answer: A. Labels and key-value properties**

 **Explanation:** Nodes can have labels identifying their type and properties such as `name`, `age`, or `city`.

---

 ### 83\. What can a property graph relationship contain?

 A. Only its direction\
 B. Its own properties\
 C. Only a URI\
 D. No information

 **Answer: B. Its own properties**

 **Explanation:** Relationships can contain properties such as `since`, `role`, or `weight`.

---

 ### 84\. Consider:

```
Alice ──WORKS_AT──> Company
```

 What does `WORKS_AT` represent?

 A. Node\
 B. Relationship type\
 C. Node property\
 D. Row key

 **Answer: B. Relationship type**

 **Explanation:** `WORKS_AT` identifies the type/name of the relationship connecting Alice and Company.

---

 ### 85\. Which property could logically belong to a `WORKS_AT` relationship?

 A. `since = 2024`\
 B. `genre = "Drama"`\
 C. `rowKey = 123`\
 D. `$exists = true`

 **Answer: A. `since = 2024`**

 **Explanation:** A relationship can contain stores products where electronics have different attributes from clothing. billions of records but usually needs only a few columns. Which model is particularly a person, their age, and a relationship to a company, including the information describing the connection, such as when employment began.

---

 ## Part M — RDF vs. Property Graph

 ### 86\. Which model uses subject-predicate-object triples?

 A. Property Graph\
 B. RDF\
 C. Key-value\
 D. Document

 **Answer: B. RDF**

 **Explanation:** RDF's fundamental representation is the triple `(subject, predicate, object)`.

---

 ### 87\. Which model directly stores properties on nodes and relationships?

 A. RDF\
 B. Property Graph\
 C. Key-value only\
 D. Relational

 **Answer: B. Property Graph**

 **Explanation:** Property graphs directly attach key-value properties to nodes and relationships.

---

 ### 88\. Which model has a stronger emphasis on semantics and data integration?

 A. Property Graph\
 B. RDF\
 C. Key-value\
 D. Document

 **Answer: B. RDF**

 **Explanation:** RDF is strongly associated with semantic representation, linked data, and data integration.

---

 ### 89\. Which model is designed for practical connected-data querying and efficient traversal?

 A. RDF\
 B. Property Graph\
 C. Key-value\
 D. Document

 **Answer: B. Property Graph**

 **Explanation:** Property graphs are designed around efficient traversal and querying of connected entities and their properties.

---

 ### 90\. Which pairing is correct?

 A. RDF → caching\
 B. Key-value → semantic Web\
 C. Property Graph → connected-data traversal\
 D. Document → RDF triples

 **Answer: C. Property Graph → connected-data traversal**

 **Explanation:** Property graphs are particularly suited to querying and traversing connected data.

---

 ## Part N — Scenario-Based Questions

 ### 91\. A company needs extremely fast retrieval of user sessions using a unique session ID. Which model is most appropriate?

 A. Graph\
 B. Key-value\
 C. RDF\
 D. Column-family

 **Answer: B. Key-value**

 **Explanation:** Sessions are commonly accessed using a unique key, making key-value storage highly suitable.

---

 ### 92\. An e-commerce application stores products where electronics have different attributes from clothing. Which model is a strong choice?

 A. Document\
 B. Graph only\
 C. Key-value only\
 D. RDF only

 **Answer: A. Document**

 **Explanation:** Document databases allow flexible schemas, so different product types can have different fields.

---

 ### 93\. A social network needs to determine who is connected to whom and find relationship paths. Which model is most appropriate?

 A. Key-value\
 B. Graph\
 C. Document\
 D. Column-family

 **Answer: B. Graph**

 **Explanation:** Graph databases are designed specifically for relationship-heavy data and traversal.

---

 ### 94\. A data warehouse frequently calculates statistics over billions of records but usually needs only a few columns. Which model is particularly suitable?

 A. Column-based\
 B. Graph\
 C. Key-value\
 D. Document

 **Answer: A. Column-based**

 **Explanation:** Column-oriented storage can avoid reading unnecessary columns, making it well suited to analytical workloads.

---

 ### 95\. An organization wants to integrate Web data while preserving explicit semantic meaning between resources. Which graph model is most appropriate?

 A. Property Graph\
 B. RDF\
 C. Key-value\
 D. Document

 **Answer: B. RDF**

 **Explanation:** RDF is designed for semantic representation, linked data, and Web-based data integration.

---

 ### 96\. An application needs to store a person, their age, and a relationship to a company, including the year they started working there. Which model naturally supports properties on both entities and relationships?

 A. RDF only\
 B. Property Graph\
 C. Key-value only\
 D. Column-family only

 **Answer: B. Property Graph**

 **Explanation:** Property graphs allow properties on nodes and relationships, such as `age = 25` on a person and `since = 2024` on a `WORKS_AT` relationship.

---

 ### 97\. You need to update the salary of every employee in a department. Which MongoDB method should you consider?

 A. `updateOne()`\
 B. `updateMany()`\
 C. `deleteMany()`\
 D. `insertMany()`

 **Answer: B. `updateMany()`**

 **Explanation:** When multiple documents match the condition and all need to be updated, `updateMany()` is appropriate.

---

 ### 98\. You want to remove only the first movie matching a particular condition. Which method should you use?

 A. `deleteMany()`\
 B. `deleteOne()`\
 C. `updateOne()`\
 D. `find()`

 **Answer: B. `deleteOne()`**

 **Explanation:** `deleteOne()` deletes the first matching document.

---

 ### 99\. You want to find the three highest-rated movies. Which combination is most appropriate?

 A. `$exists` \+ `$all`\
 B. `$sort` \+ `$limit`\
 C. `$and` \+ `$or` primarily designed for session\
 D. `$set` \+ `upsert`

 **Answer: B. `$sort` \+ `$limit`**

 **Explanation:** Sort the movies by rating descending and then limit the result to three documents.

---

 ### 100\. You need to group movies by genre and count how many movies belong to each genre. Which MongoDB feature is most appropriate?

 A. `deleteMany()`\
 B. `find()` only\
 C. Aggregation with `$group`\
 D. `insertMany()`

 **Answer: C. Aggregation with `$group`**

 **Explanation:** Aggregation pipelines can group documents and calculate values such as counts using `$sum`.

---

 # Part O — Challenging Mixed Questions

 ### 101\. Which sequence correctly represents the typical MongoDB CRUD mapping?

 A. Create → `find()`, Read → `insertOne()`, Update → `deleteOne()`, Delete → `updateOne()`\
 B. Create → `insertOne()`, Read → `find()`, Update → `updateOne()`, Delete → `deleteOne()`\
 C. Create → `sort()`, Read → `$set`, Update → `find()`, Delete → `$exists`\
 D. Create → `$group`, Read → `$match`, Update → `$limit`, Delete → `$sort`

 **Answer: B. Create → `insertOne()`, Read → `find()`, Update → `updateOne()`, Delete → `deleteOne()`**

 **Explanation:** This is the standard CRUD mapping in MongoDB.

---

 ### 102\. Which statement correctly connects NoSQL model choice to workload?

 A. Graph databases are always best for caching\
 B. Key-value databases are ideal for complex relationship traversal\
 C. Document databases are useful for flexible application data\
 D. RDF is primarily designed for session storage

 **Answer: C. Document databases are useful for flexible application data**

 **Explanation:** Each NoSQL model is optimized for different workloads. Document databases are particularly useful when data structures are flexible.

---

 ### 103\. Which statement is TRUE about `$exists`?

 A. `$exists: true` means the field must contain a non-null value\
 B. `$exists: true` means the field is present\
 C. `$exists: false` means the field contains `false`\
 D. `$exists` checks whether an array has a certain length

 **Answer: B. `$exists: true` means the field is present**

 **Explanation:** A field can exist even if its value is `null`.

---

 ### 104\. Which statement correctly distinguishes `$in`, `$all`, and `$size`?

 A. `$in` requires all values, `$all` counts values, `$size` checks existence\
 B. `$in` matches any listed value, `$all` requires all listed values, `$size` checks array length\
 C. `$in` checks array length, `$all` checks field existence, `$size` sorts\
 D. All three perform the same operation

 **Answer: B. `$in` matches any listed value, `$all` requires all listed values, `$size` checks array length**

 **Explanation:** This distinction is particularly important for MongoDB array queries.

---

 ### 105\. Which statement best summarizes the difference between RDF and a property graph?

 A. RDF uses tables while property graphs use documents\
 B. RDF uses triples and emphasizes semantics; property graphs use nodes/relationships/properties and emphasize traversal\
 C. RDF is for caching while property graphs are for sessions\
 D. They are exactly the same model

 **Answer: B. RDF uses triples and emphasizes semantics; property graphs use nodes/relationships/properties and emphasize traversal**

 **Explanation:** This is one of the key conceptual distinctions in the graph-database material.

---

 ### 106\. Which database model most directly represents this structure?

```
Alice
  |
  | FRIENDS_WITH
  ↓
Bob
```

 A. Graph\
 B. Key-value\
 C. Document\
 D. Column-family

 **Answer: A. Graph**

 **Explanation:** The diagram explicitly represents entities and a relationship between them.

---

 ### 107\. Which RDF representation corresponds to the statement "Alice knows Bob"?

 A. `(Alice, knows, Bob)`\
 B. `(knows, Alice, Bob)`\
 C. `(Bob, Alice, knows)`\
 D. `(Alice, Bob, knows)`

 **Answer: A. `(Alice, knows, Bob)`**

 **Explanation:** RDF uses the order **subject → predicate → object**.

---

 ### 108\. A database query accesses `current_balance` for millions of customers but does not need their names or addresses. Which storage strategy is likely to minimize unnecessary data access?

 A. Column-oriented storage\
 B. Row-oriented storage\
 C. Graph traversal\
 D. RDF triples

 **Answer: A. Column-oriented storage**

 **Explanation:** Column-oriented storage allows the database to focus on the required column rather than reading complete records.

---

 ### 109\. Which statement about a column-family database is FALSE?

 A. A row has a row key\
 B. A column can have a timestamp\
 C. All rows must necessarily contain exactly the same columns\
 D. A column family groups related columns

 **Answer: C. All rows must necessarily contain exactly the same columns**

 **Explanation:** Flexible column-family models allow different rows to contain different sets of columns.

---

 ### 110\. Which statement provides the best overall description of NoSQL?

 A. NoSQL is simply SQL without tables\
 B. NoSQL is a group of flexible data models designed for various scalability, availability, performance, and data-structure requirements\
 C. NoSQL can only store unstructured data\
 D. NoSQL databases cannot perform queries

 **Answer: B. NoSQL is a group of flexible data models designed for various scalability, availability, performance, and data-structure requirements**

 **Explanation:** NoSQL is not one specific database technology. It encompasses several models—document, key-value, column-family, and graph—each designed for different requirements.

---

 # 🧠 Ultra-Important Exam Cheat Sheet

 | Topic | Remember |
| --- | --- |
| **SQL** | Tables, rows, columns, fixed schema |
| **NoSQL** | Flexible models, scalability, availability |
| **ACID** | Atomicity, Consistency, Isolation, Durability |
| **BASE** | Basically Available, Soft State, Eventual Consistency |
| **CAP** | Consistency, Availability, Partition Tolerance |
| **Document** | Documents/collections → MongoDB |
| **Key-Value** | Key → value → Redis |
| **Column-Family** | Columns/column families → Cassandra |
| **Graph** | Nodes + relationships + properties → Neo4j |
| **MongoDB storage** | BSON |
| **MongoDB Create** | `insertOne()`, `insertMany()` |
| **MongoDB Read** | `find()`, `countDocuments()` |
| **MongoDB Update** | `updateOne()`, `updateMany()` |
| **MongoDB Delete** | `deleteOne()`, `deleteMany()` |
| **`$set`** | Assign/change a field |
| **`upsert`** | Update or insert if no match |
| **`$and`** | All conditions |
| **`$or`** | At least one condition |
| **`$not`** | Negates condition |
| **`$exists`** | Field presence |
| **`$in`** | Any listed value |
| **`$all`** | All listed values |
| **`$size`** | Array length |
| **`sort: 1`** | Ascending |
| **`sort: -1`** | Descending |
| **Column-oriented** | Efficient when reading selected columns |
| **Row-oriented** | Efficient when reading complete records |
| **Row key** | Unique row identifier in column-family DB |
| **Column family** | Group of related columns |
| **Graph node** | Entity |
| **Graph relationship** | Connection |
| **Graph property** | Descriptive key-value information |
| **RDF** | Subject–Predicate–Object |
| **RDF focus** | Semantics + integration \+ Web of Data |
| **URI** | Identifier for RDF resources |
| **Property Graph** | Nodes + relationships + properties |
| **Property Graph focus** | Traversal + connected-data querying |

## 🎯 The 10 Distinctions Most Likely to Cause Mistakes

 1. **Horizontal vs. vertical scaling** → horizontal = add machines; vertical = increase resources of one machine.
2. **ACID vs. BASE** → ACID emphasizes correctness/strong consistency; BASE emphasizes availability/eventual consistency.
3. **CAP** → Consistency + Availability \+ Partition Tolerance.
4. **`updateOne()` vs. `updateMany()`** → first/single match vs. all matches.
5. **`deleteOne()` vs. `deleteMany()`** → first/single match vs. all matches.
6. **`$in` vs. `$all`** → any listed value vs. all listed values.
7. **`$exists` vs. `$size`** → field presence vs. array length.
8. **Column vs. row storage** → selected columns across many records vs. complete records.
9. **RDF vs. Property Graph** → triples/semantics vs. nodes-relationships-properties/traversal.
10. **Document vs. Graph** → flexible application records vs. highly connected relationships.

 **Master memory line:**

 > **Document = flexible data, Key-Value = fast lookup, Column = analytics, Graph = relationships; MongoDB = BSON + CRUD; RDF = triples + meaning; Property Graph = nodes \+ relationships + properties.**
