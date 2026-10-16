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


## Final revision priorities

Focus on being able to explain and apply these five comparisons:

1. **RDB versus AOF:** Snapshots versus operation logs.
2. **Standalone versus primary–replica versus Sentinel versus Cluster:** Simplicity, replication, automatic failover, and sharding.
3. **Hash versus List versus Set versus Sorted Set:** Object attributes, ordered sequences, unique membership, and score-based rankings.
4. **Lazy loading versus write-through:** On-demand memory efficiency versus proactive cache updates.
5. **TTL versus cache invalidation:** Time-based expiration versus responding to data changes.

For scenario-based questions, always explain both **why the selected feature fits the requirement** and **what trade-off or limitation remains**.

Both documents are now consolidated in Markdown format with the requested filenames. The first is the unified revision guide; the second is the deduplicated review question bank with answers and explanations.
