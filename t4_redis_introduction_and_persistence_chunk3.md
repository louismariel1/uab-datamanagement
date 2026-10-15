Writing

# T5 — In-Memory Databases (Redis)

## Exam Compression — Part 3 of 4

**Suggested filename:** `t5-redis-exam-compression-part3.md`

## 1\. Chapter objectives

You should be able to:

- Explain the disadvantages and format of AOF persistence.
- Compare Redis Standalone, High Availability (primary–replica), Sentinel, and Cluster architectures.
- Explain replication, automatic failover, sharding, and hash slots.
- Identify Redis data types and select suitable types for practical scenarios.
- Explain how Redis stores commands in an AOF file.
- Distinguish high availability from horizontal scalability.

## 2\. AOF persistence: disadvantages and file format

### AOF disadvantages

The Append Only File (AOF) records write operations so Redis can reconstruct its dataset by replaying them during recovery.

Main disadvantages:

- **File size:** AOF can become larger than an RDB snapshot, especially when keys are updated frequently. AOF rewriting can reduce the file size.
- **Startup time:** Redis may need to replay many logged operations, making recovery slower than loading an RDB snapshot.
- **Performance overhead:** AOF persistence can add write-related overhead. Its impact depends on the configured fsync policy, workload, and hardware.
- **Performance variability:** Under heavy write loads, persistence and disk activity can affect latency.

### RDB versus AOF: practical choice

| Requirement | Generally suitable option | Reason |
| --- | --- | --- |
| Compact snapshots | RDB | Saves a point-in-time dataset. |
| Fast restoration from a snapshot | RDB | Loads the saved dataset without replaying the entire operation log. |
| Finer-grained recovery | AOF | Records write operations between snapshots. |
| Stronger recovery protection | AOF with an appropriate fsync policy | Reduces the potential loss of recent writes. |
| Multiple recovery options | Both RDB and AOF | Combines snapshots with operation logging. |

**Exam takeaway:** RDB stores dataset snapshots; AOF records write operations. AOF is not automatically lossless—durability depends on how frequently its log is synchronized to disk.

## 3\. Understanding the AOF file format

Redis serializes commands in a structured format based on **RESP (Redis Serialization Protocol)**.

Consider the Redis command:

```
SET hello world
```

The command has three elements: the command name `SET`, the key `hello`, and the value `world`.

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

### How to interpret the representation

| Element | Meaning |
| --- | --- |
| `*3` | The array contains three elements. |
| `$3` | The next bulk-string argument contains 3 bytes. |
| `SET` | The command name. |
| `$5` | The next bulk-string argument contains 5 bytes. |
| `hello` | The key. |
| `$5` | The next bulk-string argument contains 5 bytes. |
| `world` | The value. |

The line breaks above illustrate the structure. In the actual RESP encoding, each line is terminated by CRLF (`\r\n`).

**Important distinction:** The length markers specify **bytes**, not necessarily characters. For ASCII text such as `hello`, the character count and byte count are both five.

### Why this matters

- AOF records commands in a form Redis can process during recovery.
- Redis rebuilds its dataset by replaying the recorded operations.
- The resulting in-memory dataset reflects the commands replayed from the log.

**Exam tip:** Be able to explain `*3`, `$3`, and `$5` in an AOF/RESP example.

## 4\. Redis architectures: overview

Redis can be deployed in four common architectures:

| Architecture | Main purpose | Key characteristic |
| --- | --- | --- |
| Standalone | Simplicity | One Redis instance. |
| High Availability (primary–replica) | Redundancy and read scaling | A primary handles writes; replicas copy its dataset. |
| Sentinel | Monitoring and automatic failover | Promotes a replica when the primary fails. |
| Redis Cluster | Horizontal scaling and distributed storage | Partitions keys across multiple nodes using hash slots. |

**Critical distinction:** Sentinel and Cluster solve different problems. Sentinel manages availability for a primary–replica deployment; Redis Cluster partitions data across nodes and can also use replicas for fault tolerance.

## 5\. Standalone architecture

A Standalone deployment uses one Redis instance.

### Advantages

- Simple installation and configuration.
- Easy to operate and understand.
- Suitable for development, small applications, and workloads that do not require distributed redundancy.

### Disadvantages

- **Single point of failure:** If the instance fails, Redis service becomes unavailable until recovery.
- **Limited scaling:** Data and workload are constrained by the resources and capacity of the instance.
- **No replica redundancy:** Another live copy is not automatically available to take over.

### Exam scenario

**Question:** A small application needs a simple cache and does not require automatic failover. Which architecture is suitable?

**Answer:** Standalone, because it is straightforward to deploy and operate.

**Qualification:** Redis's command-processing model and threading behavior depend on the operation and Redis version. Do not interpret “single instance” as meaning every Redis operation in every version must execute on one thread.

## 6\. High Availability (HA): primary–replica architecture

A primary–replica deployment maintains one primary node and one or more replicas.

### How it works

1. The primary receives write operations.
2. Changes are propagated to replicas through replication.
3. Replicas maintain copies of the primary's dataset.
4. Reads may be served by the primary or replicas, depending on the application's configuration and consistency requirements.

### Advantages

- Provides redundant copies of data.
- Replicas can serve read traffic, reducing pressure on the primary.
- Can improve recovery options if the primary fails.

### Limitations

- Replication is generally asynchronous, so replicas can lag behind the primary.
- Reads from replicas may return stale data.
- A primary failure can interrupt writes if no failover mechanism promotes a replica.
- Replication alone does not guarantee zero downtime or zero data loss.

### Key scenario

If the primary fails in a basic primary–replica setup without automated promotion, the replicas may still contain data, but writes cannot simply continue through the failed primary.

**Exam takeaway:** Replication provides redundancy; it does not by itself provide automatic failover.

## 7\. Redis Sentinel

Redis Sentinel adds monitoring and automatic failover to a primary–replica deployment.

### Three main functions

1. **Monitoring:** Checks whether the primary and replicas are responding.
2. **Notification:** Reports failures or changes in the Redis deployment.
3. **Automatic failover:** When a primary is judged to be unavailable, Sentinel coordinates the promotion of a suitable replica to primary.

### Benefits

- Reduces dependence on manual intervention during a primary failure.
- Helps restore write availability by promoting a replica.
- Provides monitoring and notifications for operational problems.

### Important limitation

Sentinel improves availability but does not guarantee zero downtime or zero data loss. Failover takes time, and asynchronous replication can mean that some recent writes have not reached the promoted replica.

Sentinel also does not distribute the dataset across multiple shards. It manages availability rather than providing Redis Cluster-style horizontal data partitioning.

### Exam scenario

**Question:** A service must automatically promote a replica when its primary fails. Which Redis architecture is appropriate?

**Answer:** Redis Sentinel, because it monitors the deployment and coordinates automatic failover.

## 8\. Redis Cluster: sharding and hash slots

Redis Cluster distributes data across multiple nodes using **sharding**.

Instead of storing every key on one node, the cluster assigns keys to hash slots, which are distributed among the cluster's nodes.

### Hash slots

Redis Cluster has **16,384 hash slots**, numbered from 0 to 16,383.

The simplified slot calculation is:

$$
\text{HASH\_SLOT}=\operatorname{CRC16}(\text{key}) \bmod 16384
$$

The process is:

1. Redis calculates a CRC16 hash for the key.
2. The result is mapped to one of 16,384 slots using modulo 16,384.
3. The cluster routes the key's operation to the node responsible for that slot.

### Example distribution across three nodes

| Node | Example slot range | Number of slots |
| --- | --- | --- |
| Node 1 | 0–5460 | 5,461 |
| Node 2 | 5461–10922 | 5,462 |
| Node 3 | 10923–16383 | 5,461 |
| **Total** | **0–16383** | **16,384** |

This is an illustrative distribution. Real clusters can assign different slot ranges to nodes.

### Why hash slots are useful

- **Horizontal scalability:** Data can be distributed across multiple nodes.
- **Load distribution:** Different nodes serve different subsets of keys.
- **Rebalancing:** Slots can be moved between nodes when capacity or workload changes.
- **Distributed capacity:** The cluster can collectively store more data than a single instance, subject to available resources.

### Redis Cluster versus consistent hashing

The lecture emphasizes that Redis Cluster does not directly assign keys using the classic consistent-hashing ring model. It maps keys to a fixed set of 16,384 hash slots and assigns those slots to nodes.

**Exam distinction:** Hashing determines a key's slot; slot assignment determines which node owns that slot.

### Important limitation

Redis Cluster is not automatically equivalent to strong consistency or zero data loss. Replication, failover, network partitions, and acknowledged-write durability still matter.

## 9\. Redis data types: overview

Redis supports multiple data structures, each designed for different access patterns.

| Data type | Main property | Typical use |
| --- | --- | --- |
| Strings | Byte sequences; can store text, numbers, or serialized data | Cache entries, tokens, counters |
| Bitmaps | Bit-level operations over string values | Activity flags and compact binary states |
| Bitfields | Read and modify integer fields at specified bit offsets | Compact counters and packed records |
| Hashes | Field–value mappings under one key | User profiles and object attributes |
| Lists | Ordered sequences of strings; duplicates allowed | Queues, stacks, activity feeds |
| Sets | Unordered collection of unique strings | Unique users, tags, membership |
| Sorted sets | Unique members ordered by score | Leaderboards and rankings |
| Geospatial indexes | Store locations and support geographic queries | Nearby-place searches |
| HyperLogLogs | Probabilistic cardinality estimation | Approximate counts of unique visitors |
| Streams | Append-oriented entries with identifiers | Event logs and messaging pipelines |

**Exam strategy:** Learn each type's defining property and a representative use case. This is usually more useful than memorizing the entire command list.

## 10\. Strings

Redis strings store sequences of bytes. They can represent:

- Text.
- Numeric values used by supported numeric operations.
- Serialized objects.
- Binary data.

A single Redis string can hold up to **512 MB** by default.

### Typical uses

- Cache a webpage fragment or API response.
- Store a session token.
- Store a counter.
- Store serialized application data.

Example:

```
SET user:1001 "Eduardo"
GET user:1001
```

`SET` stores a value under a key; `GET` retrieves it.

**Exam takeaway:** Strings are Redis's general-purpose value type, but they are not limited to human-readable text.

## 11\. Lists

Redis lists are ordered sequences of string values, implemented as linked lists in the lecture's description.

### Characteristics

- Preserve element order.
- Allow duplicate values.
- Support operations at both ends.
- Can be used to implement queues and stacks.
- Have a theoretical maximum length of $2^{31}-1$ elements.

### Typical uses

- Background job queues.
- Stacks.
- Ordered activity timelines.
- Recent-event lists.

### Queue versus stack

- **Queue:** Typically FIFO — first in, first out.
- **Stack:** Typically LIFO — last in, first out.

Redis list operations at the left and right ends make both patterns possible.

Example:

```
LPUSH tasks "task-A"
RPUSH tasks "task-B"
LRANGE tasks 0 -1
```

This inserts items at opposite ends and retrieves the list elements in order.

**Exam takeaway:** Choose a List when ordering and duplicate elements matter.

## 12\. Sets

A Redis Set is an unordered collection of unique strings.

### Characteristics

- Members are unique; adding an existing member does not create a duplicate.
- Elements have no guaranteed order.
- Supports membership checks and set operations.
- Can be used as an index for unique membership.

The lecture gives a maximum size of $2^{32}-1$ members.

### Important set operations

- **Union:** Members appearing in either set.
- **Intersection:** Members appearing in both sets.
- **Difference:** Members in one set but not another.

### Example applications

- Unique IP addresses that visited a website.
- Users assigned to a role.
- Tags attached to an item.
- Finding users common to two groups.

Example:

```
SADD visitors "user-1" "user-2" "user-1"
SMEMBERS visitors
```

The Set contains only two unique members, because `user-1` is stored once.

**Exam takeaway:** Choose a Set when uniqueness matters and sorted order is unnecessary.

## 13\. Quick comparison: List versus Set

| Feature | List | Set |
| --- | --- | --- |
| Order | Preserved | Not guaranteed |
| Duplicates | Allowed | Not allowed |
| Main purpose | Ordered sequences | Unique membership |
| Typical examples | Queues, stacks, activity feeds | Unique visitors, group membership |
| Set operations | Not its main strength | Union, intersection, difference |

**Common exam trap:** A List and a Set can both contain strings, but their behavior differs. If duplicates must be retained in order, use a List. If members must be unique, use a Set.

## 14\. Potential exam practice questions

These are practice questions derived from the supplied lecture material, not confirmed exam questions.

### Q1. State three disadvantages of AOF.

**Answer:** AOF can consume more disk space than an RDB snapshot, take longer to replay during startup, and introduce write-related performance overhead.

**Explanation:** The impact depends on write volume, AOF rewriting, the fsync policy, and the storage hardware.

### Q2. Interpret `*3 $3 SET $5 hello $5 world`.

**Answer:** It represents a three-element array containing the command `SET`, the key `hello`, and the value `world`.

**Explanation:** `*3` specifies the number of elements, while each `$n` indicates the byte length of the next bulk-string argument.

### Q3. What is the main limitation of Standalone Redis?

**Answer:** It lacks node-level redundancy and has a single point of failure.

**Explanation:** If the instance becomes unavailable, service may stop until the instance is recovered.

### Q4. Explain the difference between primary–replica architecture and Sentinel.

**Answer:** Primary–replica architecture replicates data from the primary to replicas. Sentinel monitors that deployment and coordinates automatic failover.

**Explanation:** Replication maintains copies; Sentinel helps select a replacement primary after a failure.

### Q5. Does Redis Sentinel provide sharding?

**Answer:** No.

**Explanation:** Sentinel manages monitoring and failover for a primary–replica deployment. Redis Cluster distributes data across nodes using hash slots.

### Q6. What is Redis Cluster's hash-slot formula?

**Answer:**

$$
\text{HASH\_SLOT}=\operatorname{CRC16}(\text{key}) \bmod 16384
$$

**Explanation:** Each key maps to one of 16,384 slots, and the cluster assigns those slots to nodes.

### Q7. Why use Redis Cluster rather than Standalone Redis?

**Answer:** Redis Cluster can distribute data and workload across multiple nodes, improving horizontal scalability and supporting larger aggregate capacity.

**Explanation:** The cluster introduces additional routing, rebalancing, and operational complexity.

### Q8. Which data type is appropriate for a leaderboard?

**Answer:** Sorted Set.

**Explanation:** Sorted Sets associate unique members with scores and support score-based ordering and ranking.

### Q9. Which data type should store a unique collection of website visitors?

**Answer:** Set.

**Explanation:** Sets prevent duplicate members and support membership checks and set operations.

### Q10. Which data type is appropriate for a queue where order matters and duplicate tasks are allowed?

**Answer:** List.

**Explanation:** Lists preserve order and allow duplicate values. Push and pop operations can implement queue behavior.

### Q11. Explain the difference between a List and a Set.

**Answer:** A List is ordered and permits duplicates; a Set is unordered and contains unique members.

**Explanation:** Select the structure according to whether order and duplicate values are required.

### Q12. Why can a replica return stale data?

**Answer:** Replication may be asynchronous, so a replica may not yet have received or applied the latest write from the primary.

**Explanation:** Applications requiring the latest value must account for replica lag and their consistency requirements.

## 15\. Quick revision checklist

- [ ]Explain AOF file size, startup time, and performance disadvantages.
- [ ]Interpret the basic RESP representation of a `SET` command.
- [ ]Compare Standalone, primary–replica, Sentinel, and Cluster.
- [ ]Explain replication versus automatic failover.
- [ ]Explain sharding, hash slots, and the CRC16 formula.
- [ ]Memorize the total of 16,384 Redis Cluster hash slots.
- [ ]Identify the ten Redis data types listed in the lecture.
- [ ]Explain Strings, Lists, and Sets, including their key differences.
- [ ]Select suitable Redis data types for queues, unique membership, counters, and leaderboards.
- [ ]Explain why replication does not guarantee zero data loss.

## 16\. Scope of this compression

This document covers **Chunk 3 of 4**. It focuses on AOF disadvantages and encoding, Redis deployment architectures, Redis Cluster hash slots, and the data types introduced in the supplied material.

The supplied chunk begins describing Strings, Lists, and Sets in detail, while the other data types are primarily listed. Those remaining types are summarized by their standard defining properties and typical uses for exam revision.

**Merge instruction:** Keep this as Part 3. After Chunk 4 is compressed, combine all four parts into one consolidated revision document, remove repeated explanations, preserve distinct examinable concepts, and reconcile any overlapping definitions or examples.
