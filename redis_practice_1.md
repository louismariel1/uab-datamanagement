# Complete replacement solutions for List 1

The following six solutions are designed to replace or extend the corresponding sections of your Google Colab notebook. They use your existing `client` Redis connection and the `lab1:` key prefix, so they fit the notebook's current structure.

Each section includes executable Python code, verification outputs or checks, and the explanations required for the assignment report.

Execution note: Run the cells in order after the Redis server has started and `client` has been initialized with `decode_responses=True`. The examples use demonstration data, not real passwords or personal credentials.

## 1. Original Exercise 2 — URL counter using `INCR`

### Replacement code cell

python

```

# Exercise 2 — Count URL mappings using Redis INCR

# Reset the exercise data for a reproducible result.
client.delete(
    "lab1:count",
    "lab1:url:counter_example1",
    "lab1:url:counter_example2",
    "lab1:url:counter_example3"
)

# Initialize the counter to zero.
client.set("lab1:count", 0)

# Add three URL mappings and increment the counter after each new mapping.
urls_to_add = {
    "lab1:url:counter_example1": "https://example.com",
    "lab1:url:counter_example2": "https://www.python.org",
    "lab1:url:counter_example3": "https://redis.io"
}

for key, url in urls_to_add.items():
    # Count only newly created mappings.
    created = client.set(key, url, nx=True)

    if created:
        client.incr("lab1:count")

# Display the final counter and verify the mappings.
print("Final URL counter:", int(client.get("lab1:count")))

for key in urls_to_add:
    print(f"{key} -> {client.get(key)}")

assert int(client.get("lab1:count")) == 3
assert all(client.exists(key) for key in urls_to_add)

print("Exercise 2 verification passed.")
```

### Expected output

text

```
Final URL counter: 3
lab1:url:counter_example1 -> https://example.com
lab1:url:counter_example2 -> https://www.python.org
lab1:url:counter_example3 -> https://redis.io
Exercise 2 verification passed.
```

### Explanation for the report

The Redis String `lab1:count` stores the number of URL mappings created by this exercise. It is initialized to zero and incremented using `INCR` whenever a new mapping is successfully created.

`INCR` is an atomic Redis operation: Redis performs each increment as a single operation, so simultaneous clients cannot overwrite one another's increments in the way they can with a manual read-modify-write sequence.

For example, if two clients both read a counter value of 5, each might calculate 6 and write it back, incorrectly leaving the counter at 6. With two successful `INCR` commands, the result becomes 7.

Important limitation: The counter is accurate only if all relevant mapping creations and deletions consistently update it. `INCR` prevents lost increments, but it does not automatically keep the counter synchronized with every URL key in the database.

## 2. Original tutorial — Redis Lists

This replacement demonstrates the wishlist, stack, queue, and `RPOPLPUSH` operations. It also shows how a URL can be moved from a wishlist into a visited list.

### Replacement code cell

python

```

# Redis Lists — Wishlist, stack, queue, and moving URLs

wishlist_key = "lab1:user:list_demo:wishlist"
visited_key = "lab1:user:list_demo:visited"
stack_key = "lab1:user:list_demo:stack"
queue_key = "lab1:user:list_demo:queue"

# Reset the data for a reproducible result.
client.delete(wishlist_key, visited_key, stack_key, queue_key)

# 1. Wishlist: add URLs and inspect the list.
client.rpush(wishlist_key, "uab", "google", "github")

print("Wishlist:", client.lrange(wishlist_key, 0, -1))
print("Wishlist length:", client.llen(wishlist_key))

# 2. Stack: push and pop at the same end (LIFO).
client.lpush(stack_key, "uab")
client.lpush(stack_key, "google")
client.lpush(stack_key, "github")

print("Stack before pop:", client.lrange(stack_key, 0, -1))
print("Stack pop:", client.lpop(stack_key))
print("Stack after pop:", client.lrange(stack_key, 0, -1))

# 3. Queue: insert on the left, remove from the right (FIFO).
client.lpush(queue_key, "uab")
client.lpush(queue_key, "google")
client.lpush(queue_key, "github")

print("Queue before pop:", client.lrange(queue_key, 0, -1))
print("Queue pop:", client.rpop(queue_key))
print("Queue after pop:", client.lrange(queue_key, 0, -1))

# 4. Move the rightmost wishlist item to the left of visited.
# RPOPLPUSH performs the move as one Redis command.
client.delete(wishlist_key, visited_key)
client.rpush(wishlist_key, "uab", "google", "github")

moved_url = client.rpoplpush(wishlist_key, visited_key)

print("Moved URL:", moved_url)
print("Wishlist after move:", client.lrange(wishlist_key, 0, -1))
print("Visited after move:", client.lrange(visited_key, 0, -1))

# Verify the expected behavior.
assert client.lrange(wishlist_key, 0, -1) == ["uab", "google"]
assert client.lrange(visited_key, 0, -1) == ["github"]
assert client.llen(stack_key) == 2
assert client.llen(queue_key) == 2

print("Redis Lists verification passed.")
```

### Expected output

text

```
Wishlist: ['uab', 'google', 'github']
Wishlist length: 3
Stack before pop: ['github', 'google', 'uab']
Stack pop: github
Stack after pop: ['google', 'uab']
Queue before pop: ['github', 'google', 'uab']
Queue pop: uab
Queue after pop: ['github', 'google']
Moved URL: github
Wishlist after move: ['uab', 'google']
Visited after move: ['github']
Redis Lists verification passed.
```

### Explanation for the report

- Wishlist: `RPUSH` appends URLs to the right side of a List. `LRANGE` retrieves elements, and `LLEN` returns the List length.
- Stack (LIFO): `LPUSH` and `LPOP` operate on the same end, so the last inserted element is removed first.
- Queue (FIFO): `LPUSH` inserts elements at the left, while `RPOP` removes them from the right. The first inserted element is removed first.
- Moving between Lists: `RPOPLPUSH` removes the rightmost item from the source List and inserts it at the left of the destination List in one Redis command. It is useful when moving a URL between lists, although a real application would need to define what the move means for its workflow.

Note that `RPOPLPUSH` is deprecated in newer Redis versions in favor of `LMOVE`. The operation requested by the lab is demonstrated above.

## 3. Deliverable 1 — Complete Redis data model

The original diagram is a good starting point, but the final model should include the structures used by the activity tracker, visit history, session management, recommender, and creator leaderboard.

### Replacement Mermaid diagram

Replace the current Mermaid cell with the following diagram. It assumes the corresponding structures are implemented in the later sections of this notebook.

mermaid

```

flowchart TD
    A["URL Shortening Service"]

    A --> B["URL mappings<br/>String<br/>lab1:url:uab → https://www.uab.cat"]
    A --> C["URL counter<br/>String<br/>lab1:count → 3"]

    A --> D["User profile<br/>Hash<br/>username, display_name, age"]
    A --> E["URL categories<br/>Set<br/>news, tech, video"]

    A --> F["Vote ranking<br/>ZSET<br/>URL → vote score"]
    A --> G["Visit ranking<br/>ZSET<br/>URL → visit score"]
    F --> H["Relevance ranking<br/>ZSET<br/>URL → weighted score"]
    G --> H

    A --> I["Wishlist<br/>List<br/>user:wishlist"]
    A --> J["Visit history<br/>List or ZSET<br/>user:visited"]
    A --> K["Temporary session<br/>String + TTL<br/>session ID → token"]

    A --> L["User activity<br/>Hash<br/>posts, votes"]
    L --> M["Active-user leaderboard<br/>ZSET<br/>user ID → activity score"]
    A --> N["Duplicate-vote tracking<br/>Set<br/>user:voted → URL members"]

    A --> O["Recommendation data<br/>Sets<br/>user preferences"]
    O --> P["Similar users<br/>Set similarity calculation"]
    P --> Q["Recommended URLs<br/>Ranked candidate URLs"]

    A --> R["Creator leaderboard<br/>ZSET<br/>creator ID → score"]
```

### Data dictionary and sample data

Use the following table alongside the diagram in the report. The sample values are illustrative and do not imply that every key has already been created in the current notebook.

| Purpose | Example key | Structure | Example content |
| --- | --- | --- | --- |
| URL mapping | `lab1:url:uab` | String | `https://www.uab.cat` |
| URL counter | `lab1:count` | String | `3` |
| User profile | `lab1:user:0001` | Hash | `username=student_demo` |
| URL categories | `lab1:category:news` | Set | `nytimes.com`, `theverge.com` |
| Vote scores | `lab1:ranking:votes` | ZSET | `uab → 5` |
| Visit scores | `lab1:ranking:visits` | ZSET | `google → 1234` |
| Relevance scores | `lab1:ranking:relevance` | ZSET | `google → weighted score` |
| Wishlist | `lab1:user:0001:wishlist` | List | `uab`, `google`, `github` |
| Visit history | `lab1:user:user123:visited` | List or ZSET | URL events or URL timestamps |
| Session | `lab1:session:001` | String + TTL | Token with expiration |
| Activity statistics | `lab1:user:user123:activity` | Hash | `posts=5`, `votes=8` |
| Active-user ranking | `lab1:ranking:active_users` | ZSET | `user123 → 13` |
| Voted URLs | `lab1:user:user123:voted` | Set | `google.com` |
| User preferences | `lab1:user:user123:preferences` | Set | Preferred URL identifiers |
| Creator ranking | `lab1:ranking:creators` | ZSET | `creator001 → 250` |

### Explanation for the report

Redis uses different data structures for different access patterns. Strings store individual values such as URL mappings and counters; Hashes group related user attributes; Sets store unique category members or voted URLs; Lists maintain ordered collections; and ZSETs maintain scores and ranks. Temporary sessions use expiration times to limit their lifetime.

The active-user and creator leaderboards use ZSETs because each user or creator has a numeric score and can be retrieved in rank order. Recommendation systems can use Sets to represent user preferences and calculate similarity between users.

The model should be updated if the final implementation chooses different structures—for example, using a ZSET instead of a List for visit history when each URL must have a single latest-visit timestamp.

## 4. Deliverable 2 — Top 10 most active users

The existing implementation already uses Hashes for activity statistics, a ZSET for ranking, and Sets to prevent duplicate votes. The replacement below makes the solution more complete by supporting both posting and voting, rejecting duplicate votes, and maintaining the leaderboard.

### Activity definition

For this implementation, define a user's activity score as:

$$
\text{activity score} = \text{accepted posts} + \text{accepted votes}
$$

Each accepted post contributes one point, and each accepted vote contributes one point. A repeated vote for the same URL by the same user does not count again.

### Replacement code cell

python

```

# Deliverable 2 — Active users leaderboard

import redis

activity_leaderboard = "lab1:ranking:active_users"

def initialize_activity_user(username, posts=0, votes=0):
    """Create or reset a user's activity statistics and leaderboard score."""
    activity_key = f"lab1:user:{username}:activity"

    client.hset(
        activity_key,
        mapping={
            "posts": int(posts),
            "votes": int(votes)
        }
    )

    score = int(posts) + int(votes)
    client.zadd(activity_leaderboard, {username: score})

def update_activity_score(username):
    """Recalculate and store the leaderboard score from the activity Hash."""
    activity_key = f"lab1:user:{username}:activity"
    activity = client.hgetall(activity_key)

    posts = int(activity.get("posts", 0))
    votes = int(activity.get("votes", 0))
    score = posts + votes

    client.zadd(activity_leaderboard, {username: score})
    return score

def record_post(username, url):
    """Record a new URL post and increase the user's activity score."""
    activity_key = f"lab1:user:{username}:activity"
    posts_key = f"lab1:user:{username}:posts"

    # Prevent the same URL from being counted as a new post twice.
    created = client.sadd(posts_key, url)

    if created == 0:
        return False, update_activity_score(username)

    client.hincrby(activity_key, "posts", 1)
    return True, update_activity_score(username)

def vote_for_url(username, url):
    """Count a vote only if this user has not voted for this URL before."""
    votes_key = f"lab1:user:{username}:voted"
    activity_key = f"lab1:user:{username}:activity"

    # SADD returns 1 for a new member and 0 for a duplicate.
    added = client.sadd(votes_key, url)

    if added == 0:
        return False, update_activity_score(username)

    client.hincrby(activity_key, "votes", 1)
    return True, update_activity_score(username)

def show_top_active_users(limit=10):
    """Display the highest-scoring users."""
    results = client.zrevrange(
        activity_leaderboard, 0, limit - 1, withscores=True
    )

    for rank, (username, score) in enumerate(results, start=1):
        print(f"{rank}. {username}: {int(score)} points")

    return results

# Reset the relevant keys so this test is reproducible.
for key in client.scan_iter(match="lab1:user:activity_demo_*"):
    client.delete(key)

for key in client.scan_iter(match="lab1:user:activity_demo_*:posts"):
    client.delete(key)

for key in client.scan_iter(match="lab1:user:activity_demo_*:voted"):
    client.delete(key)

client.delete(activity_leaderboard)

# Seed ten sample users with initial activity statistics.
sample_users = [
    ("activity_demo_01", 5, 8),
    ("activity_demo_02", 7, 3),
    ("activity_demo_03", 2, 4),
    ("activity_demo_04", 4, 1),
    ("activity_demo_05", 3, 5),
    ("activity_demo_06", 8, 2),
    ("activity_demo_07", 1, 6),
    ("activity_demo_08", 6, 4),
    ("activity_demo_09", 3, 2),
    ("activity_demo_10", 2, 3),
]

for username, posts, votes in sample_users:
    initialize_activity_user(username, posts, votes)

print("Initial top 10 active users:")
show_top_active_users(10)

# Test a new post and a new vote.
print("\nRecording new activity:")
print("Post:", record_post("activity_demo_01", "new-example.org"))
print("Vote:", vote_for_url("activity_demo_02", "google.com"))

# Repeat the same vote: it must not add another activity point.
print("Duplicate vote:", vote_for_url("activity_demo_02", "google.com"))

print("\nUpdated top 10 active users:")
show_top_active_users(10)

# Verify duplicate-vote protection.
activity = client.hgetall("lab1:user:activity_demo_02:activity")
assert int(activity["votes"]) == 4

assert client.zcard(activity_leaderboard) == 10
print("\nActive-user leaderboard verification passed.")
```

### Expected behavior

- The leaderboard initially contains ten users.
- A new accepted post increments the user's `posts` count and score.
- A new accepted vote increments the user's `votes` count and score.
- Repeating the same vote leaves the vote count unchanged.
- The ZSET is updated so the user's rank can change automatically.

The exact printed ranking follows the scores and Redis's ordering rules. The test asserts the duplicate-vote behavior and the number of users rather than relying on a hard-coded ranking.

### Explanation for the report

Each user has a Hash containing their post and vote counts. A ZSET stores the total activity score and maintains the leaderboard. A separate Set records the URLs each user has voted for. Since Sets do not contain duplicate members, `SADD` returns `0` when a URL has already been voted for by that user.

After an accepted activity event, the user's statistics are updated and their leaderboard score is recalculated. `ZREVRANGE` retrieves the highest-scoring users, allowing the top ten to be obtained efficiently.

Concurrency limitation: This implementation demonstrates the required behavior in ordinary sequential use. In a production system, vote validation, activity updates, and leaderboard maintenance should be grouped in a Redis transaction or Lua script if they must remain consistent under concurrent requests. Also, a user posting the same URL is treated as a duplicate post in this example; if each posting event should count independently, use a unique post ID instead of the URL as the Set member.

## 5. Deliverable 4 — Ten most recent websites per user

The assignment requires a history of each user's ten most recent visits, sorted by visit time, with the most recent visit first. The implementation below uses a Redis List, stores new visits at the left, and trims the List to ten entries.

### Replacement code cell

python

```

# Deliverable 4 — Keep the ten most recent visit events

def record_visit(username, url):
    """
    Record a visit at the front of the user's history.
    Keep only the ten most recent visit events.
    """
    history_key = f"lab1:user:{username}:visited"

    # LPUSH places the newest event at index 0.
    client.lpush(history_key, url)

    # Keep indexes 0 through 9.
    client.ltrim(history_key, 0, 9)

def get_recent_visits(username):
    """Return up to ten visits, newest first."""
    history_key = f"lab1:user:{username}:visited"
    return client.lrange(history_key, 0, 9)

# Reset the example history.
username = "recent_demo"
history_key = f"lab1:user:{username}:visited"
client.delete(history_key)

# Record visits in chronological order.
record_visit(username, "uab")
record_visit(username, "google")
record_visit(username, "github")
record_visit(username, "youtube")

print("Most recent visits:", get_recent_visits(username))

# A repeated visit is a new visit event.
record_visit(username, "google")

print("After revisiting Google:", get_recent_visits(username))

# Add enough visits to test the ten-entry limit.
for i in range(1, 13):
    record_visit(username, f"site{i}.example")

recent = get_recent_visits(username)

print("Final history:", recent)
print("Number of stored recent visits:", len(recent))

assert len(recent) == 10
assert recent[0] == "site12.example"
assert recent[-1] == "site3.example"

print("Recent-visit history verification passed.")
```

### Expected output

text

```
Most recent visits: ['youtube', 'github', 'google', 'uab']
After revisiting Google: ['google', 'youtube', 'github', 'google', 'uab']
Final history: ['site12.example', 'site11.example', 'site10.example',
'site9.example', 'site8.example', 'site7.example', 'site6.example',
'site5.example', 'site4.example', 'site3.example']
Number of stored recent visits: 10
Recent-visit history verification passed.
```

### Explanation for the report

Each user has a List named `lab1:user:<username>:visited`. The `LPUSH` operation inserts every new visit at the beginning of the List, so the newest visit is always at index zero. `LRANGE key 0 9` retrieves the ten most recent events, and `LTRIM key 0 9` prevents the stored history from growing beyond ten events.

Repeated visits are intentionally retained as separate events. For example, visiting Google twice produces two entries because the List represents visit events rather than unique websites.

This design is appropriate when only the ten latest visit events are required. If the system must preserve precise timestamps or keep exactly one entry per website, a ZSET scored by visit timestamp would be a better alternative. With timestamp-based scoring, a revisit updates the URL's score to its latest visit time.

## 6. Deliverable 5 — Temporary sessions and TTL

This replacement creates three sessions, checks their TTLs, renews one session without modifying its value, and verifies that an expired session is no longer available. It also answers all four written questions.

### Replacement code cell

python

```

# Deliverable 5 — Session expiration and TTL

import time

session_keys = [
    "lab1:session:demo001",
    "lab1:session:demo002",
    "lab1:session:demo003"
]

# Reset existing test sessions.
client.delete(*session_keys, "lab1:session:expiry_test")

# 1. Create three sessions with different expiration times.
client.set(session_keys[0], "token_demo_001", ex=300)
client.set(session_keys[1], "token_demo_002", ex=600)
client.set(session_keys[2], "token_demo_003", ex=900)

print("Initial session TTLs:")
for key in session_keys:
    print(key, "->", client.ttl(key), "seconds")

# 2. Renew session 002 without changing its value.
session_to_renew = session_keys[1]
original_value = client.get(session_to_renew)

renewal_success = client.expire(session_to_renew, 1200)

print("\nRenewal successful:", renewal_success)
print("Value unchanged:", client.get(session_to_renew) == original_value)
print("Renewed TTL:", client.ttl(session_to_renew), "seconds")

assert renewal_success
assert client.get(session_to_renew) == original_value

# 3. Create a separate session with a short TTL for the expiry test.
client.set("lab1:session:expiry_test", "short_lived_token", ex=2)

print("\nBefore expiry:")
print("Exists:", bool(client.exists("lab1:session:expiry_test")))
print("TTL:", client.ttl("lab1:session:expiry_test"))

# Wait for the two-second TTL to expire.
time.sleep(3)

print("\nAfter expiry:")
print("Exists:", bool(client.exists("lab1:session:expiry_test")))
print("TTL:", client.ttl("lab1:session:expiry_test"))

assert client.exists(session_keys[0])
assert client.exists(session_keys[1])
assert client.exists(session_keys[2])
assert client.exists("lab1:session:expiry_test") == 0

print("\nSession TTL verification passed.")
```

### Expected behavior

The initial TTL values should be approximately 300, 600, and 900 seconds. They may be slightly lower by the time they are printed.

The renewal should report success, preserve the session's original token, and reset its expiration to approximately 1200 seconds.

After the two-second expiry test and the three-second wait, the temporary test session should no longer exist. Its `TTL` result should be `-2`, which means the key does not exist. By contrast, `TTL` returns `-1` for an existing key with no expiration.

### Explanation for the report

Redis supports expiration times through commands such as `SET` with the `EX` option and `EXPIRE`. Expiration allows Redis to remove temporary data automatically, reducing the need for manual cleanup.

- (a) Why do sessions usually expire? Sessions may contain authentication or temporary state. Expiration limits how long stale or abandoned sessions remain valid and reduces the risk associated with forgotten sessions.
- (b) When might a URL mapping expire? A mapping might be temporary when it represents a campaign link, a time-limited invitation, a preview, or another resource that should stop working after a defined period.
- (c) Which data should never expire? In a system where links are intended to remain permanent, canonical URL mappings, durable user records, and persistent ownership or audit information should not have accidental TTLs. The exact policy depends on retention requirements, privacy obligations, and whether a link is explicitly temporary.
- (d) What are the disadvantages of retaining everything indefinitely? Storage consumption increases over time, backups and maintenance become more expensive, stale information accumulates, and unnecessary retention can increase privacy and security risks.

Important: The three primary session TTLs are tested before their natural expiration. The short-lived fourth key is used only to demonstrate expiry, so the verification does not depend on waiting 15 minutes for the primary sessions to expire.

## Final integration checklist

Before exporting the notebook to PDF, make sure the following are done:

- [ ]Replace the original Exercise 2 cell with the corrected counter implementation.
- [ ]Replace the original Lists section with the queue, stack, wishlist, and `RPOPLPUSH` examples.
- [ ]Replace the original Mermaid diagram and include the data dictionary.
- [ ]Replace the Deliverable 2 implementation with the activity tracker and duplicate-vote test.
- [ ]Replace the Deliverable 4 section with the ten-most-recent-visits implementation.
- [ ]Replace the Deliverable 5 section with the three-session TTL implementation and written answers.
- [ ]Run every replacement cell in order and capture the actual outputs.
- [ ]Check that the final notebook remains consistent with the eight-page report limit.

One small portability adjustment for the TTL cell: if your Python environment rejects the `client.delete(*session_keys, "lab1:session:expiry_test")` call, use this equivalent form:

python

```
client.delete(*(session_keys + ["lab1:session:expiry_test"]))
```

The six replacement sections now cover the missing functionality and explanations identified in List 1. The recommender system, atomic-counter written task, and creator leaderboard from List 2 remain separate tasks for the next stage.
