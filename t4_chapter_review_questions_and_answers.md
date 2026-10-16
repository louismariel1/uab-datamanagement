## Document 2 — `t4_review_questions_and_anwers.md`

Writing

# T4 — Redis Review Questions and Answers

## Exam Practice and Explanations

**Purpose:** Consolidated review questions covering Redis fundamentals, caching, persistence, deployment architectures, data types, and practical scenarios.

**Note:** These questions are compiled from the supplied lecture revision material. The material does not establish that they were explicitly suggested by the lecturer or confirmed as actual examination questions.

## Section 1 — In-memory databases and NoSQL

### Q1. Why can an in-memory database be faster than a disk-oriented database?

**Answer:** An in-memory database primarily accesses data in RAM, avoiding disk access for every operation.

**Explanation:** RAM generally has much lower access latency than disk storage. However, actual performance also depends on networking, serialization, query complexity, concurrency, and configuration.

### Q2. Name the four major NoSQL data models and explain their differences.

**Answer:**

- **Document:** Stores semi-structured documents, often JSON-like objects.
- **Key-value:** Stores values accessed using unique keys.
- **Column-based:** Organizes data into columns or column families.
- **Graph:** Represents entities as nodes and relationships as edges.

**Explanation:** Each model supports different access patterns. The appropriate choice depends on the application's data structure, query requirements, scalability needs, and consistency requirements.

### Q3. Explain ACID, BASE, and the CAP theorem.

**Answer:**

- **ACID:** Atomicity, Consistency, Isolation, and Durability.
- **BASE:** Basically Available, Soft state, and Eventual consistency.
- **CAP:** During a network partition, a distributed system cannot guarantee both full consistency and full availability for every request.

**Explanation:** ACID emphasizes reliable transaction properties. BASE describes an approach that tolerates temporary inconsistency while prioritizing availability. CAP concerns the trade-offs a distributed system must make during a network partition.

### Q4. Name three suitable candidates for caching and explain why they are appropriate.

**Answer:**

1. Frequently accessed static content.
2. Results of expensive computations.
3. Frequently requested, expensive database queries.

**Explanation:** Caching is beneficial when reusing a result avoids substantial repeated retrieval or computation. Data that changes frequently can also be cached if its freshness requirements are handled appropriately.

### Q5. A product catalogue is requested thousands of times but changes only occasionally. Would caching help?

**Answer:** Yes.

**Explanation:** Redis can serve repeated requests from memory, reducing response time and database load. When products change, the application should refresh or invalidate affected cache entries, or use an appropriate TTL.

### Q6. What is a key-value database, and why is it suitable for storing user sessions?

**Answer:** A key-value database stores values under unique keys. Each session can be stored under a unique session identifier.

**Explanation:** Direct key-based retrieval is efficient for predictable lookups. TTL can also be used to expire sessions automatically after a defined period.

### Q7. Does storing data in RAM guarantee that it survives a server restart?

**Answer:** No.

**Explanation:** Recovery depends on persistence configuration and the failure scenario. Redis supports RDB snapshots and AOF logs, but their protection depends on when data was persisted and how persistence was configured.

### Q8. Is Redis always better than Memcached because it supports more features?

**Answer:** No.

**Explanation:** Redis supports richer data structures and persistence, while Memcached is suited to straightforward caching. The best choice depends on required functionality, workload, configuration, operational complexity, and measured performance.

## Section 2 — Redis caching and persistence

### Q9. What is a cache hit, and what is a cache miss?

**Answer:**

- **Cache hit:** The requested value exists in Redis and can be returned.
- **Cache miss:** The requested value is absent, so the application must retrieve it from another source.

**Explanation:** In a cache-aside workflow, the application commonly queries the primary database after a miss and populates Redis with the result for subsequent requests.

### Q10. Why use Redis in front of MySQL or PostgreSQL?

**Answer:** To serve frequently requested data from memory, reduce repeated database queries, and improve response times.

**Explanation:** Redis can accelerate reads without replacing the persistent database, which commonly remains the authoritative source. The application must still manage cache updates and consistency.

### Q11. Explain RDB persistence and give two advantages and two disadvantages.

**Answer:** RDB saves point-in-time snapshots of the Redis dataset in a binary file.

Advantages:

- Produces relatively compact snapshots suitable for backups.
- Generally enables fast restoration by loading a saved dataset.

Disadvantages:

- Changes since the last usable snapshot may be lost.
- Periodic snapshots provide less granular recovery than an operation log.

**Explanation:** RDB is useful when compact backups and restoration speed are important, but the snapshot interval determines the potential loss of recent changes.

### Q12. Explain AOF persistence.

**Answer:** AOF records write operations in a log. Redis can replay the recorded commands to reconstruct its dataset during recovery.

**Explanation:** Because AOF records operations between snapshots, it can provide finer-grained recovery. The amount of recent data protected depends on the synchronization policy and whether writes have reached durable storage.

### Q13. Compare RDB and AOF.

**Answer:**

| Feature | RDB | AOF |
| --- | --- | --- |
| Storage method | Point-in-time snapshot | Write-operation log |
| Recovery | Load snapshot | Replay commands |
| File size | Usually compact | Can grow over time |
| Restart time | Generally faster | May take longer |
| Potential data loss | Changes after the last usable snapshot | Depends on synchronization policy |
| Main strength | Backups and fast restoration | Finer-grained recovery |

**Explanation:** RDB prioritizes compact snapshots and restoration speed. AOF provides more granular recovery at the cost of possible file growth and replay overhead. Redis can use both mechanisms.

### Q14. A Redis server fails after the second RDB snapshot but before the third. What data might be lost?

**Answer:** Changes made after the second snapshot may be absent from the latest usable snapshot.

**Explanation:** RDB represents a point-in-time dataset, not a continuous history of every write. Recovery restores the state represented by the available snapshot.

### Q15. Why can AOF be more protective against recent data loss than periodic RDB snapshots?

**Answer:** AOF records write operations between snapshots, whereas RDB records the dataset at particular points in time.

**Explanation:** The protection is not absolute. Recent AOF entries that have not been synchronized to durable storage can still be lost, depending on the failure and configuration.

### Q16. State three disadvantages of AOF persistence.

**Answer:**

1. The log can grow larger than an RDB snapshot.
2. Recovery can take longer because commands must be replayed.
3. Logging and disk synchronization can introduce performance overhead.

**Explanation:** The impact depends on write volume, hardware, synchronization policy, and whether AOF rewriting is used to reduce log size.

### Q17. What is the RESP representation of `SET hello world`?

**Answer:**

```
*3
$3
SET
$5
hello
$5
world
```

**Explanation:**

- `*3` means the array contains three elements.
- `$3` indicates that the next bulk-string argument contains three bytes.
- `SET` is the command.
- `$5` indicates five bytes for each of the following arguments.
- `hello` is the key, and `world` is the value.

In actual RESP encoding, lines are terminated by CRLF (`\r\n`). Length markers represent bytes rather than necessarily representing the number of characters.

## Section 3 — Redis architectures and scalability

### Q18. What is the main disadvantage of Standalone Redis?

**Answer:** It has a single point of failure and no built-in replica redundancy within the standalone deployment.

**Explanation:** If the instance becomes unavailable, Redis service may be interrupted until recovery. Standalone is simple but does not itself provide distributed redundancy or automatic failover.

### Q19. Explain primary–replica architecture.

**Answer:** A primary receives writes, and one or more replicas copy the primary's dataset. Replicas may also serve reads when the application permits it.

**Explanation:** Replication provides redundant copies and can distribute read traffic. However, replication is generally asynchronous, so replicas may lag behind the primary and return stale data.

### Q20. What is Redis Sentinel, and what are its main functions?

**Answer:** Sentinel monitors a primary–replica deployment and coordinates automatic failover.

Its main functions are:

1. Monitoring node availability.
2. Sending notifications about failures and changes.
3. Coordinating the promotion of a suitable replica when the primary is judged unavailable.

**Explanation:** Sentinel reduces the need for manual intervention, but failover takes time and does not guarantee zero downtime or zero data loss.

### Q21. What is the difference between primary–replica architecture and Sentinel?

**Answer:** Primary–replica architecture replicates data; Sentinel monitors that deployment and coordinates automatic failover.

**Explanation:** Replication maintains copies of the dataset, while Sentinel helps restore write availability after a primary failure. Replication alone does not automatically promote a replacement primary.

### Q22. Does Redis Sentinel provide sharding?

**Answer:** No.

**Explanation:** Sentinel manages monitoring and failover for a primary–replica deployment. Redis Cluster distributes data across multiple nodes using hash slots.

### Q23. Explain Redis Cluster's hash-slot mechanism.

**Answer:** Redis Cluster maps each key to one of 16,384 hash slots, numbered from 0 to 16,383. Those slots are assigned to cluster nodes.

The formula is:

$$
\text{HASH\_SLOT}=\operatorname{CRC16}(\text{key})\bmod 16384
$$

**Explanation:** Redis calculates the key's CRC16 hash and maps it to a slot using modulo 16,384. The cluster routes the operation to the node responsible for that slot.

### Q24. Why use Redis Cluster rather than Standalone Redis?

**Answer:** Redis Cluster can distribute data and workload across multiple nodes, supporting horizontal scalability and greater aggregate storage capacity.

**Explanation:** The cluster can spread requests across nodes and reassign slots as requirements change. However, it introduces additional routing, rebalancing, and operational complexity.

### Q25. Explain the difference between replication and sharding.

**Answer:**

- **Replication:** Maintains copies of the same data on multiple nodes.
- **Sharding:** Distributes different portions of the dataset across multiple nodes.

**Explanation:** Replication supports redundancy and potential read scaling. Sharding supports horizontal scalability and distributed storage capacity. A deployment can use both.

### Q26. What is the difference between horizontal and vertical scaling?

**Answer:**

- **Vertical scaling:** Adds resources to an existing machine.
- **Horizontal scaling:** Adds machines or nodes to distribute workload or data.

**Explanation:** Horizontal scaling can increase aggregate capacity but introduces network overhead, routing, rebalancing, and distributed-system complexity.

### Q27. Why might a Redis replica return stale data?

**Answer:** Replication may be asynchronous, so the replica may not yet have received or applied the latest write from the primary.

**Explanation:** Applications requiring the most recent value must account for replica lag and choose a read strategy appropriate to their consistency requirements.

### Q28. Why should replicated data still be backed up?

**Answer:** Replication does not protect against every type of data loss.

**Explanation:** Accidental deletion, logical mistakes, and corruption can propagate to replicas. Independent backups and tested recovery procedures provide an additional layer of protection.

### Q29. What does the CAP theorem imply during a network partition?

**Answer:** A distributed system cannot guarantee both full consistency and full availability for every request during the partition.

**Explanation:** A system may reject or delay requests to preserve consistency, or continue responding while accepting a risk of stale or inconsistent data. Actual Redis behavior depends on the architecture, configuration, and operation.

## Section 4 — Redis data types

### Q30. Which Redis data type is appropriate for a leaderboard?

**Answer:** Sorted Set.

**Explanation:** Sorted Sets associate unique members with numerical scores and support score-based ordering and ranking.

Example:

```
ZADD leaderboard 100 "Alice"
ZADD leaderboard 250 "Bob"
ZREVRANGE leaderboard 0 -1 WITHSCORES
```

The reverse-range command returns members in descending score order.

### Q31. What is a Redis Hash, and when should it be used?

**Answer:** A Hash stores multiple string field–value pairs under one Redis key.

**Explanation:** It is suitable for objects such as user profiles, product records, and application settings because individual fields can be retrieved or updated separately.

Example:

```
HSET user:1001 name "Eduardo" age "25"
HGET user:1001 name
HGETALL user:1001
```

### Q32. Explain the difference between a List and a Set.

**Answer:** A List preserves order and permits duplicates. A Set contains unique members without guaranteed order.

**Explanation:** Use a List for ordered sequences, queues, and activity feeds. Use a Set for unique membership, such as unique visitors or group membership.

### Q33. Which data type is appropriate for a queue where order matters and duplicate tasks are allowed?

**Answer:** List.

**Explanation:** Lists preserve element order and allow duplicates. Push and pop operations at the left and right ends can implement queue or stack behavior.

### Q34. Which data type should be used to store a unique collection of website visitors?

**Answer:** Set.

**Explanation:** Sets prevent duplicate members and support membership checks, unions, intersections, and differences.

Example:

```
SADD visitors "user-1" "user-2" "user-1"
SMEMBERS visitors
```

The Set contains only two unique members.

### Q35. Compare Hashes, Lists, Sets, and Sorted Sets.

**Answer:**

| Data type | Main property | Example |
| --- | --- | --- |
| Hash | Field–value pairs | User profile |
| List | Ordered sequence; duplicates allowed | Job queue |
| Set | Unique unordered members | Unique visitors |
| Sorted Set | Unique members ordered by score | Leaderboard |

**Explanation:** Choose the type according to the required operations. Similar underlying values do not imply identical behavior.

### Q36. What are Redis Strings used for?

**Answer:** Strings store sequences of bytes, including text, numbers used by supported numeric operations, serialized objects, and binary data.

**Explanation:** They are general-purpose values for session tokens, counters, cached responses, and other simple key-based data. A single Redis string can hold up to 512 MB by default.

### Q37. What are Redis Bitmaps and Bitfields used for?

**Answer:**

- **Bitmaps:** Bit-level operations over string values, such as compact activity flags.
- **Bitfields:** Reading and modifying integer fields at specified bit offsets, such as packed counters.

**Explanation:** Both are useful when compact representations and bit-level operations are more appropriate than separate keys for every value.

### Q38. What are HyperLogLog, Geospatial indexes, and Streams used for?

**Answer:**

- **HyperLogLog:** Approximate counting of distinct elements.
- **Geospatial indexes:** Storing locations and performing geographic queries.
- **Streams:** Storing append-oriented event entries for event processing and messaging.

**Explanation:** HyperLogLog trades exactness for memory-efficient cardinality estimation. Geospatial indexes support location-based queries, while Streams organize event records with identifiers.

## Section 5 — TTL and cache synchronization

### Q39. What does TTL mean in Redis?

**Answer:** TTL means Time to Live. It specifies how long a key remains valid before expiration.

**Explanation:** TTL is useful for sessions, temporary tokens, and cache entries that should not remain valid indefinitely.

Example:

```
SET session:1001 "active" EX 60
TTL session:1001
```

The `EX 60` option sets an expiration of 60 seconds.

### Q40. What do the results `TTL key = -1` and `TTL key = -2` mean?

**Answer:**

- `-1`: The key exists but has no expiration set.
- `-2`: The key does not exist.

**Explanation:** A key without a TTL can remain in memory indefinitely until deleted or removed by an applicable eviction policy. It does not automatically expire merely because memory is needed.

### Q41. What happens when a cached key expires?

**Answer:** The key is no longer available as a valid entry. The application may retrieve the value from the primary database and repopulate Redis.

**Explanation:** Expiration does not itself trigger a database lookup. The application must implement retrieval and cache population.

### Q42. What is the difference between TTL and cache invalidation?

**Answer:** TTL expires a key after a configured lifetime. Cache invalidation removes or refreshes a value when it is no longer valid, often because the underlying data has changed.

**Explanation:** TTL is time-based. Invalidation can respond directly to data changes. Both can be used together to reduce the risk of stale cache values.

### Q43. What is lazy loading, and how does it work?

**Answer:** Lazy loading, commonly implemented as cache-aside, populates the cache only when requested data is absent.

**Workflow:**

1. Check Redis.
2. Return the cached value on a hit.
3. Query the primary database on a miss.
4. Return the result and populate Redis.

**Explanation:** This approach avoids caching records nobody requests, but the first request for a missing key incurs database latency.

### Q44. State two advantages and two disadvantages of lazy loading.

**Answer:**

Advantages:

- Only requested data is cached.
- Unnecessary cache population is avoided.

Disadvantages:

- Cache misses require a database lookup.
- Database updates can leave existing cached values stale.

**Explanation:** Lazy loading suits read-heavy applications with a limited set of frequently accessed records, provided expiration and invalidation are managed.

### Q45. What is write-through caching?

**Answer:** Write-through is a strategy in which the primary database is updated and the corresponding cache entry is updated as part of the same application write flow.

**Explanation:** Successful cache updates can keep frequently accessed values fresher, but every relevant write adds cache-update work. Failures between the database and cache updates can still create inconsistency.

### Q46. Compare lazy loading and write-through caching.

**Answer:**

| Feature | Lazy loading | Write-through |
| --- | --- | --- |
| Cache population | On demand after a miss | During or after relevant writes |
| Memory efficiency | Generally better for rarely accessed records | May cache records that are never read |
| Initial cache miss | Requires database lookup | May be avoided for recently written records |
| Write overhead | No required cache population on every write | Requires corresponding cache-update work |
| Freshness | Requires expiration, refresh, or invalidation | Can improve freshness when cache updates succeed |

**Explanation:** Lazy loading prioritizes on-demand memory efficiency. Write-through can help maintain fresh cached values when records are both frequently accessed and frequently updated.

### Q47. Does write-through guarantee that the cache is never stale?

**Answer:** No.

**Explanation:** Cache updates can fail, and concurrent operations or alternative update paths can introduce inconsistencies. Appropriate error handling, retries, and invalidation mechanisms may be required.

### Q48. A website contains millions of products, but customers view only a small fraction. Which caching strategy is suitable?

**Answer:** Lazy loading.

**Explanation:** Only requested products enter the cache, reducing unnecessary memory use. TTL and explicit invalidation can limit how long cached product data remains outdated.

### Q49. A service frequently reads recently updated account records and prioritizes cache freshness. Which caching strategy might be appropriate?

**Answer:** Write-through caching.

**Explanation:** Updating the cache alongside relevant database writes can reduce the chance that subsequent reads return older values. The implementation must still handle cache-update failures and concurrent changes.

### Q50. Why might TTL be combined with lazy loading?

**Answer:** Lazy loading caches only requested values, while TTL limits how long those values remain in the cache.

**Explanation:** Combining them can improve memory efficiency and limit the lifetime of potentially stale values. Explicit invalidation may still be necessary when changes must be reflected immediately.

## Section 6 — Integrated scenario questions

### Q51. A system must rank players by score and retrieve the top five efficiently. Which Redis data type should it use?

**Answer:** Sorted Set.

**Explanation:** Each player can be stored as a unique member associated with a score. Sorted Set operations support score-based ordering and ranking.

### Q52. An application requires automatic promotion of a replica when the primary fails, but it does not need to partition the dataset across multiple nodes. Which architecture is suitable?

**Answer:** Redis Sentinel with a primary–replica deployment.

**Explanation:** Sentinel monitors the deployment and coordinates failover. Redis Cluster is the architecture designed to distribute data across hash slots and nodes.

### Q53. A growing application exceeds the storage capacity of one Redis instance. Which architecture may help?

**Answer:** Redis Cluster.

**Explanation:** Cluster distributes keys across nodes using hash slots, allowing the system to scale storage and workload horizontally. Capacity planning, rebalancing, replication, and routing still require attention.

### Q54. A small application needs a simple cache without automatic failover. Which architecture may be sufficient?

**Answer:** Standalone Redis.

**Explanation:** Standalone is straightforward to deploy and operate. The trade-off is that the instance is a single point of failure.

### Q55. A service needs both compact backups and more granular recovery. Which persistence approach should it consider?

**Answer:** Using both RDB and AOF.

**Explanation:** RDB provides point-in-time snapshots, while AOF records write operations. Their combined use can provide complementary recovery options, subject to configuration and operational requirements.

### Q56. A cache contains an outdated product price after the primary database has been updated. Explain the problem and suggest solutions.

**Answer:** The cached value is stale because the cache was not refreshed or invalidated after the database change.

**Explanation:** Possible solutions include explicit invalidation when the product changes, updating the cache as part of the write flow, or using a TTL to limit the stale-value window. TTL alone does not guarantee immediate consistency.

### Q57. An application requires durable recovery after failures. Is primary–replica replication alone sufficient?

**Answer:** No.

**Explanation:** Replication supports redundancy and availability, but asynchronous replication may lag, and logical errors can propagate to replicas. Appropriate persistence, independent backups, and tested recovery procedures are also important.

### Q58. A system must handle an increasing number of requests. Explain how horizontal scaling can help and identify a trade-off.

**Answer:** Adding nodes can distribute requests and data across more machines, increasing aggregate capacity.

**Explanation:** The system needs suitable partitioning or routing. Horizontal scaling introduces network overhead, rebalancing, operational complexity, and distributed consistency considerations.

### Q59. A workload consists of frequently read, rarely updated user profiles. Which caching approach may be suitable?

**Answer:** Lazy loading with an appropriate TTL and invalidation policy.

**Explanation:** Frequently requested profiles can be cached on demand. TTL limits their lifetime, while invalidation or refresh handles profile changes that must be reflected sooner.

### Q60. A system caches every record after it is written, even though most records are never read. What is the main disadvantage?

**Answer:** Unnecessary cache memory consumption and additional cache-update work.

**Explanation:** Write-through can populate the cache with records that provide little performance benefit. Lazy loading may be more efficient when only a small proportion of records are accessed.
