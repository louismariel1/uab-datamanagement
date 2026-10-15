Understood. I’ll consolidate all four supplied documents into two separate Markdown documents:

1. `t4_redis_introduction_and_persistence.md` — the complete, deduplicated exam revision guide, combining Redis fundamentals, caching, persistence, architectures, data types, and practical examples. The document itself will contain no references to chunks or source-part numbers.
2. `t4_review_questions_and_anwers.md` — a separate question bank with answers and explanations, combining the review questions throughout the supplied material and removing duplicates.

One distinction: the supplied notes identify their questions as practice questions derived from the lecture, rather than confirmed questions explicitly suggested by the lecturer. I’ll preserve that distinction rather than mislabeling them.

## Document 1 — `t4_redis_introduction_and_persistence.md`

Writing

# T4 — Redis Introduction and Persistence

## Exam Revision Guide

## 1\. Learning objectives

You should be able to:

- Explain NoSQL database models and the advantages and limitations of in-memory databases.
- Explain Redis's role as a database, cache, message broker, and streaming platform.
- Describe key-value storage and identify suitable Redis data types.
- Explain cache hits, cache misses, TTL, cache invalidation, lazy loading, and write-through caching.
- Compare RDB and AOF persistence, including their performance and recovery trade-offs.
- Interpret the basic RESP representation of a Redis command.
- Compare Standalone, primary–replica, Sentinel, and Cluster architectures.
- Explain replication, failover, sharding, hash slots, availability, and horizontal scalability.
- Select appropriate Redis features for practical application scenarios.

## 2\. NoSQL database fundamentals

NoSQL databases support flexible data models, performance, and scalability for different application requirements.

| Data model | Main idea | Typical use |
| --- | --- | --- |
| Document | Stores semi-structured documents, commonly JSON-like objects | Product catalogues and user profiles |
| Key-value | Stores values accessed through unique keys | Caching, sessions, and counters |
| Column-based | Organizes data into columns or column families | Large-scale analytical and distributed workloads |
| Graph | Represents entities as nodes and relationships as edges | Social networks and relationship analysis |

### Important concepts

- **ACID:** Atomicity, Consistency, Isolation, and Durability. These properties support reliable transaction processing.
- **BASE:** Basically Available, Soft state, and Eventual consistency. This describes an approach that prioritizes availability and tolerates temporary inconsistency.
- **CAP theorem:** During a network partition, a distributed system cannot guarantee both full consistency and full availability for every request. Its behavior depends on its design and consistency requirements.

**Exam takeaway:** No database model is universally best. Select a database according to access patterns, consistency requirements, performance, capacity, and scalability needs.

## 3\. In-memory databases and Redis

An in-memory database primarily stores and processes data in RAM rather than requiring disk access for every operation.

Redis is an in-memory data store commonly used as a database, cache, message broker, and streaming platform.

### Why Redis is fast

- RAM generally has much lower access latency than disk storage.
- Frequently requested values can be retrieved without repeatedly querying a persistent database.
- Simple key-based access and specialized data structures support efficient operations.
- Caching reduces repeated computation and database access.

Actual application performance also depends on network latency, data serialization, workload, configuration, and operation complexity.

### Advantages

- Very fast data access and low latency.
- Suitable for frequent reads, rapid updates, and real-time applications.
- Reduces load on underlying databases when used as a cache.
- Supports counters, sessions, rankings, queues, and event-processing workloads.

### Limitations

- RAM is generally more expensive per unit of capacity than disk storage.
- Available memory constrains the data that can remain in memory.
- In-memory storage does not automatically guarantee data recovery after a failure.
- Persistence, replication, backups, and distributed deployments introduce additional complexity.

**Critical distinction:** In-memory does not mean volatile-only. Redis supports persistence through RDB snapshots and AOF operation logs.

## 4\. Caching fundamentals

A cache stores a reusable copy of data so an application can retrieve it faster than recomputing it or fetching it from the original source.

### What should be cached?

- Frequently accessed, relatively static content, such as product information, images, CSS, and JavaScript.
- Expensive computational results, such as recommendations or simulation outputs.
- Frequently requested database query results that are expensive to calculate.
- User sessions, frequently accessed profiles, and API responses.

The best candidates generally combine frequent access with expensive retrieval or computation.

### Benefits

- Reduces response time.
- Reduces repeated database queries and computations.
- Lowers load on the primary database.
- Improves throughput for repeated requests.

### Trade-offs

- Cached values can become stale when the underlying data changes.
- Cache misses require additional work.
- Cached entries consume memory.
- Expiration and invalidation must be designed carefully.

The primary database commonly remains the authoritative data source, while Redis holds copies for faster access.

### Cache hits and misses

- **Cache hit:** The requested key exists in the cache, so its value can be returned.
- **Cache miss:** The requested value is absent, so the application must retrieve it elsewhere, commonly from the primary database.

A typical cache-aside read flow is:

1. The application checks Redis.
2. If the key exists, it returns the cached value.
3. If the key is absent, the application queries the primary database.
4. The application returns the result and populates Redis for future requests.

## 5\. Key-value storage

A key-value database stores values under unique keys.

Example:

```
user:1001 -> "Eduardo"
```

Here, `user:1001` is the key and `Eduardo` is the value.

A Python dictionary provides a useful analogy:

```
users = {
    "user:1001": "Eduardo"
}
```

### Key-value characteristics

- Keys identify values and must be unique within the relevant keyspace.
- Direct key-based retrieval is generally efficient.
- Values can have different representations depending on the database.
- The model suits sessions, caching, counters, and predictable access patterns.

Redis extends basic key-value storage with Strings, Hashes, Lists, Sets, Sorted Sets, and other specialized data types.

**Exam distinction:** Redis is not restricted to storing plain text strings.

## 6\. Redis read and write flows

### Read flow

1. The application requests a key.
2. Redis is checked.
3. On a hit, the cached value is returned.
4. On a miss, the application retrieves the value from the persistent database.
5. The application may populate Redis with the retrieved value.

### Write flow

A common application-managed write strategy is:

1. Update the primary database.
2. Update or invalidate the corresponding cache entry.
3. Handle failures so the cache does not remain inconsistent with the database.

The precise ordering depends on the caching strategy and implementation.

**Important:** Updating Redis and the database is not automatically an atomic operation. A failed cache update, concurrent writes, or an alternative write path can leave stale data. Applications need suitable synchronization, retry, or invalidation mechanisms.

## 7\. Redis persistence

Redis supports two major persistence mechanisms:

- **RDB (Redis Database file):** Saves point-in-time snapshots of the dataset.
- **AOF (Append Only File):** Records write operations so the dataset can be reconstructed by replaying logged commands.

These mechanisms address the risk of losing data when an in-memory process stops or a machine fails.

### 7.1 RDB persistence

RDB saves snapshots at configured intervals or when triggered manually. The snapshot is stored in a binary file that Redis can load during recovery.

#### Advantages

- Produces relatively compact snapshots.
- Supports backups and point-in-time recovery.
- Generally enables faster restarts than replaying a long operation log.
- Snapshot generation can occur in the background, although forking and resource usage may temporarily affect performance.

#### Disadvantages

- Changes made after the last successful snapshot may be lost.
- Provides less granular recovery than an operation log.
- Periodic snapshots may not meet requirements where losing recent writes is unacceptable.

#### Example timeline

```
Snapshot t1 -------- Snapshot t2 -------- Failure -------- Snapshot t3
```

If a failure occurs after `t2` but before `t3`, changes made after `t2` may be absent from the latest usable snapshot.

**Memory trick:** RDB = Regular snapshots, Rapid restart, Risk of losing recent changes.

### 7.2 AOF persistence

AOF records write operations in a log. During recovery, Redis replays the recorded commands to reconstruct the dataset.

#### Advantages

- Provides more granular recovery than periodic snapshots.
- Can reduce the data-loss window between persistence events.
- Records operations that can rebuild the dataset.

#### Disadvantages

- The log can grow over time and may be larger than an RDB snapshot.
- Recovery may take longer because Redis must replay commands.
- Persistence can introduce write-related overhead.
- Recent writes can still be lost if they have not been synchronized to disk.

AOF rewriting can reduce log size by producing a more compact representation of the current dataset.

#### Fsync and durability

The AOF durability guarantee depends on the configured synchronization policy:

- More frequent synchronization generally reduces the potential loss of recent writes but can increase disk I/O.
- Less frequent synchronization can improve performance but may increase the amount of data lost during a failure.

**Important:** AOF is not automatically lossless. Its recovery protection depends on configuration and the failure scenario.

### 7.3 RDB versus AOF

| Feature | RDB | AOF |
| --- | --- | --- |
| Method | Point-in-time snapshots | Logs write operations |
| Representation | Binary dataset snapshot | Recorded commands |
| Recovery | Load a snapshot | Replay logged commands |
| File size | Usually compact | Can grow over time |
| Restart time | Generally faster | May require longer replay |
| Data-loss risk | Changes since the last usable snapshot | Depends on synchronization policy |
| Main strength | Backups and fast restoration | More granular recovery |

### Choosing a persistence strategy

- Choose **RDB** when compact snapshots, backups, and fast restoration are priorities.
- Choose **AOF** when finer-grained recovery is important.
- Consider **both** when snapshots and operation logging provide useful complementary recovery options.

Replication is not a substitute for persistence or independent backups.

## 8\. RESP and AOF command representation

Redis uses the **Redis Serialization Protocol (RESP)** to represent commands and data exchanged with clients. AOF files record write operations in a format Redis can process during recovery.

Consider:

```
SET hello world
```

Its RESP array representation is:

```
*3
$3
SET
$5
hello
$5
world
```

| Marker or value | Meaning |
| --- | --- |
| `*3` | The array contains three elements. |
| `$3` | The next bulk-string argument contains 3 bytes. |
| `SET` | Command name. |
| `$5` | The next bulk-string argument contains 5 bytes. |
| `hello` | Key. |
| `$5` | The next bulk-string argument contains 5 bytes. |
| `world` | Value. |

In the actual RESP encoding, lines are terminated by CRLF (`\r\n`). Length markers specify bytes, not necessarily characters. For ASCII text such as `hello`, the byte count and character count are both five.

**Exam tip:** Understand `*3`, `$3`, and `$5`, and explain how logged commands can reconstruct a dataset.

## 9\. Redis deployment architectures

Redis deployments differ in simplicity, redundancy, failover, and horizontal scalability.

| Architecture | Main purpose | Key characteristic |
| --- | --- | --- |
| Standalone | Simplicity | One Redis instance |
| Primary–replica | Redundancy and possible read scaling | Replicas copy the primary's dataset |
| Sentinel | Monitoring and automatic failover | Coordinates promotion of a replica |
| Redis Cluster | Horizontal scalability and distributed storage | Partitions keys across hash slots |

### 9.1 Standalone

A Standalone deployment uses one Redis instance.

**Advantages:**

- Simple to install, configure, and operate.
- Suitable for development, small applications, and straightforward caching.

**Disadvantages:**

- A single point of failure.
- Capacity and workload are constrained by the instance's resources.
- No replica is automatically available to take over.

**Scenario:** A small application needs a simple cache and does not require automatic failover. Standalone may be appropriate.

### 9.2 Primary–replica architecture

A primary–replica deployment maintains one primary and one or more replicas.

How it works:

1. The primary normally receives writes.
2. Changes are propagated to replicas.
3. Replicas maintain copies of the primary's dataset.
4. Reads may be served by replicas when the application permits it.

**Advantages:**

- Provides redundant data copies.
- Can distribute read traffic.
- Provides additional recovery options if the primary fails.

**Limitations:**

- Replication is generally asynchronous, so replicas may lag.
- Replica reads can return stale values.
- Without a failover mechanism, primary failure can interrupt writes.
- Replication does not guarantee zero downtime or zero data loss.

### 9.3 Redis Sentinel

Sentinel provides monitoring and automatic failover for a primary–replica deployment.

Its main functions are:

1. **Monitoring:** Checks the availability of the primary and replicas.
2. **Notification:** Reports failures and changes in the deployment.
3. **Automatic failover:** Coordinates promotion of a suitable replica when the primary is judged unavailable.

Sentinel reduces the need for manual intervention, but failover takes time and does not guarantee zero data loss.

**Critical distinction:** Sentinel manages availability; it does not shard the dataset across nodes.

### 9.4 Redis Cluster

Redis Cluster distributes keys across multiple nodes using sharding and hash slots. It can also use replicas to improve fault tolerance.

#### Hash slots

Redis Cluster has **16,384 hash slots**, numbered from 0 to 16,383.

The slot calculation is:

$$
\text{HASH\_SLOT}=\operatorname{CRC16}(\text{key})\bmod 16384
$$

The process is:

1. Redis calculates the CRC16 hash of the key.
2. The result is mapped to a slot using modulo 16,384.
3. The cluster routes the operation to the node responsible for that slot.

Example distribution:

| Node | Example slot range | Number of slots |
| --- | --- | --- |
| Node 1 | 0–5460 | 5,461 |
| Node 2 | 5461–10922 | 5,462 |
| Node 3 | 10923–16383 | 5,461 |
| **Total** | **0–16383** | **16,384** |

This is an illustrative distribution; real slot assignments can differ.

#### Benefits

- Distributes data and requests across nodes.
- Supports horizontal scaling and greater aggregate capacity.
- Allows slots to be reassigned as capacity or workload changes.
- Can combine sharding with replication.

Redis Cluster uses a fixed set of hash slots rather than directly assigning keys using the classic consistent-hashing ring model.

**Key distinction:** Hashing determines a key's slot; slot assignment determines which node owns that slot.

Cluster operation still involves routing, rebalancing, replication, and failure-handling trade-offs. It does not guarantee strong consistency or zero data loss for every failure scenario.

## 10\. Reliability, availability, and scalability

### Replication versus sharding

- **Replication:** Maintains copies of data on multiple nodes. It primarily supports redundancy, fault tolerance, and possible read scaling.
- **Sharding:** Distributes different portions of data across nodes. It primarily supports horizontal scalability and distributed capacity.

A system can use both.

### Horizontal versus vertical scaling

- **Vertical scaling:** Adds resources to a single machine.
- **Horizontal scaling:** Adds machines or nodes to distribute workload or data.

Horizontal scaling can increase aggregate capacity but introduces network overhead, routing, rebalancing, and operational complexity.

### Availability versus consistency

Availability concerns whether a system can respond to requests. Consistency concerns whether responses reflect the required state of the data.

During a network partition, a distributed system must make trade-offs between full consistency and full availability.

Do not assume every Redis deployment has identical CAP behavior. The outcome depends on architecture, configuration, and the operation being performed.

### Replication versus backup

Replication copies data between operational nodes and helps maintain service after certain failures. It is not an independent backup.

Accidental deletion, logical mistakes, and corruption can propagate to replicas. Independent backups and tested recovery procedures remain important.

## 11\. Redis data types

Redis provides multiple data structures. Choose a type according to the operations and access patterns the application requires.

| Data type | Defining property | Typical use |
| --- | --- | --- |
| String | Byte sequence; can hold text, numbers, serialized values, or binary data | Cache entries, tokens, counters |
| Hash | Field–value mappings under one key | User profiles and object attributes |
| List | Ordered sequence; duplicates allowed | Queues, stacks, activity feeds |
| Set | Unique, unordered members | Unique visitors and group membership |
| Sorted Set | Unique members associated with scores and ordered by score | Leaderboards and rankings |
| Bitmap | Bit-level operations over string values | Activity flags |
| Bitfield | Operations on integer fields at specified bit offsets | Packed counters and compact records |
| Geospatial index | Geographic storage and queries | Nearby-place searches |
| HyperLogLog | Probabilistic cardinality estimation | Approximate distinct visitor counts |
| Stream | Append-oriented entries with identifiers | Event logs and messaging pipelines |

### 11.1 Strings

Strings are Redis's general-purpose value type and store sequences of bytes.

They can represent text, numbers used by supported numeric operations, serialized objects, and binary data. A single Redis string can hold up to **512 MB by default**.

Example:

```
SET user:1001 "Eduardo"
GET user:1001
```

`SET` stores the value under a key; `GET` retrieves it.

Typical uses include session tokens, cached API responses, counters, and serialized application data.

### 11.2 Hashes

A Hash stores string fields and values under one Redis key.

Example:

```
HSET user:1001 name "Eduardo" age "25" city "Singapore"
HGET user:1001 name
HGETALL user:1001
```

- `HSET` adds or updates fields.
- `HGET` retrieves one field.
- `HGETALL` retrieves all fields and values.

Hashes are useful for user profiles, product attributes, and application settings. Individual fields can be retrieved or updated without treating every attribute as a separate Redis key.

The lecture gives a theoretical maximum of $2^{32}-1$ field–value pairs per Hash.

### 11.3 Lists

Lists are ordered sequences of strings that permit duplicates and support operations at both ends.

Example:

```
LPUSH tasks "task-A"
RPUSH tasks "task-B"
LRANGE tasks 0 -1
```

Typical uses include job queues, stacks, and activity timelines.

- **Queue (FIFO):** First in, first out.
- **Stack (LIFO):** Last in, first out.

List operations at the left and right ends can implement either pattern.

The lecture gives a theoretical maximum list length of $2^{31}-1$ elements.

### 11.4 Sets

A Set is an unordered collection of unique strings. Adding an existing member does not create a duplicate.

Example:

```
SADD visitors "user-1" "user-2" "user-1"
SMEMBERS visitors
```

The Set contains two unique members.

Important operations include:

- **Union:** Members appearing in either set.
- **Intersection:** Members appearing in both sets.
- **Difference:** Members in one set but not another.

Typical uses include unique visitors, tags, and group membership. The lecture gives a theoretical maximum of $2^{32}-1$ members.

### 11.5 Sorted Sets

A Sorted Set stores unique members, each associated with a numerical score. Members are ordered by score.

Example:

```
ZADD leaderboard 100 "Alice"
ZADD leaderboard 250 "Bob"
ZADD leaderboard 180 "Charlie"
ZREVRANGE leaderboard 0 2 WITHSCORES
```

The results are ordered by descending score: Bob, Charlie, Alice.

Typical uses include leaderboards, ranking products, priority ordering, and score-based retrieval.

### List versus Set versus Sorted Set

| Feature | List | Set | Sorted Set |
| --- | --- | --- | --- |
| Order | Preserved | Not guaranteed | By score |
| Duplicates | Allowed | Not allowed | Members are unique |
| Main purpose | Ordered sequences | Unique membership | Ranking and scored ordering |
| Example | Job queue | Unique visitors | Player leaderboard |

**Exam trap:** All three can contain strings, but they have different behavior. Use a List when order and duplicates matter, a Set for uniqueness without ordering, and a Sorted Set for unique members ranked by score.

### 11.6 Other data types

- **Bitmap:** Supports bit-level operations over string values, useful for compact activity flags.
- **Bitfield:** Reads and modifies integer fields at specified bit offsets, useful for packed counters.
- **Geospatial index:** Stores location data and supports geographic queries, such as finding nearby places.
- **HyperLogLog:** Estimates the number of distinct elements using probabilistic counting, saving memory when an approximate result is sufficient.
- **Stream:** Stores append-oriented entries with identifiers, making it useful for event logs and messaging pipelines.

**Selection rule:** Choose a structure based on its defining property, not merely because it can store strings.

## 12\. TTL and key expiration

**TTL (Time to Live)** specifies how long a Redis key remains valid before expiration.

Example:

```
SET session:1001 "active" EX 60
TTL session:1001
```

`EX 60` sets an expiration of 60 seconds.

### Important TTL behavior

- A positive TTL indicates that a key has a remaining lifetime.
- A TTL of zero means the key has reached expiration and expires immediately.
- `TTL` returns `-1` when a key exists without an expiration.
- `TTL` returns `-2` when the key does not exist.
- A key without a TTL can remain indefinitely until deleted or removed by an applicable eviction policy.

When a key expires, it is no longer available as a valid entry. Redis does not automatically retrieve its replacement from the primary database; the application must implement any required refetching and cache population.

### Why TTL matters

- Automatically expires temporary sessions and tokens.
- Limits how long cached values can remain valid.
- Helps control cache freshness.
- Can reduce memory usage when expiring keys are removed.

**Exam distinction:** TTL limits an entry's lifetime; it does not guarantee that the value is fresh throughout that lifetime.

## 13\. Cache synchronization strategies

Cache synchronization determines how cached copies remain aligned with the authoritative database.

### 13.1 Lazy loading (cache-aside)

Lazy loading populates the cache only when data is requested and absent.

Workflow:

1. The application receives a read request.
2. It checks Redis.
3. On a hit, it returns the cached value.
4. On a miss, it queries the primary database.
5. It returns the result and stores it in Redis.

#### Advantages

- Caches only requested data.
- Avoids filling memory with records nobody accesses.
- Reduces unnecessary cache writes.
- Works well for read-heavy workloads with a limited set of frequently accessed records.

#### Disadvantages

- The first request for an uncached key incurs database latency.
- Cache misses require database access and cache population.
- Database changes do not automatically update existing cached values.
- Stale values remain possible without expiration, refresh, or invalidation.

**Best suited for:** Large datasets where only a subset is frequently requested, such as product catalogues and read-heavy user profiles.

### 13.2 Write-through caching

In a write-through strategy, the primary database is updated and the corresponding cache entry is updated as part of the same application write flow.

A simplified workflow is:

1. The application receives a write.
2. The primary database is updated.
3. The cache is updated with the corresponding value.

The exact ordering and error handling depend on the implementation.

#### Advantages

- Can keep cached values fresher after writes.
- Reduces the chance of a subsequent read receiving an old cached value when the cache update succeeds.
- May avoid some cache misses for recently written data.

#### Disadvantages

- Every relevant write adds cache-update work.
- Records that are never read may still occupy cache memory.
- Cache writes increase resource usage.
- A database update can succeed while a cache update fails, causing inconsistency.

**Best suited for:** Frequently read and updated records where freshness is important, provided cache-update failures and synchronization are handled.

### 13.3 Lazy loading versus write-through

| Feature | Lazy loading | Write-through |
| --- | --- | --- |
| When data enters the cache | On a cache miss | During or after a relevant write |
| First read | May require database lookup | Can hit cache if already populated |
| Memory efficiency | Generally better when many records are never accessed | May cache records that are never read |
| Write overhead | No required cache population for every write | Requires corresponding cache-update work |
| Stale-data risk | Updates can leave cached copies stale | Successful cache updates can reduce stale reads |
| Main strength | On-demand memory efficiency | Proactive cache updates |

**Memory trick:**

- Lazy loading: Load when needed.
- Write-through: Update the cache when writing.

### Practical scenarios

**Scenario A:** A catalogue contains millions of products, but customers view only a small fraction.

Choose lazy loading because only requested products enter the cache.

**Scenario B:** An account service frequently reads recently updated records and prioritizes cache freshness.

Write-through may be suitable because relevant writes also update the cache.

## 14\. TTL, invalidation, and synchronization together

These mechanisms solve related but different problems.

- **TTL:** Expires a key after its configured lifetime.
- **Lazy loading:** Determines when a value is fetched into the cache.
- **Write-through:** Updates the cache alongside relevant database writes.
- **Cache invalidation:** Removes or refreshes a cached value when it is no longer valid.

They can be combined. For example, a product catalogue can use lazy loading to cache requested products, a TTL of 300 seconds to limit each entry's lifetime, and explicit invalidation when a product changes.

Expiration alone does not make a cache immediately consistent. Until a key expires, is refreshed, or is invalidated, it may contain an older value.

## 15\. Redis versus Memcached

Redis and Memcached both support in-memory caching, but they have different feature sets.

| Feature | Redis | Memcached |
| --- | --- | --- |
| Primary role | In-memory data store and cache | Distributed in-memory cache |
| Data structures | Strings, Hashes, Lists, Sets, Sorted Sets, and more | Primarily simple key-value items |
| Persistence | Supports RDB and AOF | Traditionally cache-oriented without Redis-style persistence |
| Typical applications | Caching, counters, sessions, rankings, queues, and more | Straightforward object caching |
| Best fit | Advanced data structures or persistence requirements | Simple caching needs |

Choose Redis when advanced data structures, persistence, expiration controls, counters, or rankings are required. Choose Memcached when straightforward caching meets the requirements.

**Important:** More features do not automatically make Redis faster or better. The appropriate choice depends on workload, required functionality, configuration, and measured performance.

## 16\. Essential exam comparisons

| Comparison | Key distinction |
| --- | --- |
| RAM versus disk | RAM typically has lower access latency; disk provides economical persistent capacity |
| Cache versus primary database | The cache accelerates access; the primary database commonly remains authoritative |
| ACID versus BASE | Different approaches to transactions, consistency, and availability |
| Vertical versus horizontal scaling | Add resources to one machine versus add machines or nodes |
| Replication versus sharding | Maintain copies versus distribute different portions of data |
| Replication versus backup | Operational redundancy versus independent recovery |
| RDB versus AOF | Dataset snapshots versus operation logs |
| Standalone versus primary–replica | Single instance versus replicated copies |
| Primary–replica versus Sentinel | Replication versus monitoring and automatic failover |
| Sentinel versus Cluster | High availability management versus data sharding and distributed capacity |
| List versus Set | Ordered sequence with duplicates versus unique unordered membership |
| Set versus Sorted Set | Unique unordered membership versus unique members ordered by score |
| TTL versus invalidation | Time-based expiration versus removing or refreshing data because it is no longer valid |
| Lazy loading versus write-through | On-demand population versus cache updates alongside writes |
| Redis versus Memcached | Richer data structures and persistence versus straightforward caching |

## 17\. Final revision checklist

Before the exam, ensure you can:

- [ ]Explain the four major NoSQL data models and ACID, BASE, and CAP.
- [ ]Explain why in-memory databases are fast and what their limitations are.
- [ ]Describe cache hits, misses, and suitable caching candidates.
- [ ]Explain the Redis key-value model and major use cases.
- [ ]Compare RDB and AOF, including durability, file size, and recovery time.
- [ ]Interpret the RESP representation of `SET hello world`.
- [ ]Compare Standalone, primary–replica, Sentinel, and Cluster.
- [ ]Explain replication, failover, sharding, and horizontal scaling.
- [ ]Memorize the 16,384 hash slots and the CRC16 slot formula.
- [ ]Identify Redis data types and choose the correct type for a scenario.
- [ ]Explain TTL, expiration, invalidation, and stale cache values.
- [ ]Compare lazy loading and write-through caching.
- [ ]Explain why replication does not replace backups.
- [ ]Justify a Redis or Memcached choice based on application requirements.

## 18\. Core takeaways

- Redis provides low-latency data access through in-memory storage and can preserve data through RDB snapshots and AOF logs.
- RDB prioritizes compact snapshots and fast restoration; AOF supports finer-grained recovery, depending on synchronization settings.
- Primary–replica replication maintains copies, Sentinel coordinates failover, and Cluster partitions data across hash slots.
- Redis data types should be selected according to required operations: Hashes for attributes, Lists for ordered sequences, Sets for uniqueness, and Sorted Sets for rankings.
- TTL limits key lifetime, while invalidation responds to data becoming invalid.
- Lazy loading improves memory efficiency; write-through can keep frequently accessed records fresher.
- Every design involves trade-offs between latency, memory consumption, durability, availability, scalability, and data freshness.

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

## Final revision priorities

Focus on being able to explain and apply these five comparisons:

1. **RDB versus AOF:** Snapshots versus operation logs.
2. **Standalone versus primary–replica versus Sentinel versus Cluster:** Simplicity, replication, automatic failover, and sharding.
3. **Hash versus List versus Set versus Sorted Set:** Object attributes, ordered sequences, unique membership, and score-based rankings.
4. **Lazy loading versus write-through:** On-demand memory efficiency versus proactive cache updates.
5. **TTL versus cache invalidation:** Time-based expiration versus responding to data changes.

For scenario-based questions, always explain both **why the selected feature fits the requirement** and **what trade-off or limitation remains**.

Both documents are now consolidated in Markdown format with the requested filenames. The first is the unified revision guide; the second is the deduplicated review question bank with answers and explanations.
