# 1141. User Activity for the Past 30 Days I

## Problem

Write a SQL query to find the number of **active users for each day** during the 30-day period from **2019-06-28 to 2019-07-27**.

A user is considered active if they have at least one activity on that day.

---

## Intuition

The question is asking for:

> How many unique users were active on each day?

So the approach is:

1. Filter the data for the required 30-day period.
2. Group the data by `activity_date`.
3. Count distinct `user_id` for each day.

The important part is `COUNT(DISTINCT user_id)` because the same user can have multiple activities on the same day, but should only be counted **once**.

---

## Approach

### Step 1 — Filter the required date range

```sql
WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27'
```

This keeps only activities that happened within the required 30-day period.

`BETWEEN` is inclusive, so both `2019-06-28` and `2019-07-27` are included.

---

### Step 2 — Group by day

```sql
GROUP BY activity_date
```

This creates one group for each activity date.

For example:

```text
2019-07-01
2019-07-02
2019-07-03
...
```

---

### Step 3 — Count unique users

```sql
COUNT(DISTINCT user_id)
```

This counts each user only once per day.

For example, if:

```text
2019-07-01 | User 1
2019-07-01 | User 1
2019-07-01 | User 2
2019-07-01 | User 3
```

Then:

```text
active_users = 3
```

not `4`.

---

## Solution

```sql
SELECT
    activity_date AS day,
    COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27'
GROUP BY activity_date;
```

---

## Example

Suppose the data looks like:

```text
activity_date   user_id
-------------   -------
2019-07-01      1
2019-07-01      1
2019-07-01      2
2019-07-01      3
2019-07-02      1
2019-07-02      2
```

For `2019-07-01`:

```text
User 1
User 1
User 2
User 3
```

There are **3 unique users**.

For `2019-07-02`:

```text
User 1
User 2
```

There are **2 unique users**.

Result:

```text
day          active_users
----------   ------------
2019-07-01   3
2019-07-02   2
```

---

## Why `DISTINCT` Is Important

If we used:

```sql
COUNT(user_id)
```

then multiple activities by the same user would be counted multiple times.

For example:

```text
User 1 → activity
User 1 → activity
User 1 → activity
User 2 → activity
```

`COUNT(user_id)` gives:

```text
4
```

But:

```sql
COUNT(DISTINCT user_id)
```

gives:

```text
2
```

because there are only two unique active users.

---

## Query Flow

```text
Activity
   ↓
Filter last 30 days
   ↓
GROUP BY activity_date
   ↓
COUNT(DISTINCT user_id)
   ↓
Daily active users
```

---

## Concepts Learned

* `WHERE`
* `BETWEEN`
* `GROUP BY`
* `COUNT()`
* `DISTINCT`
* Column alias using `AS`
* Date filtering
* Counting unique users

---

## Key Takeaway

Whenever the question asks:

> How many unique users/customers were there?

Think:

```sql
COUNT(DISTINCT user_id)
```

And when the question asks for the result **day-wise**:

```sql
GROUP BY activity_date
```

So the core pattern is:

```sql
SELECT
    activity_date,
    COUNT(DISTINCT user_id)
FROM Activity
GROUP BY activity_date;
```

Then add the required date filter using `WHERE`.

---

## Complexity

Let `N` be the number of activity records in the filtered date range.

* **Time Complexity:** Approximately `O(N)`, depending on the database execution plan and indexing.
* **Space Complexity:** Depends on the number of unique dates and users that need to be tracked during aggregation.

---

## Practice

[LeetCode — 1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/)

## Tags

`SQL` `COUNT` `DISTINCT` `GROUP BY` `WHERE` `BETWEEN` `Date Filtering`
