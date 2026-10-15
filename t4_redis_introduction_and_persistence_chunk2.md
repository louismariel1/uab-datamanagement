# T4 — Redis Introduction and Persistence (Chunk 2)

## Exam Compression Notes

### 1\. Introduction to Redis

**Redis** is an in-memory data store used as a database, cache, message broker, and streaming platform.

- **Storage:** Primarily stores data in RAM → very fast access and low latency.
- **Use cases:** Caching, real-time applications, real-time analytics, messaging.
- **Persistence:** Supports RDB snapshots and AOF operation logs to save data beyond memory.
- **Cache architecture:** Redis can sit in front of a persistent database such as MySQL or PostgreSQL to reduce database load.

**Exam key:** Redis provides speed through memory and persistence through RDB/AOF.

### 2\. How Redis Works

#### Redis Read

1. Check Redis for the requested key.
2. **Cache hit:** Return the cached value.
3. **Cache miss:** Retrieve the value from the persistent database.
4. Return the value and populate the cache.

**Cache hit** = data exists in cache.

**Cache miss** = data is absent from cache and must be retrieved elsewhere.

#### Redis Write (Simplified Flow)

1. Check whether the key exists in the cache.
2. Update the cached value if necessary, or insert it if absent.
3. Insert or update the persistent database.

**Exam key:** Reads benefit from cache hits; misses require a database lookup. Write consistency depends on the actual write strategy and operation order.

### 3\. RDB — Redis Database File

**Definition:** RDB persistence saves point-in-time snapshots of the Redis dataset.

#### How RDB Works

1. Redis takes snapshots at configured intervals or when triggered manually.
2. Snapshots are stored in a compact binary file.
3. Redis can reload a snapshot to restore the dataset.

#### Advantages

- **Compactness:** Efficient storage, often with a smaller backup file.
- **Performance:** Snapshot generation occurs in the background, although forking can temporarily affect performance.
- **Faster restarts:** Loads a snapshot rather than replaying a long sequence of operations.
- **Backups:** Suitable for periodic backups.

#### Disadvantages

- **Potential data loss:** Changes made after the last successful snapshot may be lost.
- **Less granular recovery:** Stores snapshots rather than a complete history of write operations.
- **Recovery limitations:** May be unsuitable when losing recent transactions is unacceptable.

**Memory trick:** RDB = _Regular snapshots → Rapid restart → Risk of losing recent changes._

### 4\. AOF — Append Only File

**Definition:** AOF persistence records write operations in a log so Redis can reconstruct its dataset by replaying the recorded commands.

#### How AOF Works

1. A write operation is recorded in the AOF log.
2. The log grows as operations are recorded.
3. During recovery, Redis replays the logged commands to rebuild the dataset.

#### Advantages

- **Durability:** Generally offers finer-grained recovery than periodic snapshots.
- **Reduced data-loss window:** Depends on the configured fsync policy.
- **Operation history:** Records write commands rather than only periodic dataset snapshots.

#### Disadvantages

- **File growth:** The log can grow over time.
- **Recovery time:** Replaying operations may take longer than loading an RDB snapshot.
- **Durability is configurable:** Unsynced recent writes can still be lost during a failure.

**Memory trick:** AOF = _Append operations → Replay commands → Better-grained recovery._

### 5\. RDB vs. AOF — Must Memorize

| Feature | RDB | AOF |
| --- | --- | --- |
| Full name | Redis Database File | Append Only File |
| Method | Point-in-time snapshots | Logs write operations |
| Data representation | Binary snapshot | Recorded commands |
| Recovery | Load snapshot | Replay commands |
| Data-loss risk | Changes since last snapshot | Depends on fsync policy |
| Restart speed | Generally faster | May require more replay time |
| File characteristics | Usually compact | Can grow over time |
| Main strength | Backups and fast restoration | More granular recovery |

**Exam conclusion:** RDB is generally preferred for compact snapshots and fast restarts; AOF is generally preferred when finer-grained persistence is important. Redis can use both mechanisms together.

### 6\. Snapshot Timeline

Example: snapshots occur at `t1`, `t2`, and `t3`.

`Snapshot t1 ───── Snapshot t2 ───── Snapshot t3`

If Redis fails after `t2` but before `t3`, changes made after `t2` may be absent from the latest usable RDB snapshot.

**Key concept:** The interval between the last usable snapshot and the failure represents the potential RDB data-loss window.

### 7\. Likely Exam Questions

**Q1. Why is Redis fast?**

Because it primarily stores data in RAM, allowing very low-latency access.

**Q2. What is a cache hit and a cache miss?**

- Hit: Requested data exists in the cache.
- Miss: Requested data is absent and must be retrieved from another source.

**Q3. Explain RDB persistence.**

RDB periodically or manually saves the dataset as a binary snapshot, allowing Redis to restore the state represented by that snapshot.

**Q4. State two advantages and two disadvantages of RDB.**

- Advantages: Compact backups and generally faster restarts.
- Disadvantages: Potential loss of changes after the last snapshot and less granular recovery.

**Q5. Explain AOF persistence.**

AOF records write operations in a log. Redis can replay these commands to reconstruct the dataset.

**Q6. Why can AOF be more durable than RDB?**

Because it records write operations between snapshots, reducing the potential loss of recent changes. The actual loss depends on the AOF fsync configuration.

**Q7. Compare RDB and AOF.**

RDB stores snapshots and is generally compact and fast to load. AOF records write operations and provides more granular recovery, potentially at the cost of larger logs and longer recovery.

**Q8. Why use Redis in front of MySQL or PostgreSQL?**

To serve frequently requested data from memory, reduce load on the persistent database, and improve application response times.

### 8\. Last-Minute Revision

- **Redis:** In-memory data store; low latency.
- **Cache hit:** Return cached data.
- **Cache miss:** Fetch from the persistent database, then populate the cache.
- **RDB:** Snapshot → compact backup → fast restart → possible loss of recent changes.
- **AOF:** Write log → command replay → finer-grained recovery → possible replay overhead.
- **Persistence choice:** RDB, AOF, or both, depending on recovery and durability requirements.
- **Critical distinction:** RDB records dataset states; AOF records write operations.

To save it under the requested filename, copy the Markdown content into a text editor and save it as `t4_redis_introduction_and_persistence_chunk2.md`.
