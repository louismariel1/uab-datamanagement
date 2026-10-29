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

# Complete solutions for List 2 — Unresolved tasks

The following three sections provide replacement-ready solutions for the remaining tasks in your Google Colab Redis notebook:

1. Deliverable 3: Simple recommender system.
2. Deliverable 6: Atomic counter and concurrent updates.
3. Deliverable 7: Content creator leaderboard.

Each solution includes executable Python code, verification checks, sample outputs or expected behavior, and a concise explanation suitable for the final report.

Before running: These examples reuse your existing `client` Redis connection, with `decode_responses=True` and the `lab1:` key prefix. The code uses demonstration users and URLs. It resets only the specifically named demo keys, not your entire Redis database.

## 1. Deliverable 3 — Simple recommender system

### Design

The recommender will use Redis Sets to represent each user's preferred URLs.

- Each user has a preference Set, such as `lab1:user:rec_demo_01:preferences`.
- Similarity between two users is measured using Jaccard similarity:

$$
J(A,B)=\frac{|A\cap B|}{|A\cup B|}
$$

Here, $A$ and $B$ are the users' preferred-URL Sets. A score of 1 means identical preference Sets; a score of 0 means no shared URLs when the union is non-empty.

- Recommendations are URLs liked by similar users but not already present in the target user's preferences.
- Candidate URLs are ranked by how many similar users prefer them, with a higher similarity score contributing more weight.

This is a simple user-based collaborative filtering approach. It is suitable for demonstrating the required Redis operations without introducing an external machine-learning library.

### Replacement code cell

python

```

# Deliverable 3 — Redis-based recommender system

from collections import defaultdict

def preference_key(username):
    return f"lab1:user:{username}:preferences"

def set_user_preferences(username, urls):
    """Replace a demo user's preference Set."""
    key = preference_key(username)
    client.delete(key)

    if urls:
        client.sadd(key, *urls)

def get_user_preferences(username):
    return client.smembers(preference_key(username))

def jaccard_similarity(user_a, user_b):
    """Calculate Jaccard similarity between two users' preference Sets."""
    prefs_a = get_user_preferences(user_a)
    prefs_b = get_user_preferences(user_b)

    union = prefs_a | prefs_b

    if not union:
        return 0.0

    intersection = prefs_a & prefs_b
    return len(intersection) / len(union)

def find_similar_users(target_user, min_similarity=0.0):
    """Return other demo users ranked by preference similarity."""
    users = [
        "rec_demo_01",
        "rec_demo_02",
        "rec_demo_03",
        "rec_demo_04",
        "rec_demo_05"
    ]

    results = []

    for other_user in users:
        if other_user == target_user:
            continue

        score = jaccard_similarity(target_user, other_user)

        if score > 0 and score >= min_similarity:
            results.append((other_user, score))

    return sorted(results, key=lambda item: (-item[1], item[0]))

def recommend_urls(target_user, top_n=5):
    """
    Recommend URLs not already preferred by the target user.
    Similar users contribute according to their Jaccard similarity.
    """
    target_preferences = get_user_preferences(target_user)
    similar_users = find_similar_users(target_user)

    candidate_scores = defaultdict(float)

    for similar_user, similarity in similar_users:
        their_preferences = get_user_preferences(similar_user)

        for url in their_preferences - target_preferences:
            candidate_scores[url] += similarity

    return sorted(
        candidate_scores.items(),
        key=lambda item: (-item[1], item[0])
    )[:top_n]

# Seed reproducible demonstration preferences.
demo_preferences = {
    "rec_demo_01": {"google.com", "github.com", "python.org"},
    "rec_demo_02": {"google.com", "github.com", "stackoverflow.com"},
    "rec_demo_03": {"google.com", "github.com", "youtube.com"},
    "rec_demo_04": {"nytimes.com", "bbc.com"},
    "rec_demo_05": {"python.org", "github.com", "stackoverflow.com"}
}

for username, urls in demo_preferences.items():
    set_user_preferences(username, urls)

target = "rec_demo_01"

print("Target preferences:", sorted(get_user_preferences(target)))

print("\nSimilar users:")
for username, score in find_similar_users(target):
    print(f"{username}: similarity={score:.3f}")

print("\nRecommended URLs:")
recommendations = recommend_urls(target, top_n=5)

for url, score in recommendations:
    print(f"{url}: recommendation_score={score:.3f}")

# Verification: recommendations must not already be in target preferences.
target_preferences = get_user_preferences(target)
assert all(url not in target_preferences for url, _ in recommendations)

# The closest seeded users share two of the target user's three preferences.
similarities = dict(find_similar_users(target))
assert abs(similarities["rec_demo_02"] - 0.5) < 1e-9
assert abs(similarities["rec_demo_03"] - 0.5) < 1e-9

print("\nRecommender verification passed.")
```

### Expected results

The target user `rec_demo_01` has the preferences:

text

```
google.com
github.com
python.org
```

Users `rec_demo_02` and `rec_demo_03` each share two of these three preferences and have a Jaccard similarity of $2/4=0.5$.

The recommendations include URLs such as `stackoverflow.com` and `youtube.com`, which the target user has not already selected. The ranking is computed from the actual similarity scores and preference Sets.

The exact printed list follows the implemented scoring rule. The verification checks that the two expected similarity scores are correct and that no recommended URL is already in the target user's preference Set.

### Explanation for the report

Redis Sets are appropriate for storing preferences because they automatically prevent duplicate URLs. `SMEMBERS` retrieves the preferences, while Python Set operations calculate the intersection and union needed for Jaccard similarity.

The recommender identifies similar users, finds URLs those users prefer that the target user has not selected, and ranks the candidates using similarity-weighted scores. This is a basic collaborative filtering model; a production system could incorporate URL categories, popularity, recency, and explicit negative feedback.

Limitation: The example searches a fixed collection of five demonstration users. A larger system should maintain a user index or another way to discover all users with preference data, and it should consider how to scale similarity calculations as the number of users grows.

## 2. Deliverable 6 — Atomic counter and concurrent updates

The original Exercise 2 already uses `INCR`. This deliverable requires explaining and demonstrating why atomic increment is safer than manually reading, modifying, and writing a counter.

### Replacement code cell

python

```

# Deliverable 6 — Demonstrate a lost update and atomic INCR

counter_key = "lab1:counter:concurrency_demo"

# Part A: Simulate two clients manually updating the same counter.
client.set(counter_key, 5)

# Both clients read the same initial value before either writes.
client_a_value = int(client.get(counter_key))
client_b_value = int(client.get(counter_key))

# Each client calculates its new value independently.
client_a_new_value = client_a_value + 1
client_b_new_value = client_b_value + 1

# Client A writes its result.
client.set(counter_key, client_a_new_value)

# Client B writes its stale result, overwriting Client A's update.
client.set(counter_key, client_b_new_value)

manual_result = int(client.get(counter_key))

print("Initial counter: 5")
print("Client A read:", client_a_value)
print("Client B read:", client_b_value)
print("Result after manual updates:", manual_result)
print("Expected result after two increments: 7")

assert manual_result == 6

# Part B: Use atomic Redis INCR instead.
client.set(counter_key, 5)

result_a = client.incr(counter_key)
result_b = client.incr(counter_key)

atomic_result = int(client.get(counter_key))

print("\nResult after two INCR operations:", atomic_result)
print("First INCR returned:", result_a)
print("Second INCR returned:", result_b)

assert atomic_result == 7
assert result_a == 6
assert result_b == 7

print("Atomic counter verification passed.")
```

### Expected output

text

```
Initial counter: 5
Client A read: 5
Client B read: 5
Result after manual updates: 6
Expected result after two increments: 7

Result after two INCR operations: 7
First INCR returned: 6
Second INCR returned: 7
Atomic counter verification passed.
```

### Explanation for the report

A manual counter update consists of multiple operations: read the current value, calculate the next value, and write it back. If two clients execute these steps concurrently, both may read the same initial value and then overwrite one another's results. This is known as a lost update.

In the example, both clients read 5. Each calculates 6, so the final value is 6 instead of the expected 7.

Redis `INCR` performs the increment atomically as one server-side operation. When two increments execute, Redis applies both operations, producing 7. Each client receives the value resulting from its own increment.

The demonstration simulates the problematic interleaving deterministically; it does not depend on a race occurring by chance.

Conclusion: Use `INCR` instead of a manual read-modify-write sequence when multiple clients may update a counter. Note that this guarantees atomicity of the increment itself, not atomic consistency between a counter and unrelated URL operations. If the counter must exactly reflect URL creation or deletion under concurrent requests, those operations need an appropriate transactional or scripted design.

## 3. Deliverable 7 — Content creator leaderboard

### Design

A Redis Sorted Set (ZSET) is a natural fit for a leaderboard because each creator has a numeric score, and Redis maintains the members in score order.

The implementation will:

- Insert 65 content creators with initial scores.
- Increase a creator's score.
- Retrieve the top 10 creators in descending score order.
- Find the rank of a particular creator.
- Display creators ranked from position 20 through position 50, inclusive.
- Verify that the results satisfy the assignment requirements.

### Replacement code cell

python

```

# Deliverable 7 — Content creator leaderboard using a Redis ZSET

leaderboard_key = "lab1:ranking:creators"

# Reset only the creator leaderboard used by this exercise.
client.delete(leaderboard_key)

# Create 65 creators with reproducible initial scores.
creator_scores = {
    f"creator_{i:03d}": i * 10
    for i in range(1, 66)
}

client.zadd(leaderboard_key, creator_scores)

print("Number of creators:", client.zcard(leaderboard_key))

# 1. Update a creator's score.
creator_to_update = "creator_001"

updated_score = client.zincrby(
    leaderboard_key,
    25,
    creator_to_update
)

print(
    f"Updated score for {creator_to_update}:",
    int(updated_score)
)

# 2. Retrieve the top 10 creators.
top10 = client.zrevrange(
    leaderboard_key,
    0,
    9,
    withscores=True
)

print("\nTop 10 content creators:")
for position, (creator, score) in enumerate(top10, start=1):
    print(f"{position}. {creator}: {int(score)} points")

# 3. Find the rank of a specific creator.
target_creator = "creator_040"

rank_zero_based = client.zrevrank(
    leaderboard_key,
    target_creator
)

if rank_zero_based is not None:
    print(
        f"\nRank of {target_creator}:",
        rank_zero_based + 1
    )
else:
    print(f"\n{target_creator} is not in the leaderboard.")

# 4. Display positions 20 through 50, inclusive.
# Redis indexes start at zero, so use indexes 19 through 49.
positions_20_to_50 = client.zrevrange(
    leaderboard_key,
    19,
    49,
    withscores=True
)

print("\nCreators ranked from position 20 to position 50:")
for position, (creator, score) in enumerate(
    positions_20_to_50,
    start=20
):
    print(f"{position}. {creator}: {int(score)} points")

# 5. Verify all assignment requirements.
assert client.zcard(leaderboard_key) == 65
assert len(top10) == 10
assert len(positions_20_to_50) == 31
assert rank_zero_based is not None
assert int(client.zscore(leaderboard_key, creator_to_update)) == 35

# Check that the top ten are ordered from highest to lowest score.
top_scores = [score for _, score in top10]
assert top_scores == sorted(top_scores, reverse=True)

print("\nCreator leaderboard verification passed.")
```

### Expected results

65 creators inserted

The dataset exceeds the requirement of 60 users.

Top 10 displayed

The highest scores appear first.

Ranks 20–50 displayed

31 entries, using the correct zero-based indexes.

With the initial scores, `creator_065` has 650 points and is initially the highest-scoring creator. `creator_001` starts with 10 points and receives an additional 25, giving it 35 points.

After the update, the code prints the actual ranking and the rank of `creator_040`. The exact rank should be taken from the notebook's output because scores may change as you test the leaderboard.

### Explanation for the report

A ZSET stores each creator as a unique member associated with a numeric score. The score can represent views, likes, followers, or a weighted combination of engagement metrics.

- `ZADD` inserts creators and assigns their initial scores.
- `ZINCRBY` increases a creator's score without manually reading and rewriting it.
- `ZREVRANGE` retrieves members from highest to lowest score.
- `ZREVRANK` obtains a creator's zero-based rank in descending order. Adding one converts it to a human-readable position.
- `ZCARD` returns the total number of creators.

A ZSET is suitable because it maintains an ordered ranking, supports efficient score updates, and provides rank and range queries without requiring the application to sort every creator manually after each update.

When two creators have equal scores, Redis uses member ordering to break the tie. If the application requires a specific tie-breaking rule, such as the earliest achievement time, that rule must be implemented explicitly.

## Final checklist for List 2

| Task | Solution provided | Main verification |
| --- | --- | --- |
| Deliverable 3 — Recommender | Yes | Similarity scores and exclusion of already-preferred URLs |
| Deliverable 6 — Atomic counter | Yes | Manual update produces 6; two `INCR` operations produce 7 |
| Deliverable 7 — Creator leaderboard | Yes | 65 creators, top 10, specific rank, positions 20–50 |

Before submitting, run the cells in Google Colab and retain the actual outputs. The code is designed to be reproducible, but the notebook output—not the illustrative values in the report—should be your final evidence.

Together with the six replacement solutions for List 1, these three sections address the remaining identified tasks. The last step is to integrate the code and explanations into the notebook, update the data model if needed, and prepare the eight-page PDF with representative screenshots and concise explanations.

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
