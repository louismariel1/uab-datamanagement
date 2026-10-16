## Recommended workflow

1. Complete an exam without viewing its solutions.
2. Record answers and confidence levels.
3. Compare your answers with the model solutions.
4. Award marks using the supplied marking criteria.
5. Record recurring misconceptions and revisit the relevant revision notes.

## Scope

The supplied revision material primarily covers Redis, NoSQL databases, caching, persistence, data structures, replication, Sentinel, Cluster, TTL, rate limiting, leaderboards, and distributed locks.

Because detailed T0–T3 and T5 lecture notes were not included in the source material, broader database and distributed-systems questions are based on foundational concepts rather than claims about the exact lecture syllabus.

## Important learning principle

A technically valid command is not necessarily a complete application solution. High-quality answers explain correctness, atomicity, durability, consistency, scalability, and failure behavior where relevant.

# File 2 — `practice-exam-1.md`

Writing

# Practice Exam 1 — Database and Redis Fundamentals

**Duration:** 120 minutes\
**Total:** 100 marks

## Instructions

- Answer every question.
- Select one answer for each multiple-choice question.
- Explain your reasoning for short-answer and design questions.
- Assume that Redis commands execute successfully unless a failure or error is explicitly described.
- Do not consult the solutions until you have finished.

## Section A — Multiple-choice questions (20 marks)

**10 questions × 2 marks**

### Question 1 — Database models (2 marks)

A social application frequently queries which users follow a particular user and how users are connected. Which data model most directly represents these relationships?

A. Key-value\
B. Graph\
C. Column-based\
D. Plain relational lookup without relationship modeling

### Question 2 — ACID and BASE (2 marks)

Which statement is most accurate?

A. BASE guarantees strong consistency after every write.\
B. ACID and BASE are interchangeable terms.\
C. ACID describes transaction properties, while BASE describes an approach that tolerates temporary inconsistency.\
D. Every NoSQL database must use BASE.

### Question 3 — Redis performance (2 marks)

Why can Redis reduce the latency of a frequently requested database query?

A. It eliminates all network latency.\
B. It stores frequently needed results in memory and avoids repeated database work on cache hits.\
C. It guarantees that every command is constant-time.\
D. It automatically makes every underlying database query faster.

### Question 4 — Cache-aside (2 marks)

A requested key is absent from Redis. What is the normal next step in a cache-aside design?

A. Return the missing key as a valid result.\
B. Restart Redis.\
C. Query the authoritative data source and optionally populate the cache.\
D. Disable expiration for every key.

### Question 5 — Redis data types (2 marks)

Which data type best suits a leaderboard with unique players and numerical scores?

A. List\
B. Set\
C. Sorted Set\
D. Hash, regardless of the required operations

### Question 6 — Expiration (2 marks)

What does `TTL session:1` returning `-1` normally mean?

A. The key does not exist.\
B. The key exists without an expiration.\
C. The key expired exactly one second ago.\
D. Redis is unable to read the key.

### Question 7 — Persistence (2 marks)

Which statement about RDB is correct?

A. It logs every individual write and always prevents data loss.\
B. It stores periodic point-in-time snapshots.\
C. It automatically creates an independent off-site backup.\
D. It replaces replication.

### Question 8 — Replication and sharding (2 marks)

Which statement is correct?

A. Replication distributes different key ranges across nodes.\
B. Sharding creates identical copies of the entire dataset on every node.\
C. Replication maintains copies; sharding distributes portions of the dataset.\
D. Replication and sharding are synonyms.

### Question 9 — RESP (2 marks)

In the following representation, what does `*3` indicate?

```
*3
$3
SET
$1
k
$1
v
```

A. The key contains three bytes.\
B. The value has three bytes.\
C. The array contains three elements.\
D. The command is executed three times.

### Question 10 — Cache freshness (2 marks)

A product price changes in the primary database, but Redis still contains the previous price and its TTL has not expired. What is the main issue?

A. Sharding\
B. Cache staleness\
C. RESP corruption\
D. Hash-slot reassignment

## Section B — Short-answer questions (20 marks)

### Question 11 — In-memory databases (5 marks)

Explain three advantages and two limitations of using Redis as an in-memory data store.

Your answer should distinguish access speed from durability.

### Question 12 — Data-structure selection (5 marks)

Choose a suitable Redis data type for each requirement and justify each choice.

1. Store a user profile with `name`, `email`, and `city`.
2. Track unique users who visited a page.
3. Maintain a sequence of recent events where duplicates are allowed.
4. Estimate the number of distinct visitors using limited memory.
5. Store append-oriented events that multiple consumers may process.

### Question 13 — RDB versus AOF (5 marks)

Compare RDB and AOF in terms of:

- Storage method
- Recovery process
- Potential data loss
- File size or storage overhead
- A suitable use case for each

### Question 14 — Caching strategies (5 marks)

Explain lazy loading and write-through caching. Give one suitable scenario for each and identify one limitation of each strategy.

## Section C — Redis command interpretation (25 marks)

### Question 15 — Hashes and TTL (7 marks)

Consider these commands:

```
HSET user:7 name "Mina" age "22" city "Madrid"
HGET user:7 name
TTL user:7
EXPIRE user:7 120
TTL user:7
HSET user:7 age "23"
HGETALL user:7
```

Answer the following:

1. What does `HGET user:7 name` return? (1 mark)
2. What does the first `TTL user:7` return if the key exists and has no expiry? (1 mark)
3. What does `EXPIRE user:7 120` do? (1 mark)
4. What does `HSET user:7 age "23"` change? (1 mark)
5. Give the final logical field-value mapping. (2 marks)
6. Does updating the hash field necessarily remove the key's existing TTL? Explain. (1 mark)

### Question 16 — Lists, Sets, and Sorted Sets (8 marks)

Execute the following conceptually:

```
LPUSH tasks A
RPUSH tasks B
LPUSH tasks A
LRANGE tasks 0 -1

SADD visitors u1 u2 u1
SCARD visitors

ZADD scores 10 Alice 30 Bob 20 Cara
ZINCRBY scores 5 Alice
ZREVRANGE scores 0 2 WITHSCORES
ZREVRANK scores Alice
```

Answer:

1. What is the list order returned by `LRANGE`? (2 marks)
2. What is `SCARD visitors`? Explain. (2 marks)
3. What are the final sorted-set scores and descending order? (2 marks)
4. What does `ZREVRANK scores Alice` return? (2 marks)

### Question 17 — RESP and persistence (5 marks)

Interpret this RESP command:

```
*3
$3
SET
$5
hello
$5
world
```

Explain how a recorded write command can help Redis reconstruct its data after a restart. State one limitation of AOF-based recovery.

### Question 18 — Atomic counters (5 marks)

Two application servers perform the following sequence on the same key:

```
GET requests:user7
# increment locally
SET requests:user7 <new-value>
```

Both read `10` before either writes. Each writes `11`.

1. What should the final value have been if both increments were applied? (1 mark)
2. What value may actually remain? (1 mark)
3. Name the concurrency problem. (1 mark)
4. Propose a safer Redis command. (1 mark)
5. Explain why your proposed command is safer. (1 mark)

## Section D — Practical and design exercises (35 marks)

### Question 19 — Cache-aside product service (10 marks)

A product service has a persistent database containing millions of products. Only a small proportion of products are frequently viewed. Product details are cached in Redis.

Design a cache-aside read and update strategy.

Your answer must address:

1. The read flow. (3 marks)
2. How cache misses are handled. (2 marks)
3. How product updates invalidate or refresh cached data. (2 marks)
4. A suitable TTL policy and its trade-off. (2 marks)
5. One concurrency or failure concern. (1 mark)

### Question 20 — Rate limiter (12 marks)

An API permits at most 50 requests per user per fixed 60-second window.

A developer proposes:

```
INCR rate_limit:user123
EXPIRE rate_limit:user123 60
GET rate_limit:user123
```

1. Explain why resetting the expiration on every request is a problem for a fixed-window design. (3 marks)
2. Write a Lua script that increments the counter and sets its TTL only when the count first becomes one. (4 marks)
3. Explain what the application should do when the returned count is 51. (2 marks)
4. Identify one limitation of a fixed-window rate limiter. (3 marks)

### Question 21 — Persistence and recovery design (13 marks)

An online booking service uses Redis to store temporary session information and a persistent relational database to store confirmed bookings.

The team requires:

- Fast session reads
- Recovery of recent Redis session changes
- Independent recovery if the Redis machine is destroyed
- High availability for the Redis service

Recommend a design using Redis persistence and deployment features.

Explain:

1. RDB and AOF choices. (4 marks)
2. Replication and Sentinel or Cluster selection. (3 marks)
3. Independent backups. (2 marks)
4. The role of the relational database. (2 marks)
5. Two limitations or failure risks that remain. (2 marks)

## End of exam

**Total: 100 marks.**

# File 3 — `practice-exam-1-solutions.md`

Writing

# Practice Exam 1 — Comprehensive Solutions

**Total:** 100 marks

## Section A — Multiple-choice answers (20 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| 1 | B | A graph directly models entities and relationships. |
| 2 | C | ACID describes transaction properties; BASE tolerates temporary inconsistency in exchange for availability and other system goals. |
| 3 | B | An in-memory cache can avoid repeated database access on a hit. Network latency and application overhead still exist. |
| 4 | C | Cache-aside fetches absent data from the authoritative source and may populate the cache. |
| 5 | C | Sorted Sets associate unique members with scores and support score-based ranking. |
| 6 | B | `TTL` returns `-1` for an existing key without an expiration. A missing key normally returns `-2`. |
| 7 | B | RDB saves point-in-time snapshots. It does not guarantee zero data loss or create off-site backups automatically. |
| 8 | C | Replication maintains copies; sharding partitions data. |
| 9 | C | `*3` means the RESP array contains three elements. |
| 10 | B | The cached value is stale because it no longer matches the authoritative database. |

**Marking:** 2 marks per correct answer; no partial marks.

## Section B — Model answers (20 marks)

### Question 11 — In-memory databases (5 marks)

Possible advantages:

- **Low latency:** RAM access is generally much faster than disk access.
- **Reduced database load:** Cache hits avoid repeated queries against the primary database.
- **Efficient data structures:** Redis supports counters, hashes, lists, sets, sorted sets, and other structures suited to common workloads.

Possible limitations:

- **Cost and capacity:** RAM is generally more expensive per unit of capacity than disk storage.
- **Durability complexity:** In-memory storage alone does not guarantee recovery after a failure. Persistence, replication, and backups must be designed separately.

**Marking:**

- Three valid advantages: 1 mark each.
- Two valid limitations: 1 mark each.

**Common trap:** Saying that Redis cannot persist data because it is in-memory. Redis supports RDB and AOF persistence.

### Question 12 — Data-structure selection (5 marks)

1. **Hash:** Stores the profile's fields under `user:...`.
2. **Set:** Ensures each visitor identifier is stored only once.
3. **List:** Maintains sequence order and permits duplicates.
4. **HyperLogLog:** Estimates distinct cardinality probabilistically with low memory usage.
5. **Stream:** Supports append-oriented events, identifiers, and consumer-group processing.

**Marking:** 1 mark per correct type with a reasonable justification.

**Common trap:** Choosing a Set for recent events merely because it stores strings. Sets do not preserve the required event sequence or duplicates.

### Question 13 — RDB versus AOF (5 marks)

| Criterion | RDB | AOF |
| --- | --- | --- |
| Storage method | Point-in-time dataset snapshots | Log of write operations |
| Recovery | Load a snapshot | Replay recorded operations |
| Potential loss | Writes after the last usable snapshot may be lost | Depends on synchronization policy and failure scenario |
| Storage | Often compact | Can be larger before rewriting |
| Suitable use | Backups and relatively fast restoration | Finer-grained recovery of recent changes |

**Marking:**

- Storage method: 1 mark.
- Recovery process: 1 mark.
- Data-loss distinction: 1 mark.
- Storage or performance trade-off: 1 mark.
- Suitable use case: 1 mark.

**Common trap:** Claiming AOF is always lossless. Unsynchronized recent writes can still be lost.

### Question 14 — Caching strategies (5 marks)

**Lazy loading / cache-aside:**

1. Check Redis.
2. Return the cached value on a hit.
3. On a miss, query the database, return the result, and populate Redis.

Suitable scenario: A huge product catalogue in which only a small fraction of products are viewed.

Limitation: The first miss incurs database latency, and cached values may become stale.

**Write-through:**

1. Update the authoritative database.
2. Update the corresponding cache entry in the write flow.

Suitable scenario: Frequently read account details that should be refreshed when updates occur.

Limitation: Every relevant write adds cache-update work, and a cache update may fail after the database update succeeds.

**Marking:**

- Lazy-loading workflow: 2 marks.
- Write-through workflow: 1 mark.
- Suitable scenarios: 1 mark.
- Limitations: 1 mark.

## Section C — Command interpretation (25 marks)

### Question 15 — Hashes and TTL (7 marks)

1. `HGET user:7 name` returns `"Mina"`. **(1 mark)**
2. The first `TTL user:7` returns `-1`, assuming the key exists without an expiration. **(1 mark)**
3. `EXPIRE user:7 120` sets a lifetime of 120 seconds. **(1 mark)**
4. `HSET user:7 age "23"` updates the existing `age` field from `"22"` to `"23"`. **(1 mark)**
5. Final mapping:```
   name -> "Mina"
   age  -> "23"
   city -> "Madrid"
   ```
   **(2 marks)**
6. Updating a hash field with `HSET` does not inherently remove the key's existing TTL. **(1 mark)**

**Common trap:** Assuming a field update automatically resets or removes the expiration. Key expiration applies to the key, not independently to every hash field in this example.

### Question 16 — Lists, Sets, and Sorted Sets (8 marks)

**1\. List order (2 marks)**

Redis lists insert at the requested end.

- First `LPUSH tasks A`: `[A]`
- Then `RPUSH tasks B`: `[A, B]`
- Then `LPUSH tasks A`: `[A, A, B]`

`LRANGE tasks 0 -1` returns:

```
A
A
B
```

**2\. Set cardinality (2 marks)**

`SCARD visitors` returns `2`.

The Set contains `u1` and `u2`. Re-adding `u1` does not create a duplicate.

**3\. Sorted-set scores and order (2 marks)**

Initial scores:

- Alice: 10
- Bob: 30
- Cara: 20

After `ZINCRBY scores 5 Alice`:

- Bob: 30
- Cara: 20
- Alice: 15

Descending order is Bob, Cara, Alice.

**4\. Alice's rank (2 marks)**

`ZREVRANK scores Alice` returns `2`.

Ranks are zero-based: Bob is 0, Cara is 1, Alice is 2.

### Question 17 — RESP and persistence (5 marks)

The command is equivalent to:

```
SET hello world
```

- `*3`: An array of three elements.
- `$3`: The command name has three bytes: `SET`.
- `$5`: The key has five bytes: `hello`.
- `$5`: The value has five bytes: `world`.

RESP length markers count bytes, and the protocol uses CRLF line endings.

An AOF can record the write command so Redis can replay it during recovery and reconstruct the dataset.

One limitation: recent commands may be lost if they have not been synchronized to durable storage according to the configured policy.

**Marking:**

- Command identification: 1 mark.
- Array interpretation: 1 mark.
- Bulk-string lengths: 1 mark.
- Replay explanation: 1 mark.
- Limitation: 1 mark.

### Question 18 — Atomic counters (5 marks)

1. Correct final value: `12`. **(1 mark)**
2. Actual value may be `11`. **(1 mark)**
3. This is a lost-update race condition. **(1 mark)**
4. Use:```
   INCR requests:user7
   ```
   **(1 mark)**
5. `INCR` performs the increment atomically at Redis, so concurrent increments are not lost in the same way as separate `GET` and `SET` operations. **(1 mark)**

**Common trap:** Assuming that two successful `SET` commands mean both increments were applied.

## Section D — Practical and design solutions (35 marks)

### Question 19 — Cache-aside product service (10 marks)

**Model answer**

Read flow:

1. Construct a stable key such as `product:123`.
2. Read the key from Redis.
3. On a hit, return the cached product.
4. On a miss, query the persistent database, return the result, and populate Redis.

Example pseudocode:

```
product = redis.get("product:123")

if product is not None:
    return deserialize(product)

product = database.get_product(123)

if product is None:
    return not_found()

redis.set("product:123", serialize(product), ex=300)
return product
```

This is illustrative application pseudocode, not a complete production implementation.

**Miss handling:** The database remains authoritative. The application retrieves the product and can cache the result for later requests.

**Update handling:** When a product changes, update the primary database and then invalidate the corresponding cache key, or update the cached value. Invalidation is often easier to reason about if there are multiple application write paths.

Example:

```
DEL product:123
```

A later read repopulates the cache.

**TTL policy:** A five-minute TTL is one possible starting point, not a universal optimum. Shorter TTLs reduce the stale-data window but increase misses and database load. Longer TTLs improve hit rates but can retain outdated values longer.

**Concurrency concern:** A read can fetch an old database value, pause, and populate Redis after another request has already invalidated the key. Versioning, careful write ordering, retries, or other synchronization may be required where this race matters.

**Marking:**

- Read flow: 3 marks.
- Miss handling: 2 marks.
- Update and invalidation: 2 marks.
- TTL trade-off: 2 marks.
- Concurrency or failure concern: 1 mark.

**Common traps:**

- Treating Redis as the authoritative database without justification.
- Assuming TTL guarantees immediate freshness.
- Assuming updating the database and Redis is automatically atomic.

### Question 20 — Rate limiter (12 marks)

**1\. Expiration problem (3 marks)**

If `EXPIRE key 60` is run after every request, the counter's lifetime is repeatedly extended. It no longer expires 60 seconds after the first request.

This is not the intended fixed-window behavior.

**2\. Lua implementation (4 marks)**

```
local count = redis.call('INCR', KEYS[1])

if count == 1 then
    redis.call('EXPIRE', KEYS[1], 60)
end

return count
```

Example invocation:

```
EVAL "local c=redis.call('INCR',KEYS[1]); if c==1 then redis.call('EXPIRE',KEYS[1],60) end; return c" 1 rate_limit:user123
```

The script atomically increments the counter and sets the expiry only on the first increment.

**3\. Request 51 (2 marks)**

The application should reject the request, typically with HTTP `429 Too Many Requests`.

The script above returns a count; it does not itself make the HTTP decision. A complete enforcement script could also return an allow/deny result.

**4\. Fixed-window limitation (3 marks)**

A fixed-window limiter can allow a burst across a boundary. For example, a user may send 50 requests near the end of one window and another 50 immediately after the next window begins.

Therefore, it does not guarantee at most 50 requests in every rolling 60-second interval.

**Marking:** As allocated above.

### Question 21 — Persistence and recovery design (13 marks)

**1\. RDB and AOF (4 marks)**

- Enable RDB snapshots for compact recovery files and backup workflows.
- Enable AOF if more granular recovery of recent session writes is required.
- Select an AOF synchronization policy based on acceptable data loss and latency.
- Test restoration and monitor persistence errors and resource usage.

RDB and AOF complement each other; neither guarantees zero loss under every failure condition.

**2\. Replication and high availability (3 marks)**

- Use primary–replica replication for redundant copies.
- Use Sentinel when the requirement is monitoring and automatic failover for a primary–replica deployment.
- Use Redis Cluster instead if the workload also requires sharding and horizontal distribution of the dataset.

A small session store that fits on one shard may not need Cluster merely because high availability is required.

**3\. Independent backups (2 marks)**

Store versioned backups outside the Redis host, preferably in a separate failure domain. Test restoration periodically.

Replication alone is insufficient because deletions and corruption can propagate to replicas.

**4\. Relational database role (2 marks)**

Confirmed bookings should remain in the persistent authoritative database. Redis session data should not become the only copy of a confirmed booking unless the entire persistence and recovery design explicitly supports that responsibility.

**5\. Remaining risks (2 marks)**

Any two well-explained risks:

- Failover may cause a short interruption.
- Asynchronous replication can lose recent writes.
- AOF synchronization policies may allow some recent changes to be lost.
- Backups may be unusable if restoration is never tested.
- Persistence and replication add operational complexity.
- Network partitions can affect availability and consistency.

**Common trap:** Claiming that Sentinel itself provides persistence or that replicas are independent backups.

## Overall grading guidance

- Award marks for equivalent technically correct answers.
- Award reasoning marks when the conclusion is correct and justified.
- Do not award full marks for a correct feature name with an incorrect explanation.
- For practical designs, reward explicit treatment of failure modes and trade-offs.

# File 4 — `practice-exam-2.md`

Writing

# Practice Exam 2 — Distributed Redis Systems and Reliability

**Duration:** 120 minutes\
**Total:** 100 marks

## Section A — Multiple-choice questions (20 marks)

**10 questions × 2 marks**

### Question 1 — Sentinel

What is the primary role of Redis Sentinel?

A. Partition keys into hash slots.\
B. Monitor Redis instances and coordinate failover.\
C. Replace RDB persistence.\
D. Convert every Redis key into a database table.

### Question 2 — Redis Cluster

How many hash slots does Redis Cluster use?

A. 1,024\
B. 4,096\
C. 16,384\
D. A different number for every cluster

### Question 3 — Replication

Why can a replica return stale data?

A. Redis replicas always use a different keyspace.\
B. Replication is generally asynchronous, so updates may not have arrived yet.\
C. RDB snapshots cannot be loaded by replicas.\
D. Replicas cannot store strings.

### Question 4 — Backups

Why is replication not a complete backup strategy?

A. Replicas cannot contain Redis data structures.\
B. Replication prevents all failures.\
C. Accidental deletion or corruption may propagate to replicas.\
D. Replication always deletes the primary dataset.

### Question 5 — Sorted-set ranking

A leaderboard has three players. Which command returns the descending rank of Alice?

A. `ZSCORE leaderboard Alice`\
B. `ZREVRANK leaderboard Alice`\
C. `ZCARD leaderboard Alice`\
D. `HGET leaderboard Alice`

### Question 6 — Atomicity

What is the most important property of a Redis Lua script for a multi-step stock check and decrement?

A. It makes Redis persistent without configuration.\
B. It executes the script atomically relative to other server activity.\
C. It guarantees zero data loss after a machine failure.\
D. It automatically creates replicas.

### Question 7 — AOF

What does `appendfsync everysec` generally mean?

A. Every key expires after one second.\
B. AOF data is synchronized approximately every second, subject to the operating environment.\
C. Redis writes an RDB snapshot every second.\
D. Every client command is guaranteed to survive every hardware failure.

### Question 8 — Cluster versus Sentinel

Which requirement most directly motivates Redis Cluster rather than Sentinel alone?

A. Monitoring one primary.\
B. Automatic failover without sharding.\
C. Distributing a dataset across multiple shards for aggregate capacity.\
D. Adding TTL to a key.

### Question 9 — Distributed locks

Why should a lock value contain a unique ownership token?

A. To increase the key's score.\
B. To allow safe owner verification when releasing the lock.\
C. To disable key expiration.\
D. To make the lock replicated synchronously.

### Question 10 — CAP

What does the CAP theorem highlight during a network partition?

A. A distributed system can always guarantee full consistency and full availability simultaneously.\
B. The system must trade off full consistency and full availability for the affected operations.\
C. Every database must stop all requests permanently.\
D. Persistence becomes impossible.

## Section B — Short-answer questions (20 marks)

### Question 11 — Deployment comparison (6 marks)

Compare Standalone, primary–replica, Sentinel, and Redis Cluster.

For each, state its primary purpose and one important limitation.

### Question 12 — Reliability concepts (5 marks)

Distinguish:

1. Replication and sharding.
2. Vertical and horizontal scaling.
3. Persistence and backup.

Give a practical example of each distinction.

### Question 13 — CAP and consistency (4 marks)

A distributed application experiences a network partition between nodes. Explain why the design may need to trade off consistency and availability. Explain why this does not mean that every Redis deployment has identical behavior.

### Question 14 — RDB/AOF recovery analysis (5 marks)

A Redis instance has an RDB snapshot from 12:00. It receives important writes at 12:01 and 12:02, then crashes at 12:03.

Its AOF is enabled with an approximately one-second synchronization policy.

Explain what might be recovered and what uncertainty remains.

## Section C — Command interpretation and debugging (25 marks)

### Question 15 — Leaderboard analysis (7 marks)

Commands:

```
ZADD leaderboard 1200 Alice 950 Bob 1500 Charlie 1100 Diana
ZINCRBY leaderboard 200 Bob
ZREVRANGE leaderboard 0 2 WITHSCORES
ZREVRANK leaderboard Alice
ZSCORE leaderboard Bob
```

State:

1. Bob's final score. (1 mark)
2. The top three players and scores. (3 marks)
3. Alice's zero-based rank. (2 marks)
4. Why a human-readable rank may differ from the command result by one. (1 mark)

### Question 16 — Redis Cluster slot calculation (5 marks)

A key is assigned using:

$$
\text{slot}=\operatorname{CRC16}(\text{key})\bmod 16384
$$

Answer:

1. How many slots exist? (1 mark)
2. What range of slot numbers is valid? (1 mark)
3. What determines which node handles the key? (1 mark)
4. Does every node store the complete dataset in a normally sharded cluster? Explain. (2 marks)

You are not required to calculate a CRC16 value.

### Question 17 — Distributed lock debugging (7 marks)

A developer uses:

```
SET lock:item-7 serverA NX EX 10
```

After doing some work, Server A executes:

```
DEL lock:item-7
```

Answer:

1. What do `NX` and `EX 10` achieve? (2 marks)
2. Describe a race condition involving lock expiration and deletion. (3 marks)
3. Explain how a unique token and conditional release help. (2 marks)

### Question 18 — Stock race condition (6 marks)

Stock is `5`. Two servers each want to sell `4` units.

Both execute this conceptual sequence:

```
Read stock
If stock >= 4:
    Decrement stock by 4
    Approve order
```

Explain the race condition and write a Lua script that atomically checks the stock and decrements it only if enough stock exists.

Your script should reject non-positive quantities.

## Section D — Applied design (35 marks)

### Question 19 — Architecture selection (10 marks)

A growing application has a Redis dataset too large for one machine. It needs:

- Horizontal data distribution
- Redundancy
- Automatic recovery from some node failures
- Reasonable operational complexity

Recommend an architecture. Explain the roles of sharding, replicas, and failover. State two limitations.

### Question 20 — Event processing (10 marks)

An application collects ordered application events and needs consumers to process them reliably, including tracking consumer progress.

1. Select an appropriate Redis data type. (2 marks)
2. Explain why a List or Set may be less suitable. (3 marks)
3. Describe how Streams and consumer groups help. (3 marks)
4. State two limitations or design concerns. (2 marks)

### Question 21 — Durability incident (15 marks)

An online application uses primary–replica Redis replication. A write succeeds on the primary, but the primary fails before the replica has received the update. The replica is promoted. The application observes that the write is missing.

1. Explain the incident. (4 marks)
2. Explain why replication alone does not guarantee zero data loss. (3 marks)
3. Recommend persistence and backup improvements. (4 marks)
4. Explain how to reduce the risk for especially important writes and state the remaining limitation. (4 marks)

## End of exam

**Total: 100 marks.**

# File 5 — `practice-exam-2-solutions.md`

Writing

# Practice Exam 2 — Comprehensive Solutions

**Total:** 100 marks

## Section A — Multiple-choice answers (20 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| 1 | B | Sentinel monitors instances and coordinates failover. |
| 2 | C | Redis Cluster has 16,384 hash slots. |
| 3 | B | Asynchronous replication allows replicas to lag behind the primary. |
| 4 | C | Logical errors and deletions can propagate to replicas. |
| 5 | B | `ZREVRANK` returns a member's zero-based descending rank. |
| 6 | B | Redis executes a Lua script atomically relative to other server activity. |
| 7 | B | The policy generally requests synchronization approximately every second; it is not a universal hardware-failure guarantee. |
| 8 | C | Cluster shards data to distribute capacity and workload. |
| 9 | B | The token enables conditional ownership verification. |
| 10 | B | During a partition, full consistency and full availability cannot both be guaranteed for every affected operation. |

## Section B — Model answers (20 marks)

### Question 11 — Deployment comparison (6 marks)

| Architecture | Primary purpose | Limitation |
| --- | --- | --- |
| Standalone | Simplicity | One instance can be a single point of failure. |
| Primary–replica | Redundant copies and possible read scaling | Replicas may lag; automatic failover is not provided by replication alone. |
| Sentinel | Monitoring and failover for primary–replica deployments | Does not shard the dataset. |
| Redis Cluster | Sharding and horizontal distribution, with optional replicas | More complex routing, rebalancing, and failure handling. |

**Marking:** 1.5 marks per architecture for purpose and limitation.

### Question 12 — Reliability concepts (5 marks)

**Replication versus sharding:** Replication creates copies; sharding distributes different portions of the dataset. For example, a replica can provide a second copy of session data, while Cluster distributes different keys across shards.

**Vertical versus horizontal scaling:** Vertical scaling adds resources to a machine; horizontal scaling adds machines or nodes. For example, increasing RAM is vertical scaling; adding shards is horizontal scaling.

**Persistence versus backup:** Persistence stores data durably for recovery from certain failures; a backup is a separately retained recovery copy. A local AOF is persistence, while an independently stored and versioned snapshot is a backup.

**Marking:** 1 mark for each correct distinction and 2 marks for useful examples.

### Question 13 — CAP and consistency (4 marks)

A network partition prevents some nodes from communicating. A design that continues responding independently on both sides may return inconsistent values. A design that refuses certain requests to preserve a stronger consistency guarantee sacrifices availability for those operations.

The CAP trade-off concerns behavior during partitions, not a claim that every system always chooses only consistency or only availability under normal operation.

Redis behavior depends on architecture, configuration, and the operation being performed. Sentinel, primary–replica replication, and Cluster have different failover and routing behavior.

**Marking:**

- Explain partition: 1 mark.
- Explain consistency: 1 mark.
- Explain availability: 1 mark.
- Qualify the claim by architecture/configuration: 1 mark.

### Question 14 — RDB/AOF recovery analysis (5 marks)

The RDB snapshot represents the dataset at 12:00. If that is the latest usable snapshot, it alone does not contain the writes made afterward.

The AOF may contain the writes at 12:01 and 12:02. With an approximately one-second synchronization policy, recent synchronized writes are more likely to survive than with RDB alone. However, writes in the unsynchronized interval may be lost depending on timing and failure mode.

No exact recovery outcome can be guaranteed from the information given.

**Marking:**

- Identify snapshot state: 1 mark.
- Explain that later writes are absent from that snapshot: 1 mark.
- Explain AOF replay: 1 mark.
- Explain synchronization trade-off: 1 mark.
- Avoid claiming guaranteed zero loss: 1 mark.

## Section C — Command interpretation (25 marks)

### Question 15 — Leaderboard analysis (7 marks)

Bob starts with `950`. `ZINCRBY leaderboard 200 Bob` changes his score to `1150`.

Final scores:

| Descending position | Player | Score |
| --- | --- | --- |
| 1 | Charlie | 1500 |
| 2 | Alice | 1200 |
| 3 | Bob | 1150 |
| 4 | Diana | 1100 |

`ZREVRANK leaderboard Alice` returns `1` because ranks start at zero.

`ZSCORE leaderboard Bob` returns `1150` (commonly displayed as a numeric string in client output).

**Marking:**

- Bob's score: 1 mark.
- Correct top three and scores: 3 marks.
- Alice's rank: 2 marks.
- Zero-based explanation: 1 mark.

### Question 16 — Redis Cluster slots (5 marks)

1. There are **16,384 slots**. (1 mark)
2. Valid slot numbers are **0–16,383**, inclusive. (1 mark)
3. The CRC16-derived slot and the cluster's slot assignment determine which node owns the key. (1 mark)
4. In a normally sharded cluster, each primary owns a subset of slots and therefore a portion of the dataset; replicas may hold copies of their associated primary's shard. (2 marks)

**Common trap:** Assuming that hashing alone determines the final physical node. Hashing selects a slot; the cluster's current slot assignment maps that slot to a node.

### Question 17 — Distributed lock debugging (7 marks)

**1\.****`NX`****and****`EX 10`****(2 marks)**

- `NX`: Create the key only if it does not exist.
- `EX 10`: Set an expiration of ten seconds.

**2\. Race condition (3 marks)**

Server A acquires the lock. Its work takes longer than ten seconds, so the lock expires. Server B acquires a new lock using its own token. Server A then executes `DEL lock:item-7` and deletes Server B's lock.

**3\. Safe release (2 marks)**

Generate a unique token for each acquisition. Execute an atomic script that deletes the lock only when the stored value matches the caller's token.

Example:

```
if redis.call('GET', KEYS[1]) == ARGV[1] then
    return redis.call('DEL', KEYS[1])
else
    return 0
end
```

This prevents a former owner from deleting a newer owner's lock.

**Further limitation:** A token does not by itself prevent an expired owner from continuing to modify an external resource. Fencing tokens or stronger coordination may be needed for critical operations.

### Question 18 — Stock race condition (6 marks)

Both servers can read stock `5` before either decrements it. Both then conclude that four units are available. Independent checks and decrements can approve orders that collectively exceed stock.

A suitable Lua script:

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

For the example, the first successful request reduces stock to `1`. The second request for four units returns `-1`, leaving stock at `1`.

The script is atomic within Redis, so another client cannot interleave its own Redis command in the middle of its execution.

**Marking:**

- Explain race: 2 marks.
- Valid script: 3 marks.
- Correct rejection and invariant: 1 mark.

**Common trap:** Claiming that acquiring a lock alone makes every subsequent business operation safe. The validation and decrement still need appropriate atomicity.

## Section D — Applied design solutions (35 marks)

### Question 19 — Architecture selection (10 marks)

**Recommendation:** Redis Cluster with appropriate replica configuration.

**Sharding:** Distributes the dataset across multiple primary nodes using hash slots. This supports aggregate capacity beyond one machine.

**Replicas:** Maintain copies of each shard and can provide candidates for failover. They may also serve permitted read traffic, although stale reads must be considered.

**Failover:** Cluster detects certain node failures and promotes an eligible replica when the configured cluster conditions permit it. This helps restore service without relying entirely on manual intervention.

**Limitations:**

- Cluster introduces operational complexity, key-routing considerations, and resharding work.
- Replication is generally asynchronous; a failure can lose recent writes.
- A cluster can still experience outages if too many required nodes fail or quorum/slot-availability conditions are not met.
- Cross-slot multi-key operations can be constrained.

**Marking:**

- Appropriate architecture: 2 marks.
- Sharding explanation: 2 marks.
- Replica explanation: 2 marks.
- Failover explanation: 2 marks.
- Two limitations: 2 marks.

**Alternative answers:** A carefully justified design using multiple independent shards with external orchestration may earn credit, but it must explain how routing, replication, and failover work.

### Question 20 — Event processing (10 marks)

**1\. Data type (2 marks)**

Use a Redis Stream.

**2\. Why not a List or Set? (3 marks)**

- A List supports ordering and duplicates, but does not by itself provide the same built-in stream entry identifiers and consumer-group workflow.
- A Set is unordered and removes duplicate members, making it unsuitable for preserving an ordered event log.
- A Stream is better aligned with event identifiers, ordered entries, and consumer progress.

**3\. Streams and consumer groups (3 marks)**

Streams append entries with identifiers. Consumer groups distribute processing among consumers and track delivered entries and pending work. Consumers can acknowledge completed entries, supporting recovery and coordinated processing.

**4\. Limitations (2 marks)**

Any two:

- Consumer acknowledgements and pending entries require operational handling.
- Retention and trimming policies can remove events consumers still need.
- Delivery and processing semantics do not automatically guarantee exactly-once effects in an external database.
- Duplicate processing and idempotency must be considered.
- Consumer lag requires monitoring.

**Common trap:** Equating an event being stored in a Stream with the business operation triggered by that event being completed exactly once.

### Question 21 — Durability incident (15 marks)

**1\. Incident explanation (4 marks)**

The primary accepted a write, but replication had not yet delivered it to the replica. The primary failed before the replica had the update. The replica was promoted with an older dataset, so the write disappeared from the active Redis deployment.

This is possible because primary–replica replication is generally asynchronous.

**2\. Why replication is insufficient (3 marks)**

Replication reduces the risk of losing data when a node fails, but does not ensure every acknowledged write has reached every replica before a failure. It also propagates many logical changes, including accidental deletions.

**3\. Improvements (4 marks)**

- Enable suitable RDB snapshots.
- Enable AOF with a synchronization policy selected for the required durability.
- Retain independent, versioned backups outside the Redis host or failure domain.
- Test restore procedures and monitor replication lag and persistence health.

**4\. Important writes and remaining limitation (4 marks)**

A client can use `WAIT` to request acknowledgements from a specified number of replicas after writing. This can reduce the probability of losing a recent write, but it does not turn Redis replication into a strongly consistent CP system or guarantee zero loss under every failover scenario.

For business-critical confirmed bookings, keep the authoritative record in a persistent database with a suitable transaction and durability design.

**Marking:**

- Incident sequence: 2 marks.
- Asynchronous replication explanation: 2 marks.
- Persistence and backup recommendations: 4 marks.
- `WAIT` or equivalent risk reduction: 2 marks.
- Remaining limitation: 2 marks.
- Appropriate authoritative data store: 3 marks.

## Overall grading guidance

Full-credit answers connect each mechanism to the failure it addresses and explain the failure it does not prevent.

# File 6 — `practice-exam-3.md`

Writing

# Practice Exam 3 — Integrated Redis Design and Practical Reasoning

**Duration:** 120 minutes\
**Total:** 100 marks

## Section A — Multiple-choice questions (20 marks)

**10 questions × 2 marks**

### Question 1 — Cache invalidation

A cached product value is invalidated after the primary database is updated. What happens on the next cache-aside read?

A. Redis permanently rejects the key.\
B. The application may fetch the new value from the database and repopulate Redis.\
C. The old value must be returned.\
D. Redis automatically rolls back the database update.

### Question 2 — TTL

A session key has a TTL of `-2`. What does this normally indicate?

A. The key exists without expiry.\
B. The key is missing.\
C. The key will expire in two seconds.\
D. The key contains two fields.

### Question 3 — HyperLogLog

Which requirement best suits HyperLogLog?

A. Exact inventory decrements\
B. Approximate count of distinct visitors with low memory use\
C. Exact event ordering\
D. Atomic distributed lock ownership

### Question 4 — List behavior

Which statement about Redis Lists is correct?

A. They prohibit duplicate values.\
B. They automatically order members by numerical score.\
C. They preserve sequence order and allow duplicates.\
D. They are only suitable for numeric values.

### Question 5 — Write-through caching

Which is a limitation of write-through caching?

A. It can never update a cached value.\
B. It can add cache-update work for each relevant write.\
C. It requires every key to have no TTL.\
D. It prevents all possible stale reads.

### Question 6 — AOF rewriting

What is the general purpose of AOF rewriting?

A. Convert every Redis key into a hash slot.\
B. Reduce the size of the operation log by writing a more compact representation of the current dataset.\
C. Remove every replica.\
D. Guarantee that the last write survives every hardware failure.

### Question 7 — Rate limiting

A fixed-window counter expires 60 seconds after the first request. Does this enforce at most 50 requests in every rolling 60-second interval?

A. Yes, always.\
B. No, because bursts may occur across window boundaries.\
C. Yes, if the key is a Hash.\
D. Only when RDB is enabled.

### Question 8 — Lua scripts

Why is Lua useful for a rate limiter?

A. It automatically distributes keys across clusters.\
B. It can combine the increment, expiry initialization, and decision into one atomic Redis operation.\
C. It disables network failures.\
D. It guarantees an HTTP response without application code.

### Question 9 — Eviction and expiration

Which statement is most accurate?

A. TTL and eviction are exactly the same mechanism.\
B. TTL is time-based expiration; eviction policies remove keys to manage memory pressure.\
C. TTL applies only to Lists.\
D. Eviction guarantees data will remain available forever.

### Question 10 — Distributed locks

What is a critical concern when a lock expires while its original holder is still working?

A. The original holder necessarily stops executing.\
B. Another holder may acquire the lock while the original holder continues acting.\
C. Redis deletes all unrelated keys.\
D. The primary database becomes read-only.

## Section B — Short-answer questions (20 marks)

### Question 11 — Choose the correct data type (6 marks)

For each scenario, choose a Redis data type and justify it.

1. An account profile with named fields.
2. A leaderboard with score-based ranking.
3. A deduplicated collection of group members.
4. A FIFO queue where order matters and duplicates are allowed.
5. A compact approximate distinct-visitor counter.
6. An append-oriented event pipeline with consumer groups.

### Question 12 — TTL, invalidation, and eviction (5 marks)

Explain the difference between TTL expiration, explicit cache invalidation, and eviction. Give a situation where each mechanism is useful.

### Question 13 — RESP and AOF (4 marks)

Explain why this command representation can be stored in an AOF:

```
*3
$3
SET
$3
cat
$3
dog
```

Explain the difference between a command's byte length and its character length.

### Question 14 — Database choice (5 marks)

A service needs simple object caching, while another service needs persistent counters, sorted rankings, and event streams.

Compare Redis and Memcached for these two requirements. Explain why the feature-rich option is not automatically the best option for every workload.

## Section C — Practical exercises (25 marks)

### Question 15 — Rate limiter repair (8 marks)

A developer writes:

```
local count = redis.call('INCR', KEYS[1])
redis.call('EXPIRE', KEYS[1], 60)

if count > 50 then
    return 0
end

return 1
```

1. Identify the main TTL bug. (2 marks)
2. Repair the script for a fixed 60-second window. (3 marks)
3. Explain the meaning of the return values. (1 mark)
4. Explain one reason this is not a strict rolling-window limiter. (2 marks)

### Question 16 — Leaderboard interpretation (7 marks)

Commands:

```
ZADD leaderboard 400 Alice 350 Bob 500 Cara
ZINCRBY leaderboard 75 Alice
ZINCRBY leaderboard 50 Bob
ZREVRANGE leaderboard 0 2 WITHSCORES
ZREVRANK leaderboard Bob
ZCARD leaderboard
```

Provide:

1. Final scores. (3 marks)
2. Descending order. (2 marks)
3. Bob's zero-based rank. (1 mark)
4. Number of unique members. (1 mark)

### Question 17 — Inventory script (10 marks)

Stock is stored in `inventory:product100`. Write a Lua script that:

- Reads stock, treating a missing key as zero.
- Rejects quantities less than or equal to zero.
- Rejects a request larger than available stock.
- Decrements stock only when the request is valid.
- Returns a clear code or resulting stock value.

Explain why the script prevents a race between the check and decrement.

## Section D — Integrated scenarios (35 marks)

### Question 18 — E-commerce architecture (12 marks)

An e-commerce application has:

- Frequently viewed product details
- Product prices that can change
- A leaderboard for customer reward points
- Temporary user sessions
- Confirmed orders that must be durable

Design a solution using Redis and a persistent database.

Explain:

1. Which data should remain authoritative in the persistent database. (2 marks)
2. Suitable Redis data types for product cache, leaderboard, and sessions. (3 marks)
3. A cache-aside and invalidation strategy for product prices. (3 marks)
4. Persistence and availability considerations. (2 marks)
5. Two risks and how to mitigate them. (2 marks)

### Question 19 — Lock and inventory failure analysis (11 marks)

Two application servers sell the final unit of a product.

Server A acquires a ten-second Redis lock. It pauses for fifteen seconds. The lock expires, and Server B acquires a new lock and sells the unit. Server A resumes and attempts its own sale.

1. Explain why the lock's expiry creates a risk. (3 marks)
2. Explain why token-checked lock release alone does not prevent Server A's stale business operation. (2 marks)
3. Recommend a robust stock-update design. (3 marks)
4. Explain fencing tokens or another suitable stale-owner defense. (3 marks)

### Question 20 — Recovery and consistency design (12 marks)

A high-traffic service stores cached query results and temporary sessions in Redis. It also maintains a separate persistent database.

The service must tolerate Redis node failures, minimize loss of important session changes, and recover from accidental deletion.

Recommend a design and explain:

1. Persistence choices and synchronization trade-offs. (3 marks)
2. Replication and failover. (3 marks)
3. Independent backups and recovery testing. (2 marks)
4. Cache invalidation and stale-data handling. (2 marks)
5. Two limitations that remain. (2 marks)

## End of exam

**Total: 100 marks.**

# File 7 — `practice-exam-3-solutions.md`

Writing

# Practice Exam 3 — Comprehensive Solutions

**Total:** 100 marks

## Section A — Multiple-choice answers (20 marks)

| Question | Answer | Explanation |
| --- | --- | --- |
| 1 | B | A cache miss can cause the application to fetch the authoritative value and repopulate Redis. |
| 2 | B | `TTL` returns `-2` when the key does not exist. |
| 3 | B | HyperLogLog estimates distinct cardinality using a probabilistic method. |
| 4 | C | Lists preserve order and permit duplicate values. |
| 5 | B | Write-through adds work to relevant writes and can cache data that is rarely read. |
| 6 | B | AOF rewriting compacts the log representation of the current dataset. |
| 7 | B | Fixed-window boundaries can permit bursts across adjacent windows. |
| 8 | B | Lua can execute related Redis commands atomically. |
| 9 | B | TTL is time-based expiration; eviction responds to memory-management policy. |
| 10 | B | An expired lock does not stop the original process from executing. |

## Section B — Model answers (20 marks)

### Question 11 — Data types (6 marks)

1. **Hash:** Named account fields.
2. **Sorted Set:** Unique members associated with scores and ordered by score.
3. **Set:** Unique group members.
4. **List:** Ordered sequence supporting duplicates and operations at both ends.
5. **HyperLogLog:** Approximate distinct-count estimation with low memory use.
6. **Stream:** Append-oriented events and consumer-group processing.

**Marking:** 1 mark per correct type and justification.

### Question 12 — TTL, invalidation, and eviction (5 marks)

- **TTL expiration:** A key becomes unavailable after its configured lifetime. Example: a temporary session or cached API response.
- **Explicit invalidation:** An application removes or refreshes a key because its value is no longer valid. Example: deleting `product:123` after its price changes.
- **Eviction:** Redis removes keys according to the configured eviction policy when memory management requires it. Example: a cache configured to evict less recently used keys under memory pressure.

TTL does not guarantee freshness throughout the key's lifetime. Explicit invalidation can respond immediately to a known update, while eviction is primarily a memory-management mechanism.

**Marking:**

- TTL definition and example: 2 marks.
- Invalidation definition and example: 1.5 marks.
- Eviction definition and example: 1.5 marks.

### Question 13 — RESP and AOF (4 marks)

The command is:

```
SET cat dog
```

`*3` denotes an array of three elements. The three bulk strings are `SET`, `cat`, and `dog`. Each has a length of three bytes in this ASCII example.

AOF records write operations in a protocol-based representation that Redis can replay during recovery.

For ASCII, character count and byte count match. For multibyte encodings such as UTF-8, a character can occupy multiple bytes, so RESP length markers must count bytes rather than abstract characters.

**Marking:**

- Command identification: 1 mark.
- Array and bulk strings: 1 mark.
- AOF replay: 1 mark.
- Byte-versus-character distinction: 1 mark.

### Question 14 — Redis versus Memcached (5 marks)

For simple object caching, Memcached may be sufficient and operationally straightforward.

For persistent counters, score-based rankings, and event streams, Redis is generally a more suitable fit because it supports richer data structures, persistence options, and stream-oriented workflows.

The best choice depends on measured workload, required features, deployment complexity, memory behavior, and durability requirements. Extra features can introduce overhead and operational work without benefiting a simple cache.

**Marking:**

- Appropriate choice for simple caching: 1 mark.
- Appropriate choice for advanced structures: 2 marks.
- Persistence/data-type comparison: 1 mark.
- Workload-based justification: 1 mark.

## Section C — Practical exercises (25 marks)

### Question 15 — Rate limiter repair (8 marks)

**1\. TTL bug (2 marks)**

The script calls `EXPIRE` after every request. That continually resets the lifetime, so the counter does not behave as a fixed window measured from the first request.

**2\. Corrected script (3 marks)**

```
local count = redis.call('INCR', KEYS[1])

if count == 1 then
    redis.call('EXPIRE', KEYS[1], 60)
end

if count > 50 then
    return 0
end

return 1
```

**3\. Return values (1 mark)**

- `1`: Allow the request.
- `0`: Reject the request.

The application must interpret this result and produce the appropriate response, typically HTTP 429 for a rejected request.

**4\. Fixed-window limitation (2 marks)**

A user can make a burst near the end of one window and another burst immediately after the next window starts. The design does not guarantee a strict maximum in every rolling 60-second interval.

### Question 16 — Leaderboard interpretation (7 marks)

After the score updates:

- Alice: $400 + 75 = 475$
- Bob: $350 + 50 = 400$
- Cara: $500$

Descending order:

| Position | Player | Score |
| --- | --- | --- |
| 1 | Cara | 500 |
| 2 | Alice | 475 |
| 3 | Bob | 400 |

`ZREVRANK leaderboard Bob` returns `2`.

`ZCARD leaderboard` returns `3`, because the sorted set has three unique members.

**Marking:**

- Final scores: 3 marks.
- Correct order: 2 marks.
- Bob's rank: 1 mark.
- Cardinality: 1 mark.

### Question 17 — Inventory script (10 marks)

**Model solution**

```
local stock = tonumber(redis.call('GET', KEYS[1]) or '0')
local quantity = tonumber(ARGV[1])

if quantity <= 0 then
    return -2
end

if stock < quantity then
    return -1
end

local remaining = redis.call('DECRBY', KEYS[1], quantity)
return remaining
```

Example invocation:

```
EVAL "local s=tonumber(redis.call('GET',KEYS[1]) or '0'); local q=tonumber(ARGV[1]); if q<=0 then return -2 end; if s<q then return -1 end; return redis.call('DECRBY',KEYS[1],q)" 1 inventory:product100 4
```

Return values:

- `-2`: Invalid quantity.
- `-1`: Insufficient stock.
- Non-negative result: Remaining stock after a successful decrement.

The script reads stock, checks the requested quantity, and performs the decrement within one atomic execution. Another client cannot interleave its own Redis commands between those steps.

**Important limitation:** This script protects the stock value but does not automatically make an external order database transaction atomic with Redis. The application must coordinate order creation and stock changes appropriately.

**Marking:**

- Reads missing stock as zero: 1 mark.
- Rejects non-positive quantity: 2 marks.
- Rejects insufficient stock: 2 marks.
- Performs decrement only after validation: 2 marks.
- Correct return behavior: 1 mark.
- Explains atomicity: 2 marks.

## Section D — Integrated scenarios (35 marks)

### Question 18 — E-commerce architecture (12 marks)

**1\. Authoritative data (2 marks)**

Keep confirmed orders and authoritative product prices in the persistent database. Redis may cache product details and accelerate reads, but should not be the only copy of durable order records.

**2\. Redis data types (3 marks)**

- Product details: String containing a serialized product, or a Hash containing individual fields.
- Reward leaderboard: Sorted Set.
- Temporary sessions: String or Hash, with an appropriate TTL.

**3\. Cache-aside and invalidation (3 marks)**

For reads, check Redis first. On a miss, query the database and populate Redis.

When the price changes, update the database and invalidate the corresponding Redis key, or refresh the cached value. A TTL provides an additional limit on stale entries but is not a substitute for prompt invalidation when immediate freshness matters.

Concurrent reads and writes may still produce stale values if an older read repopulates the cache after invalidation. Where necessary, use versioning or stronger coordination.

**4\. Persistence and availability (2 marks)**

Choose persistence according to the value of Redis-only data and acceptable recovery loss. Use replication and an appropriate failover mechanism if Redis service availability matters. Temporary cache entries can often be reconstructed; irreplaceable state requires stronger protection.

**5\. Risks and mitigation (2 marks)**

Any two:

- Stale product prices: invalidation, TTL, or versioning.
- Lost recent Redis writes: persistence and replication, while recognizing their limits.
- Duplicate or lost order effects: authoritative database transactions and idempotency.
- Cache stampede: request coalescing, controlled refresh, or staggered TTLs.
- Memory pressure: capacity planning and an appropriate eviction policy.

### Question 19 — Lock and inventory failure analysis (11 marks)

**1\. Expiration risk (3 marks)**

Server A's lock expires while A is paused. Server B acquires a new lock and sells the final unit. Server A resumes and may continue its stale operation even though it no longer owns the lock.

The lock coordinates ownership only while its ownership assumptions remain valid; expiration does not terminate a process.

**2\. Token-checked release (2 marks)**

A unique token prevents A from deleting B's new lock. However, it does not automatically prevent A from submitting an inventory update after resuming.

Lock release safety and business-operation safety are separate requirements.

**3\. Robust stock update (3 marks)**

Use an atomic stock-validation-and-decrement script in Redis, or enforce the invariant using an authoritative transactional database update.

For example, the database can conditionally update stock only when enough units remain and verify that exactly one row was updated. Order creation and stock decrement should be coordinated using transactions or a reliable workflow.

**4\. Fencing tokens or stale-owner defense (3 marks)**

A fencing-token system issues monotonically increasing ownership numbers. The protected resource rejects operations carrying a token older than the latest accepted token. Thus, a paused former owner cannot overwrite the result of a newer owner merely because it resumed.

A conditional database update or another authoritative atomic invariant can also protect the stock from overselling. The defense must be enforced at the resource being modified; checking a Redis lock only at acquisition time is insufficient.

### Question 20 — Recovery and consistency design (12 marks)

**1\. Persistence choices (3 marks)**

Use RDB for compact snapshots and recovery points. Add AOF if recent session changes need finer-grained recovery. Choose an AOF synchronization policy based on the acceptable balance between latency and possible data loss.

**2\. Replication and failover (3 marks)**

Use replicas to maintain redundant copies. Use Sentinel for primary–replica monitoring and failover, or Cluster when sharding is also required. Understand that asynchronous replication may lag and failover can lose recent writes.

**3\. Backups and testing (2 marks)**

Keep versioned backups outside the Redis host and test restoration. Independent backups help recover from accidental deletion, corruption, and loss of the original machine.

**4\. Cache freshness (2 marks)**

Invalidate or refresh cached results when the authoritative database changes. Use TTL as a fallback limit on stale data. Handle concurrent writes and reads where stale repopulation would be unacceptable.

**5\. Remaining limitations (2 marks)**

Any two:

- AOF synchronization cannot guarantee survival of every hardware or storage failure.
- Replication does not ensure zero loss of acknowledged writes.
- Failover can interrupt requests.
- Backups can be incomplete or unusable without testing.
- A distributed system can experience consistency/availability trade-offs during partitions.
- Memory pressure can trigger eviction and cache misses.

## Overall grading guidance

Award full credit when the answer identifies the correct mechanism, explains how it works, and acknowledges its limitations. A solution that merely names Redis features without explaining failure behavior should not receive full marks.

## How to use these files

1. Create the seven files listed in `README.md` in your GitHub repository.
2. Copy each corresponding Markdown document into its file.
3. Complete each exam before opening its solutions file.
4. Record your score and review every lost mark, particularly where the mistake involved a concurrency or durability assumption.

Recommended priority: Start with Practice Exam 1, use Exam 2 to test distributed-systems reasoning, and finish with Exam 3 as the integrated design assessment.

The three papers and their solutions are now provided above in GitHub-ready Markdown format. The remaining step for a downloadable repository is to save the contents into the individual `.md` files; no downloadable files have been attached in this response.
