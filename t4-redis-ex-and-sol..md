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
