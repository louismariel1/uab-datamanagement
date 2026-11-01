# T4 — Redis MCQ Practice Bank (60 Questions)

## Exam Practice and Explanations

**Purpose:** A 60-question multiple-choice practice bank based on the supplied *T4 — Redis Review Questions and Answers* document. It covers in-memory databases, NoSQL, caching, persistence, Redis architectures, data types, TTL, cache synchronization, and integrated scenarios.

**Source note:** Questions are derived from the supplied revision material. The source does not establish that these are actual examination questions or that they were explicitly suggested by the lecturer.

**Instructions:** Select the **one best answer** for each question. The answer key and explanations appear after all questions.

---

## Section 1 — In-memory databases and NoSQL

### Q1. Why can an in-memory database be faster than a disk-oriented database?
A. It automatically compresses every value.  
B. It primarily accesses data in RAM, avoiding disk access for every operation.  
C. It eliminates network latency and serialization overhead.  
D. It guarantees that every query uses an index.

### Q2. Which option identifies the four major NoSQL data models?
A. Relational, object, hierarchical, and time-series  
B. Document, key-value, column-based, and graph  
C. Table, row, array, and binary  
D. Key-value, relational, graph, and spatial

### Q3. Which statement best describes the CAP theorem?
A. A distributed system always guarantees consistency, availability, and partition tolerance simultaneously.  
B. During a network partition, a distributed system cannot guarantee both full consistency and full availability for every request.  
C. CAP states that every database must use atomic transactions.  
D. CAP applies only to standalone databases.

### Q4. Which is an appropriate candidate for caching?
A. A value that is never requested  
B. Frequently requested results of expensive database queries  
C. Data that must always be recalculated and never reused  
D. Data that changes every millisecond and has no freshness policy

### Q5. A product catalogue is requested thousands of times but changes only occasionally. What is the most appropriate conclusion?
A. Caching is unsuitable because product data is persistent.  
B. Caching can reduce response time and database load, provided changes refresh or invalidate affected entries.  
C. Every product must be stored only in Redis.  
D. The catalogue should be cached forever without an expiration or invalidation policy.

### Q6. Why is a key-value database suitable for storing user sessions?
A. Sessions require graph traversal between every field.  
B. Each session can be retrieved using a unique session identifier, and TTL can expire it.  
C. Key-value databases guarantee that sessions survive every server failure.  
D. Session data must be stored in sorted order.

### Q7. Does storing data in RAM guarantee that it survives a Redis server restart?
A. Yes, because RAM is non-volatile.  
B. Yes, because Redis always writes every command synchronously to disk.  
C. No; recovery depends on persistence configuration, such as RDB snapshots or AOF.  
D. No; Redis cannot persist data.

### Q8. Is Redis always better than Memcached because it supports more features?
A. Yes; more features guarantee better performance for every workload.  
B. No; the best choice depends on functionality, workload, configuration, operational complexity, and measured performance.  
C. Yes; Memcached cannot store values in memory.  
D. No; Redis is only a relational database.

## Section 2 — Redis caching and persistence

### Q9. What is a cache hit?
A. The requested value exists in Redis and can be returned.  
B. The requested value is absent from Redis.  
C. Redis has restarted and restored an RDB file.  
D. A write operation has been rejected.

### Q10. Why might an application place Redis in front of MySQL or PostgreSQL?
A. To replace the relational database in every use case  
B. To serve frequently requested data from memory, reducing repeated database queries and response times  
C. To guarantee that every cached value is always current  
D. To eliminate application-level consistency handling

### Q11. What does RDB persistence save?
A. A continuous record of every read operation  
B. Point-in-time snapshots of the Redis dataset in a binary file  
C. Only the names of Redis keys  
D. A relational export of all Redis data

### Q12. Which is an advantage of RDB persistence?
A. It guarantees that no recent write can ever be lost.  
B. It produces relatively compact snapshots suitable for backups.  
C. It records every operation individually.  
D. It never requires disk storage.

### Q13. Which is a disadvantage of periodic RDB snapshots?
A. Recovery is impossible after a restart.  
B. Changes since the last usable snapshot may be lost.  
C. RDB cannot store strings.  
D. RDB always requires command replay.

### Q14. How does AOF persistence reconstruct the Redis dataset during recovery?
A. By replaying recorded write operations  
B. By querying the original relational database  
C. By recalculating every value from application source code  
D. By reading only the most recent cache hit

### Q15. Which statement best compares RDB and AOF?
A. RDB is a write-operation log, while AOF is a point-in-time snapshot.  
B. RDB stores snapshots; AOF records write operations that can be replayed.  
C. Both store only key names.  
D. Neither mechanism supports recovery after a restart.

### Q16. A Redis server fails after the second RDB snapshot but before the third. What may be missing from recovery?
A. All data created before the first snapshot  
B. Changes made after the second snapshot that are absent from the latest usable snapshot  
C. Only keys that have a TTL  
D. All data stored in replicas

### Q17. Why can AOF provide finer-grained recovery than periodic RDB snapshots?
A. AOF records write operations between snapshots.  
B. AOF stores every read request.  
C. AOF eliminates the need for synchronization to durable storage.  
D. AOF always has a smaller file than RDB.

### Q18. Which is a possible disadvantage of AOF persistence?
A. It cannot record write operations.  
B. Its log can grow, recovery may take longer, and logging or synchronization can add overhead.  
C. It cannot be used together with RDB.  
D. It prevents Redis from accepting writes.

### Q19. In RESP, what does `*3` represent in the encoding of `SET hello world`?
A. Three bytes in the command name  
B. An array containing three elements  
C. Three Redis databases  
D. Three seconds of key expiration

### Q20. In RESP, what does `$5` immediately before `hello` indicate?
A. The string has five bytes.  
B. The key has a TTL of five seconds.  
C. The command contains five arguments.  
D. The value is stored in database number five.

## Section 3 — Redis architectures and scalability

### Q21. What is the main disadvantage of Standalone Redis?
A. It cannot store strings.  
B. It has a single point of failure and no built-in replica redundancy within the standalone deployment.  
C. It requires hash-slot routing.  
D. It cannot be used as a cache.

### Q22. In a primary–replica deployment, which node normally receives writes?
A. Every replica independently  
B. The primary  
C. Sentinel only  
D. The client-side cache

### Q23. What is the role of Redis Sentinel?
A. To partition keys into 16,384 hash slots  
B. To monitor a primary–replica deployment and coordinate automatic failover  
C. To replace Redis persistence  
D. To convert Lists into Sorted Sets

### Q24. Which statement correctly distinguishes replication from Sentinel?
A. Replication monitors failures; Sentinel copies the dataset.  
B. Replication maintains copies of data; Sentinel monitors the deployment and coordinates failover.  
C. Both perform sharding.  
D. Sentinel is the Redis data type used for sessions.

### Q25. Does Redis Sentinel provide sharding?
A. Yes, by assigning each key to a hash slot.  
B. Yes, by splitting every Hash into separate nodes.  
C. No; Redis Cluster distributes data across nodes using hash slots.  
D. No; Sentinel disables replication.

### Q26. How many hash slots does Redis Cluster use?
A. 256  
B. 1,024  
C. 16,384  
D. 65,536

### Q27. Which formula describes Redis Cluster's hash-slot calculation?
A. `CRC16(key) × 16384`  
B. `CRC16(key) mod 16384`  
C. `SHA256(key) mod 256`  
D. `key length mod 16384`

### Q28. Why might an application use Redis Cluster instead of Standalone Redis?
A. To distribute data and workload across multiple nodes, supporting horizontal scalability and greater aggregate storage capacity  
B. To avoid all network communication  
C. To guarantee zero downtime and zero data loss  
D. To remove the need for key routing

### Q29. What is the difference between replication and sharding?
A. Replication distributes different data portions; sharding copies the same dataset.  
B. Replication maintains copies of the same data; sharding distributes different portions of the dataset.  
C. They are two names for the same operation.  
D. Neither can involve multiple nodes.

### Q30. What is vertical scaling?
A. Adding machines or nodes to distribute workload  
B. Adding resources to an existing machine  
C. Splitting keys into hash slots  
D. Copying a dataset to a replica

### Q31. Why might a Redis replica return stale data?
A. Replication may be asynchronous, so the latest write has not yet been received or applied.  
B. Replicas always store older RDB formats.  
C. Redis automatically delays every read by one minute.  
D. A replica cannot receive updates from a primary.

### Q32. Why should replicated data still be backed up?
A. Replication prevents all accidental deletion.  
B. Logical mistakes, accidental deletion, and corruption can propagate to replicas.  
C. Replicas cannot contain copies of the dataset.  
D. Backups are required only when using Sorted Sets.

## Section 4 — Redis data types

### Q33. Which Redis data type is most appropriate for a leaderboard?
A. List  
B. Hash  
C. Sorted Set  
D. HyperLogLog

### Q34. What is the main characteristic of a Redis Hash?
A. It stores multiple field–value pairs under one Redis key.  
B. It stores only unique members ordered by score.  
C. It stores an append-only event log.  
D. It stores geographic coordinates only.

### Q35. Which statement correctly compares a List and a Set?
A. A List is unordered and unique; a Set is ordered and permits duplicates.  
B. A List preserves order and permits duplicates; a Set contains unique members without guaranteed order.  
C. Both always preserve insertion order and permit duplicates.  
D. Both store only field–value pairs.

### Q36. A queue must preserve order and allow duplicate tasks. Which Redis data type is appropriate?
A. Set  
B. List  
C. Sorted Set  
D. HyperLogLog

### Q37. Which data type is suitable for storing a unique collection of website visitors?
A. Set  
B. List  
C. Hash  
D. Stream

### Q38. Which Redis data type stores unique members associated with numerical scores and supports score-based ranking?
A. String  
B. Hash  
C. Sorted Set  
D. List

### Q39. What are Redis Strings used for?
A. Only text values shorter than 100 bytes  
B. Sequences of bytes, including text, numeric values used by supported operations, serialized objects, and binary data  
C. Only geographic coordinates  
D. Only unique collections of members

### Q40. Which statement about Redis Bitmaps and Bitfields is correct?
A. Bitmaps support bit-level operations over strings; Bitfields read and modify integer fields at bit offsets.  
B. Bitmaps store graph edges; Bitfields store sorted members.  
C. Both are persistence file formats.  
D. Both are used exclusively for geospatial queries.

### Q41. Which Redis feature is designed for approximate counting of distinct elements?
A. Streams  
B. HyperLogLog  
C. Geospatial indexes  
D. Hashes

### Q42. Which Redis feature is intended for storing locations and performing geographic queries?
A. Geospatial indexes  
B. HyperLogLog  
C. AOF  
D. Lists

### Q43. What are Redis Streams primarily used for?
A. Append-oriented event entries for event processing and messaging  
B. Point-in-time snapshots  
C. Unique visitor counting only  
D. Storing user profile fields

## Section 5 — TTL and cache synchronization

### Q44. What does TTL mean in Redis?
A. Total Transaction Limit  
B. Time to Live  
C. Temporary Table Log  
D. Time to Load

### Q45. What does `TTL key = -1` mean?
A. The key does not exist.  
B. The key exists but has no expiration set.  
C. The key expires in one second.  
D. The key is stored on a replica.

### Q46. What does `TTL key = -2` mean?
A. The key does not exist.  
B. The key exists without an expiration.  
C. The key is being persisted to AOF.  
D. The key has two fields.

### Q47. What happens when a cached key expires?
A. Redis automatically queries MySQL and recreates it.  
B. The key is no longer available as a valid entry; the application may retrieve the value from the primary database and repopulate Redis.  
C. The key is copied to every replica permanently.  
D. The entire Redis database is restarted.

### Q48. What is the difference between TTL and cache invalidation?
A. TTL is time-based expiration; invalidation removes or refreshes a value when it is no longer valid, often after underlying data changes.  
B. TTL updates the primary database; invalidation creates an RDB snapshot.  
C. They are identical mechanisms.  
D. Invalidation is possible only with AOF.

### Q49. What is lazy loading, commonly implemented as cache-aside?
A. Populate the cache only when requested data is absent.  
B. Populate every cache entry when the server starts.  
C. Write only to Redis and never query the primary database.  
D. Expire all cache entries after every read.

### Q50. Which is an advantage of lazy loading?
A. Every record is cached before it is requested.  
B. Only requested data is cached, avoiding unnecessary cache population.  
C. It guarantees that the cache is always current.  
D. It eliminates database lookups on cache misses.

### Q51. What is write-through caching?
A. A strategy in which the primary database is updated and the corresponding cache entry is updated as part of the same application write flow  
B. A strategy that updates Redis but never updates the primary database  
C. A strategy that writes only periodic RDB snapshots  
D. A strategy that populates the cache only after a cache miss

### Q52. Does write-through caching guarantee that the cache is never stale?
A. Yes, because cache updates cannot fail.  
B. Yes, because concurrent operations are impossible.  
C. No; failures, concurrent operations, or alternative update paths can still introduce inconsistencies.  
D. No; write-through never updates cache entries.

### Q53. A website has millions of products, but customers view only a small fraction. Which caching strategy is suitable?
A. Write-through for every product, regardless of access  
B. Lazy loading  
C. Disable caching permanently  
D. Store every product only in a Redis List

### Q54. Why might TTL be combined with lazy loading?
A. TTL makes the primary database unnecessary.  
B. Lazy loading caches requested values, while TTL limits how long those values remain in the cache.  
C. TTL guarantees immediate consistency after every database update.  
D. Lazy loading requires all keys to have no expiration.

## Section 6 — Integrated scenario questions

### Q55. A system must rank players by score and retrieve the top five efficiently. Which Redis data type should it use?
A. Set  
B. Sorted Set  
C. Hash  
D. HyperLogLog

### Q56. An application requires automatic promotion of a replica when the primary fails, but does not need to partition the dataset. Which architecture is suitable?
A. Standalone Redis  
B. Redis Sentinel with a primary–replica deployment  
C. Redis Cluster without replication  
D. A Redis List with TTL

### Q57. A growing application exceeds the storage capacity of one Redis instance. Which architecture may help distribute storage and workload?
A. Standalone Redis  
B. Redis Cluster  
C. Sentinel alone  
D. AOF alone

### Q58. A service needs compact backups and more granular recovery. Which persistence approach should it consider?
A. RDB only  
B. AOF only  
C. Both RDB and AOF  
D. Neither RDB nor AOF

### Q59. A cached product price remains outdated after the primary database has been updated. What is the problem and a suitable remedy?
A. The cache is stale; use invalidation, update the cache in the write flow, or use TTL to limit the stale-value window.  
B. The cache is corrupted; disable all persistence.  
C. The cache is sharded; switch to a List.  
D. The database is unavailable; add more hash slots.

### Q60. An application caches every record immediately after it is written, even though most records are never read. What is the main disadvantage?
A. It prevents Redis from supporting replication.  
B. It can consume cache memory unnecessarily and adds cache-update work.  
C. It makes every cache hit slower than a database query.  
D. It disables TTL.

---

# Answer Key and Explanations

| Question | Correct answer | Explanation |
|---:|:---:|---|
| 1 | B | An in-memory database primarily accesses data in RAM, avoiding disk access for every operation. Actual performance also depends on networking, serialization, query complexity, concurrency, and configuration. |
| 2 | B | The four models are document, key-value, column-based, and graph. Each supports different data structures and access patterns. |
| 3 | B | During a network partition, a distributed system cannot guarantee both full consistency and full availability for every request. |
| 4 | B | Frequently requested, expensive query results are good cache candidates because reuse avoids repeated retrieval or computation. |
| 5 | B | A heavily read, infrequently changed catalogue can benefit from caching. Changes should refresh or invalidate affected entries, or be bounded by an appropriate TTL. |
| 6 | B | A unique session identifier enables direct key-based retrieval, and TTL can expire sessions automatically. |
| 7 | C | RAM alone does not provide restart durability. Recovery depends on configured persistence, including RDB snapshots and AOF. |
| 8 | B | Redis is not automatically superior for every workload. The choice depends on functionality, workload, configuration, complexity, and measured performance. |
| 9 | A | A cache hit occurs when the requested value exists in Redis and can be returned. |
| 10 | B | Redis can serve frequently requested values from memory, reducing repeated queries to the persistent relational database. |
| 11 | B | RDB saves point-in-time snapshots of the Redis dataset in a binary file. |
| 12 | B | RDB snapshots are relatively compact and useful for backups. |
| 13 | B | Writes made after the latest usable snapshot may be missing from recovery. |
| 14 | A | AOF records write operations and reconstructs the dataset by replaying them during recovery. |
| 15 | B | RDB is snapshot-based; AOF is an operation log. Redis can use both. |
| 16 | B | A snapshot represents a point in time. Changes made afterward may be absent from the latest usable snapshot. |
| 17 | A | AOF records writes between snapshots, potentially allowing more granular recovery. Unsynchronized recent entries may still be lost. |
| 18 | B | AOF can grow, require longer replay during recovery, and add logging or disk synchronization overhead. |
| 19 | B | In RESP, `*3` indicates an array containing three elements: `SET`, `hello`, and `world`. |
| 20 | A | `$5` indicates that the following bulk string contains five bytes. RESP length markers count bytes, not necessarily characters. |
| 21 | B | A standalone instance is a single point of failure and has no built-in replica redundancy within that deployment. |
| 22 | B | In the described primary–replica architecture, the primary receives writes and replicas copy its dataset. |
| 23 | B | Sentinel monitors primary–replica deployments and coordinates automatic failover. |
| 24 | B | Replication copies data; Sentinel monitors availability and coordinates failover. |
| 25 | C | Sentinel does not shard data. Redis Cluster distributes keys across nodes using hash slots. |
| 26 | C | Redis Cluster uses 16,384 hash slots, numbered from 0 through 16,383. |
| 27 | B | The slot is calculated using `CRC16(key) mod 16384`. |
| 28 | A | Cluster distributes data and workload across nodes, supporting horizontal scalability and greater aggregate storage capacity. |
| 29 | B | Replication maintains copies of the same data; sharding distributes different portions of the dataset. |
| 30 | B | Vertical scaling adds resources to an existing machine. |
| 31 | A | Asynchronous replication can mean a replica has not yet received or applied the latest primary write. |
| 32 | B | Accidental deletion, logical errors, or corruption can propagate to replicas, so independent backups remain important. |
| 33 | C | Sorted Sets associate unique members with numerical scores and support ranking. |
| 34 | A | A Redis Hash stores multiple string field–value pairs under one Redis key. |
| 35 | B | Lists preserve order and allow duplicates; Sets contain unique members without guaranteed order. |
| 36 | B | Lists preserve sequence and allow duplicate tasks, making them suitable for queue patterns. |
| 37 | A | Sets enforce unique membership, so a visitor added more than once appears only once. |
| 38 | C | Sorted Sets maintain unique members with scores and support score-based ordering. |
| 39 | B | Redis Strings store byte sequences, including text, numeric values used by supported operations, serialized objects, and binary data. |
| 40 | A | Bitmaps provide bit-level operations over strings; Bitfields work with integer fields at specified bit offsets. |
| 41 | B | HyperLogLog provides memory-efficient approximate cardinality counting. |
| 42 | A | Redis geospatial indexes store locations and support geographic queries. |
| 43 | A | Streams store append-oriented event entries used in event processing and messaging. |
| 44 | B | TTL means Time to Live and specifies how long a key remains valid before expiration. |
| 45 | B | `TTL` returning `-1` means the key exists but has no expiration set. |
| 46 | A | `TTL` returning `-2` means the key does not exist. |
| 47 | B | An expired key is no longer available as a valid entry. The application, not expiration itself, performs any database lookup and cache repopulation. |
| 48 | A | TTL is time-based; invalidation removes or refreshes data when it becomes invalid, often after a source-data change. |
| 49 | A | Lazy loading/cache-aside populates Redis on demand after a cache miss. |
| 50 | B | Only requested values are cached, avoiding unnecessary cache population. |
| 51 | A | Write-through updates the primary database and the corresponding cache entry in the same application write flow. |
| 52 | C | Cache-update failures, concurrent operations, and alternative update paths can still cause inconsistency. |
| 53 | B | Lazy loading is suitable when only a small fraction of a large catalogue is actually accessed. |
| 54 | B | Lazy loading controls which values enter the cache; TTL limits how long cached values remain there. |
| 55 | B | Sorted Sets support score-based ranking and efficient retrieval of top-ranked members. |
| 56 | B | Sentinel monitors a primary–replica deployment and coordinates promotion of a replica when the primary is judged unavailable. |
| 57 | B | Redis Cluster distributes keys across hash slots and nodes, supporting horizontal storage and workload scaling. |
| 58 | C | RDB offers compact point-in-time snapshots; AOF records writes for more granular recovery. They provide complementary options. |
| 59 | A | The cache is stale. Invalidation, cache updates in the write flow, or TTL can address the problem; TTL alone does not guarantee immediate consistency. |
| 60 | B | Populating entries that are rarely read can waste memory and adds cache-update work. Lazy loading may be more efficient for such workloads. |

---

## Suggested 3-Exam Structure

For practice sessions, the 60 questions can be divided into three exams of 20 questions:

- **Exam 1 — Questions 1–20:** In-memory databases, NoSQL, caching fundamentals, RDB, AOF, and RESP.
- **Exam 2 — Questions 21–40:** Redis architectures, replication, Sentinel, Cluster, scaling, and core data types.
- **Exam 3 — Questions 41–60:** Advanced data types, TTL, cache synchronization, and integrated scenarios.

**Practice note:** The answer key is placed after all questions so the document can be used as a self-test. For an interactive exam, hide the answer key until submission.
