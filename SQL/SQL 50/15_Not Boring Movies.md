# 620. Not Boring Movies

## 📌 Problem

Given a `Cinema` table, find movies that satisfy both conditions:

1. The movie description is **not** `"boring"`.
2. The movie ID is **odd**.

The result should contain:

* `id`
* `movie`
* `description`
* `rating`

The movies should be sorted by `rating` in **descending order**.

---

## 🧠 Intuition

This is a very simple problem because all the requirements are directly mentioned in the question.

We just need to translate each requirement into a SQL condition.

The question says:

```text
Description should NOT be boring
        AND
Movie ID should be odd
        AND
Sort by rating from highest to lowest
```

So we need:

```sql
description != 'boring'
```

for the first condition.

For an odd ID:

```sql
id % 2 != 0
```

And finally:

```sql
ORDER BY rating DESC
```

---

## 💡 Approach

1. Select the required columns from the `Cinema` table.
2. Filter out movies where `description = 'boring'`.
3. Keep only movies with an odd `id`.
4. Sort the remaining movies by `rating` in descending order.

---

## 💻 Solution

```sql id="b4w9q2"
SELECT
    id,
    movie,
    description,
    rating
FROM Cinema
WHERE description != 'boring'
  AND id % 2 != 0
ORDER BY rating DESC;
```

---

## 🔍 Query Breakdown

### 1. Select required columns

```sql id="x8m3kp"
SELECT
    id,
    movie,
    description,
    rating
```

These are the columns required in the output.

---

### 2. Remove boring movies

```sql id="v2n7cq"
WHERE description != 'boring'
```

The `!=` operator means **not equal to**.

So:

```text id="h3k8mx"
description = 'boring' → ❌
description = 'funny'  → ✅
description = 'great'  → ✅
```

---

### 3. Find odd IDs

```sql id="p6r1yt"
id % 2 != 0
```

The `%` operator returns the remainder after division.

For an odd number:

```text id="f9k2wd"
5 % 2 = 1
7 % 2 = 1
9 % 2 = 1
```

Therefore:

```sql id="a7c3mz"
id % 2 != 0
```

keeps odd IDs.

For even numbers:

```text id="q8v4bn"
4 % 2 = 0
6 % 2 = 0
8 % 2 = 0
```

So they are excluded.

---

### 4. Combine conditions with AND

```sql id="j5x9kp"
WHERE description != 'boring'
  AND id % 2 != 0
```

Both conditions must be true.

```text id="n6q2wd"
Not boring
    AND
Odd ID
```

---

### 5. Sort by rating

```sql id="r3m7vx"
ORDER BY rating DESC
```

`DESC` means **descending order**.

So the highest-rated movie appears first.

Example:

```text id="y8c4pq"
9.5
8.7
7.9
6.5
```

---

## 🧪 Example

### Input

| id | movie   | description | rating |
| -: | ------- | ----------- | -----: |
|  1 | Movie A | boring      |    8.0 |
|  2 | Movie B | fun         |    9.0 |
|  3 | Movie C | exciting    |    7.5 |
|  4 | Movie D | boring      |    9.5 |
|  5 | Movie E | great       |    8.8 |

### Apply the conditions

#### Condition 1

```text id="9g6x8b"
description != 'boring'
```

Removes Movie A and Movie D.

#### Condition 2

```text id="3r7n1c"
id % 2 != 0
```

Keeps IDs:

```text id="s8j2kf"
1, 3, 5
```

After both conditions:

| id | movie   | description | rating |
| -: | ------- | ----------- | -----: |
|  3 | Movie C | exciting    |    7.5 |
|  5 | Movie E | great       |    8.8 |

### Apply `ORDER BY rating DESC`

Final result:

| id | movie   | description | rating |
| -: | ------- | ----------- | -----: |
|  5 | Movie E | great       |    8.8 |
|  3 | Movie C | exciting    |    7.5 |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `FROM`
* `WHERE`
* `AND`
* `!=`
* Modulo `%`
* Odd/even number filtering
* `ORDER BY`
* `DESC`
* Multiple filtering conditions

---

## 📚 Key Takeaways

### 1. `%` Modulo Operator

The modulo operator returns the remainder.

```sql id="j7p4kx"
id % 2
```

Useful for checking odd/even numbers:

```text id="4x8n2v"
id % 2 = 0 → Even
id % 2 = 1 → Odd
```

---

### 2. Multiple conditions

When all conditions must be satisfied, use:

```sql id="n3q8yw"
WHERE condition_1
  AND condition_2
```

---

### 3. Descending order

```sql id="k5m2zp"
ORDER BY rating DESC
```

sorts from highest to lowest.

---

## 🔗 Practice

**LeetCode:** Not Boring Movies

https://leetcode.com/problems/not-boring-movies/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `WHERE` `AND` `Modulo` `Odd Numbers` `ORDER BY` `DESC` `Filtering`
