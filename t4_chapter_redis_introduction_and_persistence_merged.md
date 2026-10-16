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

# T4 — Redis Practical Exercises and Exam Practice

## 1\. Exercise 1: API Rate Limiter — 50 Requests per Minute

### Scenario

An API allows each user a maximum of 50 requests per minute. Redis tracks the number of requests using a counter and automatically expires the counter after 60 seconds.

### Key concepts

- `INCR`: Atomically increments an integer counter.
- `GET`: Retrieves the current counter value.
- `EXPIRE`: Sets a key's expiration time in seconds.
- `TTL`: Returns the remaining time to live.
- `SET` and `NX`: Can be used to initialize a counter only if the key does not already exist.

### Solution A: Basic implementation

Use a unique Redis key for each user and time window.

```
INCR rate_limit:user123
```

Increment the number of requests for user `user123`.

Initialize the expiration on the first request:

```
EXPIRE rate_limit:user123 60
```

Check the current count:

```
GET rate_limit:user123
```

Check the remaining TTL:

```
TTL rate_limit:user123
```

Check whether the limit has been exceeded:

```
GET rate_limit:user123
```

If the returned count is greater than 50, reject the request. Otherwise, allow it.

**Important:** The basic sequence above is illustrative. Setting `EXPIRE` after every request would reset the 60-second timer and implement a sliding expiration rather than a fixed 60-second window. Set the expiration only when the counter is first created.

### Solution B: Initialize the counter only once

Redis `MULTI`/`EXEC` can execute a group of commands atomically, but conditional initialization is easier to demonstrate using a small Lua script.

```
local count = redis.call('INCR', KEYS[1])

if count == 1 then
    redis.call('EXPIRE', KEYS[1], 60)
end

return count
```

Execute this script using `EVAL`:

```
EVAL "local c=redis.call('INCR',KEYS[1]); if c==1 then redis.call('EXPIRE',KEYS[1],60) end; return c" 1 rate_limit:user123
```

The script increments the counter and sets the expiration only when the counter is created. Because the script executes atomically, another request cannot interleave between these operations.

### Simulate 51 requests

For manual testing, run the following command 51 times:

```
EVAL "local c=redis.call('INCR',KEYS[1]); if c==1 then redis.call('EXPIRE',KEYS[1],60) end; return c" 1 rate_limit:user123
```

Expected results:

- Request 1 returns `1`.
- Request 50 returns `50`.
- Request 51 returns `51`.

The script returns the counter value; it does not itself reject requests. The application must interpret the result:

```
if count > 50:
    # Reject the request: HTTP 429 Too Many Requests
    pass
else:
    # Allow the request
    pass
```

For a production rate limiter, the increment, expiration, and limit check should all occur atomically. A Lua script can perform the complete decision in Redis, preventing concurrent requests from bypassing the limit.

### Exam interpretation

This implementation is a **fixed-window rate limiter**: the counter expires 60 seconds after the first request in the window. It does not guarantee that no more than 50 requests occur in every possible rolling 60-second interval. A strict rolling-window limit requires a different design.

---

## 2\. Exercise 2: Real-Time Gaming Leaderboard

### Scenario

Track player scores and display rankings. Redis sorted sets automatically maintain members ordered by score.

### Key concepts

- `ZADD`: Adds a member with a score.
- `ZINCRBY`: Increments a member's score.
- `ZREVRANGE`: Retrieves members from highest to lowest score.
- `ZREVRANK`: Returns a player's zero-based rank, highest score first.
- `ZSCORE`: Retrieves a player's score.

### Step 1: Add players

```
ZADD leaderboard 1200 Alice 950 Bob 1500 Charlie 1100 Diana
```

Expected scores:

| Player | Score |
| --- | --- |
| Charlie | 1500 |
| Alice | 1200 |
| Diana | 1100 |
| Bob | 950 |

### Step 2: Increase a player's score

```
ZINCRBY leaderboard 200 Bob
```

Bob's score increases from 950 to 1150.

### Step 3: View the top three players

```
ZREVRANGE leaderboard 0 2 WITHSCORES
```

Expected result:

```
1) "Charlie"
2) "1500"
3) "Alice"
4) "1200"
5) "Bob"
6) "1150"
```

Redis uses zero-based indexes, so indexes `0` through `2` retrieve three members.

### Step 4: Get a player's rank

```
ZREVRANK leaderboard Alice
```

Expected result:

```
1
```

Alice is ranked second because the highest-ranked player has rank `0`.

### Step 5: Get a player's score

```
ZSCORE leaderboard Bob
```

Expected result:

```
"1150"
```

### Additional useful commands

Count the players:

```
ZCARD leaderboard
```

Retrieve players in ascending score order:

```
ZRANGE leaderboard 0 -1 WITHSCORES
```

Remove a player:

```
ZREM leaderboard Bob
```

### Exam interpretation

A sorted set stores unique members associated with numerical scores. Redis maintains the score ordering, making sorted sets suitable for leaderboards, ranking systems, and priority-based applications.

**Important distinction:** `ZREVRANK` returns a zero-based rank, whereas a human-readable leaderboard usually numbers the top player as position 1. Add 1 to the returned rank when displaying it to users.

---

## 3\. Exercise 3: Distributed Lock for Inventory Updates

### Scenario

Two application servers want to update the stock of the same product. Without coordination, both servers could read the same stock value and make conflicting updates.

A distributed lock ensures that only one cooperating server holds the lock at a time.

### Key concepts

- `SET`: Creates a Redis key.
- `NX`: Creates the key only if it does not exist.
- `EX`: Sets an expiration time in seconds.
- `GET`: Checks the current lock value.
- `DEL`: Deletes a key.
- Unique ownership tokens: Identify the server that acquired the lock.

### Step 1: Server A acquires the lock

```
SET lock:product100 serverA-token NX EX 10
```

If successful, Redis returns:

```
OK
```

The lock belongs to Server A and expires after 10 seconds.

The token should be unique for each lock acquisition, not merely a reusable server identifier.

### Step 2: Server B attempts to acquire the same lock

```
SET lock:product100 serverB-token NX EX 10
```

Expected result while Server A holds the lock:

```
(nil)
```

The key already exists, so Server B cannot acquire the lock.

### Step 3: Update the inventory

Assume the product initially has 10 units in stock and Server A wants to sell 3 units.

The application should verify that it owns the lock and that sufficient stock exists before updating inventory.

A simple illustrative update is:

```
GET inventory:product100
```

Expected result:

```
"10"
```

Then:

```
DECRBY inventory:product100 3
```

Expected result:

```
7
```

**Production requirement:** Checking lock ownership, checking stock, and decrementing stock must be performed safely together. The separate commands above are for illustrating the exercise, not a fully atomic inventory transaction.

### Step 4: Check remaining stock

```
GET inventory:product100
```

Expected result:

```
"7"
```

### Step 5: Release the lock safely

Do not blindly execute `DEL lock:product100`: the lock might have expired and been acquired by another server in the meantime.

Instead, release it only if the stored token matches the token belonging to the current owner.

```
if redis.call('GET', KEYS[1]) == ARGV[1] then
    return redis.call('DEL', KEYS[1])
else
    return 0
end
```

Execute the release script:

```
EVAL "if redis.call('GET',KEYS[1]) == ARGV[1] then return redis.call('DEL',KEYS[1]) else return 0 end" 1 lock:product100 serverA-token
```

A return value of `1` means the lock was deleted. A return value of `0` means the key did not exist or the token did not match.

### Step 6: Verify that the lock is removed

```
GET lock:product100
```

Expected result:

```
(nil)
```

Server B can now attempt to acquire the lock again.

### Preventing negative inventory

A robust inventory update must reject an order if the requested quantity exceeds available stock. The check and decrement must be atomic.

For example, if the stock is 7 and a customer requests 8 units, the operation must fail and stock must remain 7.

A Lua script can implement this safely:

```
local stock = tonumber(redis.call('GET', KEYS[1]) or '0')
local quantity = tonumber(ARGV[1])

if quantity <= 0 then
    return -2
end

if stock < quantity then
    return -1
end

return redis.call('DECRBY', KEYS[1], quantity)
```

Execute:

```
EVAL "local s=tonumber(redis.call('GET',KEYS[1]) or '0'); local q=tonumber(ARGV[1]); if q<=0 then return -2 end; if s<q then return -1 end; return redis.call('DECRBY',KEYS[1],q)" 1 inventory:product100 8
```

With stock equal to 7, the script returns `-1` and leaves the stock unchanged.

This script protects the stock invariant, but it does not implement the entire order workflow. Creating the order and updating stock must also be coordinated if they must succeed or fail together.

### Exam interpretation

`SET key token NX EX seconds` combines lock acquisition, conditional creation, and automatic expiration. Unique tokens and atomic release prevent one server from accidentally deleting another server's lock.

A Redis lock is not a substitute for atomic business logic. For inventory management, atomic stock validation and decrement are essential. In high-stakes distributed systems, lock expiration, process pauses, failover, and fencing-token requirements may also need consideration.

---

## 4\. Additional Exam Practice Questions

### Question 1 — Rate limiting

A user sends 51 requests during one 60-second rate-limit window. The maximum permitted number is 50.

What should the application do when the counter reaches 51?

**Answer:** Reject the request, typically with HTTP status `429 Too Many Requests`.

**Explanation:** The first 50 requests are permitted. The 51st request exceeds the configured limit. The Redis counter alone does not reject requests; application logic or an atomic Redis script must enforce the decision.

### Question 2 — Redis expiration

Why should the counter's expiration be set only when the counter is first created in a fixed-window rate limiter?

**Answer:** To ensure the window expires 60 seconds after its first request rather than 60 seconds after the most recent request.

**Explanation:** Resetting the TTL after every request changes the behavior and can prevent the counter from expiring while requests continue.

### Question 3 — Sorted sets

What is the difference between `ZREVRANGE` and `ZREVRANK`?

**Answer:** `ZREVRANGE` retrieves members in descending score order; `ZREVRANK` returns the zero-based rank of a particular member in descending score order.

**Explanation:** One returns members, optionally with scores; the other returns a member's position.

### Question 4 — Score updates

A player has a score of 400. The application executes:

```
ZINCRBY leaderboard 75 Alice
```

If Alice previously had a score of 400, what is her new score?

**Answer:** `475`.

**Explanation:** `ZINCRBY` atomically adds the supplied increment to the member's existing score.

### Question 5 — Distributed locks

What does `NX` mean in the following command?

```
SET lock:product100 serverA-token NX EX 10
```

**Answer:** Set the key only if it does not already exist.

**Explanation:** `NX` prevents another client from overwriting an existing lock. `EX 10` sets a 10-second expiration.

### Question 6 — Lock release

Why is this command potentially unsafe?

```
DEL lock:product100
```

**Answer:** The original lock may have expired and another server may have acquired it. The first server could then delete the second server's lock.

**Explanation:** Release should atomically verify that the stored token matches the owner's token before deleting the key.

### Question 7 — Race conditions

Two servers read a stock value of 5. Each attempts to sell 4 units. What can go wrong if both independently check stock and then decrement it?

**Answer:** Both may approve their orders based on the same initial value, potentially resulting in negative inventory or overselling.

**Explanation:** Reading, validating, and decrementing stock must be coordinated atomically. A Lua script or another suitable atomic transaction mechanism can prevent this race condition.

### Question 8 — Choosing a Redis data structure

Which Redis data structure is most appropriate for a leaderboard that must return the top three players efficiently?

**Answer:** A sorted set.

**Explanation:** Sorted sets associate unique members with scores and maintain them in score order. Commands such as `ZREVRANGE` can retrieve the highest-scoring members.

### Question 9 — Counter atomicity

Why is `INCR` preferable to retrieving a counter with `GET`, incrementing it in application code, and writing it back with `SET`?

**Answer:** `INCR` performs the increment atomically in Redis.

**Explanation:** Separate read-modify-write operations can lose updates when concurrent requests modify the same counter.

### Question 10 — Expiration and locks

Why should a distributed lock have a finite expiration?

**Answer:** To reduce the risk that a crashed or disconnected client leaves the lock held indefinitely.

**Explanation:** Expiration improves recovery, but it introduces another consideration: a process may continue working after its lock expires. Token validation and, where required, fencing tokens help protect against stale lock holders.

### Question 11 — Fixed versus sliding windows

Does a counter with a 60-second TTL necessarily guarantee at most 50 requests in every rolling 60-second period?

**Answer:** No.

**Explanation:** A fixed-window counter measures requests relative to the start of its window. Requests near the boundary can allow more than 50 requests within a rolling 60-second period. A sliding-window or token-bucket design may be more appropriate for stricter policies.

### Question 12 — Stock validation

The stock is 12 units. A customer requests 5 units. What stock should remain after a successful atomic update?

**Answer:** 7 units.

**Explanation:** The requested quantity is less than the available stock, so the decrement can succeed. If the requested quantity exceeded 12, the update should fail without changing stock.

---

## 5\. Quick Revision Sheet

| Command or concept | Purpose |
| --- | --- |
| `INCR` | Atomically increment a counter |
| `GET` | Retrieve a key's value |
| `EXPIRE` | Set a key's expiration |
| `TTL` | Check remaining lifetime |
| `ZADD` | Add members and scores |
| `ZINCRBY` | Increase a member's score |
| `ZREVRANGE` | Retrieve members from highest to lowest score |
| `ZREVRANK` | Retrieve a member's descending rank |
| `ZSCORE` | Retrieve a member's score |
| `SET ... NX EX` | Acquire a lock only if absent, with expiration |
| Unique lock token | Identify the lock owner |
| Lua script | Execute related Redis operations atomically |
| Race condition | Incorrect behavior caused by conflicting concurrent operations |
| Fixed-window limiter | Counts requests in a window with a defined start |
| Sorted set | Suitable for rankings and leaderboards |

## 6\. Core Exam Takeaways

- Use Redis counters and TTLs to implement basic rate limiting.
- Set the TTL correctly: fixed-window expiration should not be reset on every request.
- Use sorted sets for leaderboards, ranking, and score management.
- Remember that sorted-set ranks are zero-based.
- Use `SET NX EX` for basic distributed lock acquisition.
- Use unique tokens and atomic token verification when releasing locks.
- Use atomic validation and updates to prevent overselling.
- Understand the difference between atomic individual commands and atomic multi-step workflows.
- Know the limitations of fixed-window rate limiting and expiring distributed locks.

**Suggested GitHub filename:** `t7-redis-practical-exercises-and-exam-practice.md`

## Final revision priorities

Focus on being able to explain and apply these five comparisons:

1. **RDB versus AOF:** Snapshots versus operation logs.
2. **Standalone versus primary–replica versus Sentinel versus Cluster:** Simplicity, replication, automatic failover, and sharding.
3. **Hash versus List versus Set versus Sorted Set:** Object attributes, ordered sequences, unique membership, and score-based rankings.
4. **Lazy loading versus write-through:** On-demand memory efficiency versus proactive cache updates.
5. **TTL versus cache invalidation:** Time-based expiration versus responding to data changes.

For scenario-based questions, always explain both **why the selected feature fits the requirement** and **what trade-off or limitation remains**.

Both documents are now consolidated in Markdown format with the requested filenames. The first is the unified revision guide; the second is the deduplicated review question bank with answers and explanations.
