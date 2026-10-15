# T5 — In-Memory Databases (Redis)

## Exam Compression — Part 4 of 4

**Suggested filename:** `t5-redis-exam-compression-part4.md`

## 1\. Chapter objectives

You should be able to:

- Explain Redis Hashes and Sorted Sets.
- Describe TTL (Time to Live) and key expiration.
- Compare lazy loading and write-through caching strategies.
- Explain cache hits, cache misses, and cache invalidation challenges.
- Select appropriate caching strategies for different workloads.
- Explain the trade-offs between cache freshness, memory usage, latency, and database load.

## 2\. Redis Hashes

A **Hash** stores a mapping of string fields to string values under a single Redis key.

It is useful for representing objects with multiple attributes without requiring a separate Redis key for every field.

### Example

```
HSET user:1001 name "Eduardo" age "25" city "Singapore"
HGET user:1001 name
HGETALL user:1001
```

The Hash `user:1001` contains three fields: `name`, `age`, and `city`.

- `HSET` adds or updates fields.
- `HGET` retrieves one field.
- `HGETALL` retrieves all fields and values.

### Characteristics

- Stores multiple field–value pairs under one key.
- Allows individual fields to be added, retrieved, or removed.
- Supports numeric operations on suitable field values, such as incrementing a counter.
- Can represent objects such as user profiles, product information, or application settings.
- The lecture gives a maximum of $2^{32}-1$ field–value pairs per Hash.

**Exam takeaway:** Use a Hash when an object contains multiple attributes that should be accessed together or individually.

## 3\. Redis Sorted Sets

A **Sorted Set** stores unique members, each associated with a numerical score. Members are ordered by score.

### Characteristics

- Every member is unique within the Sorted Set.
- Each member has an associated score.
- Members can be retrieved in score order.
- Supports ranking and score-based retrieval.

### Example: leaderboard

```
ZADD leaderboard 100 "Alice"
ZADD leaderboard 250 "Bob"
ZADD leaderboard 180 "Charlie"
ZREVRANGE leaderboard 0 2 WITHSCORES
```

The members have these scores:

| Member | Score |
| --- | --- |
| Alice | 100 |
| Bob | 250 |
| Charlie | 180 |

The reverse-range query returns members in descending score order: Bob, Charlie, Alice.

### Typical use cases

- Gaming leaderboards.
- Ranking products by popularity.
- Scheduling items according to priority scores.
- Retrieving the highest- or lowest-scoring members.

**Exam distinction:** A normal Set guarantees uniqueness but has no guaranteed ordering. A Sorted Set guarantees unique members and orders them by score.

## 4\. Redis data types: comparison

| Data type | Defining property | Example application |
| --- | --- | --- |
| String | One byte sequence per key | Session token or cached response |
| Hash | Field–value pairs | User profile |
| List | Ordered sequence; duplicates allowed | Queue or activity timeline |
| Set | Unique, unordered members | Unique visitors |
| Sorted Set | Unique members ordered by score | Leaderboard |
| Bitmap | Bit-level operations | Daily activity flags |
| Bitfield | Packed integer fields | Compact counters |
| Geospatial index | Geographic location queries | Nearby places |
| HyperLogLog | Approximate distinct count | Unique visitor estimation |
| Stream | Append-oriented event entries | Event processing |

**Quick selection rule:**

- Multiple object attributes → Hash.
- Ordered sequence with duplicates → List.
- Unique membership → Set.
- Unique members ranked by score → Sorted Set.
- Approximate number of distinct items → HyperLogLog.
- Append-oriented event records → Stream.

## 5\. TTL — Time to Live

**TTL (Time to Live)** specifies how long a Redis key remains valid before it expires.

It is especially useful in caching because memory is limited and cached values may become outdated.

### How TTL works

1. A key is stored with an expiration time.
2. Redis tracks the key's remaining lifetime.
3. When the key expires, it is no longer available as a valid key.
4. If the application subsequently requests that value, it may retrieve the data from the primary database and repopulate the cache.

### Example

```
SET session:1001 "active" EX 60
TTL session:1001
```

The `EX 60` option sets an expiration of 60 seconds.

After the key expires, a subsequent lookup returns no value unless the key has been recreated.

### Important distinctions

| Situation | Meaning |
| --- | --- |
| Positive TTL | The key has a limited remaining lifetime. |
| TTL of zero | In Redis, a key with an expiration time of zero expires immediately. |
| No expiration | The key has no expiration time and can remain until deleted or removed by an applicable eviction policy. |
| Expired key | The key is no longer available as a valid entry. |

**Important correction:** A Redis key without a TTL does not automatically expire simply because memory is needed. It can remain indefinitely unless deleted or removed by an applicable eviction policy. Also, Redis's `TTL` command can return `-1` for a key without an expiration and `-2` when the key does not exist.

### Why TTL matters

- Automatically removes time-sensitive data.
- Helps prevent cached values from remaining valid indefinitely.
- Supports session expiration and temporary tokens.
- Can reduce memory pressure when keys expire.
- Helps control cache freshness.

**Exam takeaway:** TTL controls how long a key remains valid. It is an expiration mechanism, not a guarantee that every cached value is always fresh.

## 6\. Database synchronization and caching strategies

A cache stores copies of data, while the primary database commonly remains the authoritative source.

When the database changes, the cache must be managed to avoid stale data.

The lecture covers two strategies:

1. Lazy loading.
2. Write-through caching.

## 7\. Lazy loading strategy

**Definition:** Lazy loading retrieves data from the cache when requested and loads it into the cache only when necessary.

It is also commonly called cache-aside when the application explicitly manages cache reads and population.

### How it works

1. The user requests data.
2. The application checks the cache.
3. **Cache hit:** Return the cached value.
4. **Cache miss:** Retrieve the value from the primary database.
5. Return the result and populate the cache for future requests.

### Example

A product page is requested for the first time:

- The product is not in Redis.
- The application queries the primary database.
- The result is returned to the user and stored in Redis.
- Future requests can retrieve the product from Redis until the cache entry expires or is invalidated.

### Advantages

- Only requested data is cached.
- Avoids filling the cache with data nobody accesses.
- Reduces unnecessary cache writes.
- Works well when reads are frequent and writes are infrequent.

### Disadvantages

- The first request for an uncached value incurs extra latency.
- Cache misses require a database lookup and a cache-population operation.
- Database updates do not automatically update the cache.
- Cached values may become stale unless expiration or invalidation is implemented.

### Best use cases

- Large datasets where only a subset of items is frequently requested.
- Product catalogues with repeated reads.
- User profiles that are read often but updated infrequently.
- Applications where occasional cache misses are acceptable.

**Exam takeaway:** Lazy loading saves memory by caching only requested data, but cache misses are slower and database updates can leave cached copies stale.

## 8\. Write-through caching strategy

**Definition:** In a write-through strategy, data is written to the primary database and the cache is updated as part of the write operation.

### How it works

1. The application receives a write or update.
2. The primary database is updated.
3. The cache is updated with the corresponding value.

The exact ordering and error-handling mechanism depend on the implementation.

### Advantages

- Updated values can be placed in the cache immediately after database writes.
- Reduces the chance that subsequent reads encounter outdated cached values.
- Avoids some cold-cache misses for data that has just been written.
- Suitable when recently written data is likely to be read soon.

### Disadvantages

- Every relevant write requires cache-update work.
- Data that is never read may still occupy cache memory.
- Additional cache writes can increase cost and resource usage.
- If the database update succeeds but the cache update fails, inconsistency is still possible unless retries or other safeguards are implemented.

**Important qualification:** Write-through does not guarantee that the cache can never become stale. That depends on whether cache updates succeed and how concurrent writes, failures, and invalidation are handled.

### Best use cases

- Frequently accessed records that are also updated.
- Applications where fresh cached values are important.
- Workloads where recently written information is likely to be requested soon.

## 9\. Lazy loading versus write-through

| Feature | Lazy loading | Write-through |
| --- | --- | --- |
| When data enters the cache | On a cache miss | During or immediately after a relevant write |
| Initial read | May require a database lookup | May be served from cache if already populated |
| Memory efficiency | Generally better when many records are never requested | May cache records that are never read |
| Cache misses | Common on first access or after expiration | Can be reduced for recently written, cached data |
| Write overhead | Does not require populating the cache on every write | Requires corresponding cache-update work |
| Stale-data risk | Database changes may leave cached values stale | Updates can keep the cache fresher if cache writes succeed |
| Best suited for | Read-heavy workloads with a limited hot set | Workloads where frequently accessed data must remain fresh |

**Memory trick:**

- **Lazy loading:** Load when needed.
- **Write-through:** Update the cache when writing.

### Exam scenario 1

A website contains millions of products, but customers view only a small fraction of them.

**Best choice:** Lazy loading.

**Reason:** It caches only requested products and avoids filling RAM with rarely accessed items.

### Exam scenario 2

A system frequently reads recently updated account information and places high importance on cache freshness.

**Potential choice:** Write-through.

**Reason:** Relevant writes also update the cache, reducing the chance that subsequent reads return older values.

## 10\. TTL and caching strategies together

TTL and cache synchronization solve related but different problems.

- **TTL** determines when a cache entry expires.
- **Lazy loading** determines when data is fetched into the cache.
- **Write-through** determines how the cache is updated alongside database writes.
- **Cache invalidation** removes or replaces cached values that are no longer valid.

These mechanisms can be combined.

For example, a product catalogue might use lazy loading to cache products only when requested, a TTL of 300 seconds to limit how long each entry remains valid, and explicit invalidation when a product is updated.

### Key distinction

Expiration does not necessarily make a cache immediately consistent with the database. Until the expiration occurs—or the entry is invalidated or refreshed—a cached value may remain stale.

## 11\. Potential exam practice questions

These are practice questions derived from the lecture content, not confirmed questions from the lecturer.

### Q1. What is a Redis Hash, and when should it be used?

**Answer:** A Hash stores multiple string field–value pairs under one Redis key.

**Explanation:** It is suitable for objects such as user profiles, where individual attributes can be retrieved or updated separately.

### Q2. What distinguishes a Sorted Set from a normal Set?

**Answer:** A Sorted Set associates each unique member with a score and orders members by score. A normal Set stores unique members without guaranteed order.

**Explanation:** Sorted Sets are useful for leaderboards, while Sets are useful for unique membership.

### Q3. What does TTL mean in Redis?

**Answer:** TTL means Time to Live. It determines how long a key remains valid before expiration.

**Explanation:** TTL is useful for sessions, temporary tokens, and cache entries that should not remain valid indefinitely.

### Q4. What happens when a cached key expires?

**Answer:** The key is no longer available as a valid cache entry. The application may retrieve the data from the primary database and repopulate the cache.

**Explanation:** Expiration does not itself retrieve the data from the database; the application must implement the retrieval and cache-population process.

### Q5. What is lazy loading?

**Answer:** Data is loaded into the cache only when it is requested and absent from the cache.

**Explanation:** This saves memory by avoiding the caching of data that is never requested, but the first request can be slower.

### Q6. State two advantages and two disadvantages of lazy loading.

**Answer:**

Advantages:

- Only requested data is cached.
- Avoids unnecessary cache population.

Disadvantages:

- Cache misses require a database lookup.
- Database updates can leave cached values stale.

### Q7. What is write-through caching?

**Answer:** It is a strategy in which the primary database is updated and the corresponding cache entry is updated as part of the same application write flow.

**Explanation:** It helps keep cached values fresh, but adds write overhead and still requires handling cache-update failures.

### Q8. Compare lazy loading and write-through.

**Answer:** Lazy loading populates the cache on demand, while write-through updates the cache alongside database writes.

**Explanation:** Lazy loading tends to conserve memory; write-through can keep frequently accessed data fresher.

### Q9. Why might TTL be combined with lazy loading?

**Answer:** Lazy loading caches only requested values, while TTL limits how long those values remain in the cache.

**Explanation:** Together, they can improve memory efficiency and limit the lifetime of potentially stale cached data.

### Q10. Does write-through guarantee that the cache is never stale?

**Answer:** No.

**Explanation:** Cache updates can fail, and concurrent operations or other update paths can introduce inconsistencies. The application needs appropriate error handling and synchronization.

### Q11. A service caches every record after it is written, even if most records are never read. What is the main disadvantage?

**Answer:** Unnecessary cache memory consumption and extra cache-update work.

**Explanation:** Write-through can populate the cache with data that provides little performance benefit.

### Q12. Explain the difference between TTL and cache invalidation.

**Answer:** TTL expires a key after its configured lifetime. Cache invalidation removes or refreshes a value when it is no longer valid, often because the underlying data has changed.

**Explanation:** TTL is time-based; invalidation can respond directly to data changes.

## 12\. Quick revision checklist

- [ ]Explain Redis Hashes and their field–value structure.
- [ ]Explain Sorted Sets and score-based ranking.
- [ ]Distinguish Hashes, Lists, Sets, and Sorted Sets.
- [ ]Define TTL and explain key expiration.
- [ ]Explain why keys without TTLs can remain in memory indefinitely.
- [ ]Describe the lazy-loading workflow, including hits and misses.
- [ ]Describe the write-through workflow.
- [ ]Compare lazy loading and write-through advantages and disadvantages.
- [ ]Explain how stale cache values can arise.
- [ ]Distinguish TTL, invalidation, and cache synchronization.
- [ ]Select an appropriate caching strategy for a practical scenario.

## 13\. Final chapter takeaways

- **Redis Hash:** Stores multiple field–value pairs under one key.
- **Sorted Set:** Stores unique members ordered by numerical scores.
- **TTL:** Limits how long a key remains valid.
- **Lazy loading:** Caches data only when it is requested.
- **Write-through:** Updates the cache alongside database writes.
- **Cache invalidation:** Removes or refreshes data that is no longer valid.
- **Core trade-off:** Cache design balances latency, memory consumption, database load, and data freshness.

## 14\. Scope of this compression

This document covers **Chunk 4 of 4**, completing the supplied T5 Redis lecture material.

It focuses on Hashes, Sorted Sets, TTL, lazy loading, write-through caching, and cache synchronization.

**Merge instruction:** Combine Parts 1–4 into a single consolidated T5 exam revision document. Remove duplicate definitions and repeated questions, retain every distinct examinable concept, and preserve important comparisons, technical details, practical examples, and likely exam questions.

## All four chunks are complete

You now have the four exam compressions. When consolidating them, pay particular attention to these high-value comparisons:

1. RDB vs. AOF — snapshots versus operation logs.
2. Standalone vs. HA vs. Sentinel vs. Cluster — simplicity, replication, automatic failover, and sharding.
3. Redis data types — especially Hash vs. List vs. Set vs. Sorted Set.
4. Lazy loading vs. write-through — memory efficiency versus proactive cache updates.
5. TTL vs. cache invalidation — time-based expiration versus responding to data changes.

These comparisons are particularly useful for scenario-based exam questions.
