Below is an initial report draft for the Redis lab, structured to fit the assignment's eight-page limit when formatted in 11-point font with single spacing. It documents the objectives, data model, implementation choices, and solutions, leaving clearly marked spaces for actual Colab outputs, screenshots, and the rendered diagram.

The report is intended to be extended after you execute and verify the complete notebook. It does not claim that screenshots or runtime results have already been collected.

Writing

# Artificial Intelligence Degree — Course 2026–2027

## Data Management — Lab 1: Redis

**Department:** Computer Architecture and Operating Systems (CAOS), UAB\
**Student(s):** \[Insert name(s)\]\
**Date:** \[Insert submission date\]

## 1\. Introduction and objectives

Redis is an in-memory data store that supports several data structures, including Strings, Hashes, Sets, Lists, and Sorted Sets (ZSETs). Its operations are designed for low-latency data access and applications such as caching, session management, URL shortening, activity tracking, recommendation systems, and leaderboards.

The objective of this laboratory is to implement a Redis-backed URL-shortening service using Python. The exercises explore Redis data structures and operations, followed by applications involving user activity, recommendations, recent-visit histories, temporary sessions, atomic counters, and rankings.

The implementation uses Google Colab, a locally running Redis server, and the Python `redis` client library. All laboratory keys use the `lab1:` prefix to separate the lab's data from unrelated Redis keys.

## 2\. Environment configuration

Redis was installed and started in the Colab environment, and the Python client was installed using `pip`. The client connects to the local server on port 6379.

python

```
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)

print(client.ping())
```

The `decode_responses=True` option makes Redis return strings rather than byte sequences, simplifying the processing of returned values.

Before the exercises, the notebook removes existing keys belonging to the `lab1:` namespace. This provides a clean workspace for repeatable tests.

**Evidence to add:** Insert a screenshot showing successful Redis startup and the output of `client.ping()`.

## 3\. Original exercises: Redis data structures

### 3.1 Exercise 1 — String URL mappings

Redis Strings are used to associate short identifiers with original URLs. Individual mappings are created with `SET`, while multiple mappings can be inserted using `MSET`. Values are retrieved with `GET` and `MGET`.

python

```
client.set("lab1:url:uab", "https://www.uab.cat")
client.set("lab1:url:google", "https://www.google.com")
client.set("lab1:url:youtube", "https://www.youtube.com")

more_urls = {
    "lab1:url:github": "https://github.com",
    "lab1:url:wikipedia": "https://www.wikipedia.org",
    "lab1:url:stackoverflow": "https://stackoverflow.com"
}

client.mset(more_urls)
print(client.mget(list(more_urls.keys())))
```

This exercise demonstrates the storage and retrieval of URL mappings using individual and multi-key operations.

### 3.2 Exercise 2 — Atomic URL counter

A String named `lab1:count` stores the number of URLs added during the exercise. The counter is initialized to zero and incremented whenever a new mapping is created.

python

```
client.set("lab1:count", 0)

urls = {
    "lab1:url:example1": "https://example.com",
    "lab1:url:python": "https://www.python.org",
    "lab1:url:redis": "https://redis.io"
}

for key, url in urls.items():
    client.set(key, url)
    client.incr("lab1:count")

print("URL counter:", client.get("lab1:count"))
```

The expected counter value is three when the cell runs successfully in a clean workspace.

`INCR` is safer than manually reading, incrementing, and writing a counter because the increment is an atomic Redis operation. Concurrent increments are serialized by Redis, preventing the lost-update problem that can occur with separate read and write commands.

For a production URL service, the mapping creation and counter increment should also be coordinated if the counter must count only successfully created, unique mappings.

### 3.3 Exercise 3 — User profiles with Hashes

Redis Hashes store multiple fields under one key. Each user profile can contain attributes such as a username, display name, age, and other application-specific information.

python

```
client.hset(
    "lab1:user:0001",
    mapping={
        "username": "student_demo",
        "display_name": "Student Demo",
        "age": "21"
    }
)

print(client.hgetall("lab1:user:0001"))
```

The `HSET` command supports inserting multiple field-value pairs in a single operation. `HGETALL` retrieves the complete profile.

The notebook also demonstrates a second profile with a password field. In a real authentication system, passwords must never be stored in plaintext; a suitable password-hashing algorithm should be used instead.

### 3.4 Exercise 4 — URL categories with Sets

Redis Sets store unique members and support operations such as union and intersection. They are appropriate for classifying websites into categories without duplicating the same member within a category.

python

```
client.sadd(
    "lab1:category:news",
    "nytimes.com", "theverge.com"
)

client.sadd(
    "lab1:category:tech",
    "theverge.com", "cnet.com"
)

client.sadd(
    "lab1:category:video",
    "youtube.com", "vimeo.com", "netflix.com"
)

client.sunionstore(
    "lab1:category:all",
    "lab1:category:news",
    "lab1:category:tech",
    "lab1:category:video"
)

client.sinterstore(
    "lab1:category:news-tech",
    "lab1:category:news",
    "lab1:category:tech"
)
```

The union contains all distinct websites across the three categories. The intersection contains websites that belong to both `news` and `tech`; in this example, that includes `theverge.com`.

### 3.5 Exercises 5 and 6 — Sorted Sets and rankings

A Redis Sorted Set associates each unique member with a numeric score. Members are ordered by score, making this structure suitable for popularity rankings.

python

```
client.zadd("lab1:ranking:votes", {
    "uab": 5,
    "google": 1,
    "bing": 10,
    "github": 7,
    "youtube": 12
})

client.zadd("lab1:ranking:votes", {"special": 9999})

print(client.zrevrange(
    "lab1:ranking:votes", 0, 4, withscores=True
))
```

`ZSCORE` retrieves an individual score, `ZCARD` returns the number of members, and `ZREVRANGE` retrieves members in descending score order. The item with score 9999 should appear first in the top-five result.

### 3.6 Exercise 7 — Combined relevance ranking

The relevance ranking combines votes and visits into a single score. Votes receive twice the weight of visits.

python

```
client.zadd("lab1:ranking:votes", {
    "uab": 5,
    "google": 1,
    "bing": 10
})

client.zadd("lab1:ranking:visits", {
    "uab": 20,
    "google": 1234,
    "github": 30
})

client.zunionstore(
    "lab1:ranking:relevance",
    ["lab1:ranking:votes", "lab1:ranking:visits"],
    weights=[2, 1],
    aggregate="SUM"
)

print(client.zrevrange(
    "lab1:ranking:relevance", 0, -1, withscores=True
))
```

For each website, the resulting score is:

**Relevance = 2 × votes + visits.**

Members present in only one input Sorted Set are also included. The aggregation is performed by Redis, avoiding the need to retrieve and combine the rankings in Python.

### 3.7 Exercise 8 — Lists and visit history

Redis Lists maintain element order and allow insertion and removal at either end. They can implement queues, stacks, wishlists, and recent-visit histories.

python

```
wishlist_key = "lab1:user:0001:wishlist"
visited_key = "lab1:user:0001:visited"

client.rpush(wishlist_key, "google", "bing", "github")
client.lpush(visited_key, "youtube")

print(client.lrange(wishlist_key, 0, -1))
print(client.llen(wishlist_key))
```

`RPUSH` inserts at the right, `LPUSH` inserts at the left, `LRANGE` retrieves a range, and `LLEN` returns the length. Depending on the insertion and removal ends, a List can behave as a queue or a stack.

The implementation also demonstrates moving an element between Lists using `RPOPLPUSH`, or its modern replacement, `LMOVE`.

## 4\. Complete Redis data model

The service combines multiple Redis structures, each chosen for a particular access pattern.

| Entity | Example key | Redis structure | Purpose |
| --- | --- | --- | --- |
| URL mapping | `lab1:url:uab` | String | Stores the original URL |
| URL counter | `lab1:count` | String | Counts URL additions |
| User profile | `lab1:user:0001` | Hash | Stores profile attributes |
| URL category | `lab1:category:news` | Set | Stores unique category members |
| Vote ranking | `lab1:ranking:votes` | ZSET | Ranks URLs by votes |
| Visit ranking | `lab1:ranking:visits` | ZSET | Ranks URLs by visits |
| Relevance ranking | `lab1:ranking:relevance` | ZSET | Combines ranking scores |
| Wishlist | `lab1:user:0001:wishlist` | List | Stores ordered saved URLs |
| Visit history | `lab1:user:user123:visited` | List | Stores recent visit events |
| User preferences | `lab1:user:<username>:preferences` | Set | Stores preferred URLs |
| User activity | `lab1:user:<username>:activity` | Hash | Stores post and vote counts |
| Voted URLs | `lab1:user:<username>:voted` | Set | Prevents duplicate votes |
| Active-user leaderboard | `lab1:ranking:active_users` | ZSET | Ranks users by activity |
| Creator leaderboard | `lab1:ranking:creators` | ZSET | Ranks content creators |
| Temporary session | `lab1:session:<id>` | String with TTL | Stores an expiring session token |

**\[Insert the rendered Mermaid data-model diagram here.\]**

The diagram should show the URL service, its data structures, and the relationships between votes, visits, relevance, user activity, and recommendation data.

## 5\. Deliverable 2 — Top 10 most active users

### 5.1 Activity definition

A user's activity score is defined as the sum of accepted posts and accepted votes:

$$
A(u)=P(u)+V(u)
$$

where $P(u)$ is the number of posts and $V(u)$ is the number of accepted votes.

Each user has an activity Hash containing these counters. A ZSET stores the user's total activity score, allowing the ten highest-scoring users to be retrieved using `ZREVRANGE`.

### 5.2 Updating activity

When a user casts a vote, the system first inserts the URL into the user's voted-URL Set using `SADD`. Redis returns one if the member is new and zero if it already exists. Only a new vote should increment the vote counter and update the leaderboard.

A new URL post similarly increases the post count and recalculates the user's activity score. The ZSET is updated immediately so subsequent leaderboard queries use the latest recorded score.

The design demonstrates duplicate-vote prevention through Set membership. However, a production implementation should use a Lua script or another suitable atomic mechanism to ensure that vote registration, counter updates, and leaderboard changes remain consistent under concurrent requests.

**\[Insert a screenshot of the initial and updated active-user leaderboard, including the duplicate-vote test.\]**

## 6\. Deliverable 3 — Simple recommender system

The recommender uses collaborative filtering based on user preferences. Each user has a Set containing their preferred URLs.

The similarity between users $A$ and $B$ is measured using Jaccard similarity:

$$
J(A,B)=\frac{|A\cap B|}{|A\cup B|}
$$

The system compares a target user's preferences with those of other users and ranks users with the highest positive similarity. URLs preferred by similar users but not already selected by the target user become recommendation candidates.

Each candidate receives a score equal to the sum of the similarities of the users who prefer it. The system recommends the highest-scoring candidates.

This approach illustrates how Redis Sets support preference storage, intersection, union, and duplicate elimination. Its limitations include sparse preference data and the absence of recommendations when no similar users can be found. A production system could introduce a fallback to globally popular or category-relevant URLs.

**\[Insert a screenshot showing the target user's preferences, similar users, similarity scores, and recommendations.\]**

## 7\. Deliverable 4 — Ten most recent visits

Each user has a visit-history List. A new visit is inserted at the left using `LPUSH`, making index zero the most recent event.

The system retrieves the ten latest visits with `LRANGE key 0 9` and uses `LTRIM key 0 9` to retain only the ten most recent entries.

Repeated visits are preserved as separate events, which accurately reflects a visit history. If a user visits Google again, another Google entry is inserted at the beginning.

This design provides constant-time insertion at the beginning of the List and avoids storing an unbounded history when only ten entries are required.

**\[Insert a screenshot demonstrating a new visit, a repeated visit, and the final ten-entry history.\]**

## 8\. Deliverable 5 — Temporary sessions and TTL

Redis supports expiration times for keys. A session can be created with a TTL using `SET` and the `ex` parameter.

python

```
client.set(
    "lab1:session:demo001",
    "token_demo_001",
    ex=300
)
```

The remaining lifetime is obtained using `TTL`. The `EXPIRE` command renews the expiration time without changing the stored value. An expired key is automatically removed by Redis and is no longer returned by ordinary key lookups.

Session expiration limits the lifetime of stale authentication state and reduces storage consumption. URL mappings may also expire when they are intended for temporary campaigns or time-limited links.

Permanent URL mappings and essential user records should not expire accidentally. Nevertheless, durable data must still be governed by the application's retention and deletion policies.

**\[Insert a screenshot showing three session TTLs, renewal of one session, and successful expiration of a short-lived test key.\]**

## 9\. Deliverable 6 — Atomic counters

A manual counter update can lose increments if two clients read the same value before either writes its result. For example, both clients may read five, calculate six, and write six, even though two increments were requested.

Redis `INCR` avoids this problem because each increment is atomic. Starting at five, two successful increments result in seven.

This atomicity applies to the increment command itself. If the counter must be updated together with another operation, such as creating a URL mapping, the combined workflow needs an appropriate transaction or Lua script.

**\[Insert a screenshot comparing the simulated lost update with the atomic INCR result.\]**

## 10\. Deliverable 7 — Content creator leaderboard

A ZSET named `lab1:ranking:creators` stores creator identifiers and their performance scores. The implementation inserts 65 creators, satisfying the requirement to use more than 60 members.

`ZINCRBY` increases a creator's score. `ZREVRANGE` retrieves the top ten creators, and `ZREVRANK` obtains a creator's descending rank. Since Redis uses zero-based indexes, one must add one to the returned rank to obtain a human-readable position.

To retrieve positions 20 through 50, the query uses indexes 19 through 49. This returns 31 creators when the leaderboard contains at least 50 members.

Sorted Sets are appropriate because they combine unique members, numeric scores, and efficient ranking and range operations. Equal scores are ordered according to Redis's tie-breaking rules, which should be considered when presenting the results.

**\[Insert a screenshot showing the number of creators, the top ten, a creator's rank, and positions 20–50.\]**

## 11\. Conclusions

This laboratory demonstrates how Redis data structures can be combined to implement a low-latency URL-shortening service and several associated features.

Strings support URL mappings, counters, and temporary session values. Hashes organize user attributes and activity counters. Sets provide unique membership and support category operations and duplicate-vote prevention. Lists maintain ordered wishlists and recent-visit events. Sorted Sets support popularity rankings, activity leaderboards, relevance calculations, and creator rankings.

The additional applications demonstrate how these structures can be combined to implement a basic collaborative filtering recommender and real-time rankings. The exercises also illustrate the importance of atomic operations, appropriate key naming, expiration policies, and verification through actual Redis outputs.

The implementation provides a foundation for more advanced systems, while production deployments would require additional attention to concurrency, authentication, data retention, scalability, and error handling.

## Appendix — Execution evidence and final checks

Before submission, execute the notebook from a clean Colab runtime and verify that all cells complete successfully.

Include representative evidence for:

1. Redis installation and connection.
2. String, Hash, Set, List, and ZSET operations.
3. The complete rendered data-model diagram.
4. The active-user leaderboard and duplicate-vote behavior.
5. The recommender's similarity scores and recommendations.
6. Recent-visit ordering and the ten-entry limit.
7. Session expiration and renewal.
8. Atomic counter behavior.
9. The creator leaderboard with 65 members.

The final PDF should include actual results rather than illustrative outputs, use the required 11-point font and single spacing, and remain within eight A4 pages. Screenshots should be selective and readable, with no more than two or three per exercise.

## Recommended next step

To turn this draft into the final submission, I recommend extending it in this order:

1. Execute the notebook from a fresh Colab runtime and save the actual outputs.
2. Insert the rendered Mermaid diagram and the most useful screenshots.
3. Verify the numerical results, particularly the relevance scores and creator ranking.
4. Condense the report to eight pages, preserving the data model, key code examples, explanations, and evidence.

One technical detail worth checking before finalizing: the earlier creator-leaderboard example updates `creator_001` from 10 to 35, which changes the ordering of the lower-ranked creators. The rank assertions and displayed positions should be based on the actual post-update scores, not the original ordering.
