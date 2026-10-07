# 196. Delete Duplicate Emails

## Problem

Write a SQL query to delete all duplicate emails from the `Person` table.

For each email, we need to **keep the person with the smallest `id`** and delete the remaining duplicate records.

### Table: `Person`

| Column | Description             |
| ------ | ----------------------- |
| id     | Unique ID of the person |
| email  | Email address           |

---

## Intuition

The easiest way to think about this problem is:

> First find the minimum `id` for each unique email.
> These are the records that we want to keep.
> Then delete every `id` that is not in that list.

So the logic is:

```text
Person
   ↓
GROUP BY email
   ↓
Find MIN(id) for each email
   ↓
These IDs should be kept
   ↓
Delete IDs NOT IN this list
```

---

## Approach

### Step 1 — Find the minimum ID for each email

```sql
SELECT
    MIN(id) AS id
FROM Person
GROUP BY email;
```

For example:

```text
id    email
---   ----------------
1     a@gmail.com
2     a@gmail.com
3     b@gmail.com
4     b@gmail.com
5     c@gmail.com
```

The query returns:

```text
id
--
1
3
5
```

These are the IDs we want to **keep**.

---

### Step 2 — Use those IDs as a filter

We can use the previous query as a subquery:

```sql
SELECT id
FROM (
    SELECT
        MIN(id) AS id
    FROM Person
    GROUP BY email
) AS temp;
```

This gives us the list of IDs that should remain.

---

### Step 3 — Delete everything else

Now we use:

```sql
WHERE id NOT IN (...)
```

This means:

> Delete the records whose ID is **not** one of the minimum IDs.

---

## Solution

```sql
DELETE FROM Person
WHERE id NOT IN (
    SELECT id
    FROM (
        SELECT
            MIN(id) AS id
        FROM Person
        GROUP BY email
    ) AS temp
);
```

---

## Why Do We Need the Extra Subquery?

In MySQL, when modifying a table and selecting from the same table inside a subquery, using a derived table helps avoid the **"You can't specify target table for update in FROM clause"** issue.

That's why this:

```sql
SELECT MIN(id)
FROM Person
GROUP BY email
```

is wrapped inside:

```sql
SELECT id
FROM (
    ...
) AS temp
```

Then the outer `DELETE` can use that result.

---

## Example

Before deletion:

```text
id    email
---   ----------------
1     john@gmail.com
2     john@gmail.com
3     alex@gmail.com
4     alex@gmail.com
5     sam@gmail.com
```

Minimum ID for each email:

```text
john@gmail.com  → 1
alex@gmail.com  → 3
sam@gmail.com   → 5
```

So IDs `1`, `3`, and `5` are kept.

IDs `2` and `4` are deleted.

### Final table

```text
id    email
---   ----------------
1     john@gmail.com
3     alex@gmail.com
5     sam@gmail.com
```

---

## Query Flow

```text
Person
   ↓
GROUP BY email
   ↓
MIN(id)
   ↓
IDs to KEEP
   ↓
NOT IN
   ↓
Delete duplicate IDs
```

---

## Important Concept: `MIN(id)`

The problem specifically says to keep the person with the smallest ID.

That's why we use:

```sql
MIN(id)
```

If the IDs are:

```text
1
4
7
```

then:

```text
MIN(id) = 1
```

So ID `1` stays and IDs `4` and `7` are deleted.

---

## Concepts Learned

* `DELETE`
* Subqueries
* Nested subqueries
* `GROUP BY`
* `MIN()`
* `NOT IN`
* Derived tables
* Removing duplicate records
* Keeping one record from each duplicate group

---

## Key Takeaway

A useful pattern for this type of problem is:

```sql
DELETE FROM table
WHERE id NOT IN (
    SELECT id
    FROM (
        SELECT MIN(id)
        FROM table
        GROUP BY duplicate_column
    ) AS temp
);
```

The important thought process is:

**Find what to keep → identify everything else → delete everything else.**

---

## Complexity

Let `N` be the number of rows in `Person`.

* **Time Complexity:** Approximately `O(N)` for grouping/filtering, depending on the database execution plan and indexes.
* **Space Complexity:** `O(E)`, where `E` is the number of unique email addresses retained by the grouping operation.

---

## Practice

[LeetCode — 196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)

## Tags

`SQL` `DELETE` `GROUP BY` `MIN` `Subquery` `NOT IN` `Duplicates`
