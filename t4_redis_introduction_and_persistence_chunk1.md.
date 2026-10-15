# T5 — In-Memory Databases (Redis)

## Exam Compression — Part 1 of 4

**Suggested filename:** `t5-redis-exam-compression-part1.md`

## 1\. Chapter objectives

You should be able to:

- Explain why in-memory databases are fast and when they are useful.
- Describe caching strategies and identify suitable data for caching.
- Explain the key-value data model and Redis's main characteristics.
- Compare Redis and Memcached.
- Explain the performance, capacity, persistence, and recovery trade-offs of in-memory databases.
- Understand how replication, horizontal scalability, and availability relate to distributed key-value databases.
- Apply Redis concepts to practical scenarios.

## 2\. NoSQL recap

NoSQL databases are designed to provide flexible data models, scalability, and performance for applications with different requirements.

The four major NoSQL models are:

| Model | Main idea |
| --- | --- |
| Document | Stores semi-structured documents, commonly JSON-like objects. |
| Key-value | Stores values accessed using unique keys. |
| Column-based | Organizes data into column families or columns, depending on the system. |
| Graph | Represents entities as nodes and their relationships as edges. |

### Important concepts

- **ACID:** Atomicity, Consistency, Isolation, and Durability; associated with reliable transaction processing.
- **BASE:** Basically Available, Soft state, and Eventual consistency; associated with systems that prioritize availability and tolerate temporary inconsistency.
- **CAP theorem:** During a network partition, a distributed system cannot guarantee both full consistency and full availability for every request. The appropriate trade-off depends on the system's design.

**Exam takeaway:** No database model is universally best. Select a database according to the application's access patterns, consistency needs, performance requirements, and scalability goals.

## 3\. In-memory databases

An in-memory database primarily stores and processes data in RAM rather than relying on disk for every operation.

### Advantages

- Very fast data access.
- Low latency, suitable for real-time applications.
- Useful for frequent reads and rapid updates.
- Can support horizontal scaling in suitable distributed architectures.

### Limitations

- RAM is more expensive per unit of capacity than many disk-based storage options.
- Available memory limits the amount of data that can be held in memory.
- Data may be lost after a failure if persistence or recovery mechanisms are inadequate.
- Recovery, persistence, replication, and distributed operation introduce additional complexity.

**Critical distinction:** In-memory does not necessarily mean temporary or volatile-only. Redis supports persistence mechanisms that can preserve data beyond the lifetime of the process.

### Why memory is faster

RAM can serve data without the mechanical and storage-access overhead associated with traditional disk-based operations. CPU caches and memory hierarchies also matter, but the central exam idea is that keeping frequently needed data close to the processing system reduces access latency.

The lecture's storage-latency analogy reinforces this principle: the farther data is from the CPU in the memory/storage hierarchy, the more expensive access generally becomes.

## 4\. Caching

A cache stores a reusable copy of data so that an application can retrieve it more quickly than recomputing it or fetching it from the original data source.

### Analogy

Keeping frequently used tools in a nearby shed avoids repeatedly travelling to a distant hardware store. Similarly, a cache keeps frequently needed data close to the application.

### What should be cached?

1. **Static, frequently accessed data:** Profile information, images, CSS, JavaScript, and other content that changes infrequently.
2. **Expensive computational results:** Results of recommendation algorithms, simulations, and other costly calculations.
3. **Slow or complex database queries:** Frequently requested query results, especially when producing them requires expensive joins or substantial processing.

### Cache benefits

- Reduces response time.
- Reduces repeated database queries and computation.
- Decreases load on the underlying database.
- Improves the ability to handle repeated requests.

### Cache trade-offs

- Cached values can become stale when the original data changes.
- Cache invalidation and refresh strategies require careful design.
- A cache consumes memory and may experience misses.
- A cache should not automatically be treated as the authoritative source of data.

**Exam tip:** The best caching candidates are generally expensive to retrieve or compute and are accessed frequently. Data that changes constantly may still be cached, but requires an appropriate expiration or invalidation strategy.

## 5\. Why key-value databases?

A key-value database is one of the simplest NoSQL models. It stores a value under a unique key.

Example:

```
user:1001 -> "Eduardo"
```

Here:

- `user:1001` is the key.
- `Eduardo` is the value.

A Python dictionary is a useful analogy:

```
users = {
    "user:1001": "Eduardo"
}
```

The database uses the key to retrieve the associated value.

### Key-value characteristics

- Keys identify values and must be unique within the relevant keyspace.
- Values can have different representations, depending on the database.
- Simple key-based access can be highly efficient.
- The model is well suited to caching, counters, sessions, and other predictable access patterns.

Redis extends the basic key-value model by supporting specialized data structures, including strings, hashes, lists, sets, sorted sets, and streams.

**Exam distinction:** A key-value database is not necessarily limited to storing plain strings. Redis values can be structured using its supported data types.

## 6\. Reliability, scalability, and availability

The lecture emphasizes three properties of distributed key-value systems.

### Reliability and fault tolerance

Replication maintains copies of data on multiple nodes. Replication can improve fault tolerance and help a system continue operating after some failures, although it does not guarantee that every acknowledged write survives every failure.

### Horizontal scalability

Horizontal scaling adds machines or nodes rather than relying exclusively on a larger individual machine.

In a partitioned or sharded system, data is distributed among nodes. A hashing or partitioning mechanism can help determine where a key belongs.

Benefits include distributing storage and request load across multiple machines.

### Availability

Availability concerns whether the system can respond to requests when components fail or become unreachable. Replicas may help serve requests when a node fails, depending on the replication and consistency configuration.

### CAP theorem: important qualification

The lecture summarizes Redis clusters as generally prioritizing availability and partition tolerance. Treat this as a simplified description, not a universal guarantee.

In CAP terminology, when a network partition occurs, a distributed system must make trade-offs between consistency and availability. Redis behavior depends on the deployment, replication configuration, and operation being performed.

**Likely exam question:** Why is replication not the same as backup?

**Answer:** Replication copies data between operational nodes and primarily supports availability and fault tolerance. Accidental deletion or corruption can also propagate to replicas, so independent backups and recovery procedures may still be necessary.

## 7\. Redis versus Memcached

Both are widely used in-memory technologies, but their capabilities differ.

| Feature | Redis | Memcached |
| --- | --- | --- |
| Primary use | In-memory data store and cache | Distributed in-memory cache |
| Data structures | Rich built-in data structures | Primarily simple key-value items |
| Persistence | Supports persistence | Traditionally cache-oriented, without Redis-style persistence |
| Typical strengths | Caching, counters, rankings, sessions, and more | Simple, high-speed object caching |
| Selection | Better when advanced data structures or persistence are required | Suitable when straightforward caching is sufficient |

### How to choose

- Choose **Redis** when you need advanced data structures, counters, sorted-set rankings, expiration controls, or persistence.
- Choose **Memcached** when simple object caching meets the requirements and its operational model is appropriate.

Do not conclude that Redis is always faster or always better. Performance depends on workload, configuration, data structures, network conditions, and implementation.

## 8\. Essential exam comparisons

| Concept A | Concept B | Main distinction |
| --- | --- | --- |
| RAM | Disk | RAM typically offers lower access latency; disk provides economical persistent capacity. |
| Cache | Primary database | A cache accelerates access; the primary database commonly remains the authoritative data source. |
| ACID | BASE | Different approaches to transactions, consistency, and availability. |
| Vertical scaling | Horizontal scaling | Add resources to one machine versus add machines or nodes. |
| Replication | Sharding | Maintain copies of data versus distribute different portions of data. |
| Redis | Memcached | Richer data structures and persistence versus primarily straightforward caching. |
| In-memory | Volatile-only | In-memory describes the main access/storage model, not an absolute absence of persistence. |

## 9\. Potential exam practice questions

These are additional practice questions derived from the lecture content, not confirmed questions from the lecturer.

### Q1. Why can an in-memory database be faster than a disk-oriented database?

**Answer:** It can access data in RAM without requiring disk access for every operation.

**Explanation:** RAM generally has much lower access latency than disk storage. Actual application performance still depends on the query, network, serialization, concurrency, and other system overheads.

### Q2. Name three appropriate candidates for caching.

**Answer:**

- Frequently accessed static content.
- Results of expensive computations.
- Frequently requested, expensive database queries.

**Explanation:** These candidates offer benefits when reusing a cached result avoids substantial repeated work.

### Q3. A product catalogue is requested thousands of times but changes only occasionally. Would caching help?

**Answer:** Yes.

**Explanation:** Repeated reads can be served from the cache, reducing response time and database load. Updates require an appropriate invalidation, refresh, or expiration strategy to prevent stale catalogue information.

### Q4. Why is a key-value model suitable for caching user sessions?

**Answer:** Each session can be stored under a unique session key and retrieved directly.

**Explanation:** Predictable key-based lookups make retrieval straightforward. Expiration can also remove sessions automatically when their validity period ends.

### Q5. Does storing data in RAM guarantee that it survives a server restart?

**Answer:** No.

**Explanation:** Persistence and recovery configuration determine whether data can be recovered after a restart or failure. Redis provides persistence options, but their protection depends on how they are configured.

### Q6. What is the difference between replication and horizontal scaling?

**Answer:** Replication creates copies of data; horizontal scaling distributes work or data across additional nodes.

**Explanation:** Replicas can support availability and fault tolerance, while sharding can distribute storage and request load. A system may use both.

### Q7. A system must rank players by score and retrieve the top five efficiently. Which is the more suitable choice: Redis or Memcached?

**Answer:** Redis.

**Explanation:** Redis sorted sets support score-based ordering and ranking operations. Memcached's basic caching model does not provide the same native sorted-set functionality.

### Q8. What does the CAP theorem imply during a network partition?

**Answer:** A distributed system cannot guarantee both full consistency and full availability for every request during the partition.

**Explanation:** A system may reject or delay some requests to preserve consistency, or continue answering requests with a risk of returning stale or inconsistent data.

### Q9. Why might caching an expensive database query be worthwhile even if the result changes occasionally?

**Answer:** If requests are frequent enough, the reduction in repeated query work can outweigh the cost of refreshing the cached result.

**Explanation:** The design must balance performance against freshness, using an appropriate invalidation or expiration policy.

### Q10. Is Redis always the best choice because it has more features than Memcached?

**Answer:** No.

**Explanation:** More features do not automatically produce better performance or a better fit. Choose based on required functionality, workload, operational complexity, and measured performance.

### Q11. A service needs to handle a growing volume of requests. Explain how horizontal scaling can help.

**Answer:** Additional nodes can share storage and request-processing load.

**Explanation:** The system must distribute keys or requests appropriately and account for network overhead, rebalancing, replication, and consistency.

### Q12. Why should replicated data still be backed up?

**Answer:** Replication alone does not protect against every kind of data loss.

**Explanation:** Logical mistakes, accidental deletion, and corruption may propagate to replicas. Independent backups provide a separate recovery mechanism.

## 10\. Quick revision checklist

Before the exam, make sure you can explain:

- [ ]The four major NoSQL data models.
- [ ]ACID, BASE, and the CAP theorem.
- [ ]The benefits and limitations of in-memory databases.
- [ ]How caching improves performance and what makes data a good caching candidate.
- [ ]How a key-value database organizes data.
- [ ]The difference between Redis and Memcached.
- [ ]Replication, sharding, horizontal scaling, and availability.
- [ ]Why persistence, backups, and cache invalidation matter.

## 11\. Scope of this compression

This document covers only **Chunk 1 of 4**. It captures the concepts present in the supplied material, including the introductory Redis material. Later chunks may introduce additional commands, persistence mechanisms, eviction policies, replication details, clustering, or deployment options. Those topics should be integrated only when they appear in the remaining lecture material or are explicitly added as supplementary explanations.

**Merge instruction:** Keep this document as Part 1. After all four chunks have been compressed, combine their key concepts, remove duplicated explanations, retain all distinct examinable details, and produce one consolidated T5 exam revision document.

Send Chunk 2 of 4 when you're ready.
