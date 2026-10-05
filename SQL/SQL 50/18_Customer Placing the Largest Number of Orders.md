# 596. Classes With at Least 5 Students

## Problem

Write a SQL query to find all classes that have **at least 5 students**.

### Table: `Courses`

| Column  | Description  |
| ------- | ------------ |
| student | Student name |
| class   | Class name   |

We need to return the classes where the number of students is **5 or more**.

---

## Intuition

This is a very simple problem.

We need to:

1. Group the records by `class`.
2. Count how many students are in each class.
3. Keep only the classes where the count is **at least 5**.

Since we are filtering the result of an aggregate function (`COUNT`), we use `HAVING` instead of `WHERE`.

---

## Approach

### Step 1 — Group by class

```sql
GROUP BY class
```

This creates one group for each class.

For example:

```text
Math      → 6 students
Science   → 4 students
English   → 5 students
```

### Step 2 — Count students

```sql
COUNT(class)
```

This tells us how many students are present in each class.

### Step 3 — Keep classes with at least 5 students

```sql
HAVING COUNT(class) >= 5
```

`HAVING` is used because we are filtering an aggregated result.

---

## Solution

```sql
SELECT
    class
FROM Courses
GROUP BY class
HAVING COUNT(class) >= 5;
```

---

## Alternative Approach

We can also first calculate the student count in a subquery and then filter it in the outer query:

```sql
SELECT
    class
FROM (
    SELECT
        class,
        COUNT(*) AS student_count
    FROM Courses
    GROUP BY class
) a
WHERE student_count >= 5;
```

This approach works, but the `GROUP BY + HAVING` solution is more direct for this problem.

---

## Why `HAVING` Instead of `WHERE`?

A common SQL concept here is the difference between `WHERE` and `HAVING`.

### `WHERE`

Filters individual rows **before** grouping.

```sql
WHERE class = 'Math'
```

### `HAVING`

Filters groups **after** aggregation.

```sql
HAVING COUNT(class) >= 5
```

Since `COUNT()` is an aggregate function, we use `HAVING`.

---

## Query Flow

```text
Courses
   ↓
GROUP BY class
   ↓
COUNT students in each class
   ↓
HAVING COUNT(class) >= 5
   ↓
Return class
```

---

## Concepts Learned

* `GROUP BY`
* `COUNT()`
* `HAVING`
* Aggregate functions
* Difference between `WHERE` and `HAVING`
* Filtering grouped results
* Subqueries

---

## Key Takeaway

When the question says:

> Find groups having at least N records

Think:

```sql
GROUP BY ...
HAVING COUNT(...) >= N
```

For this problem:

```sql
GROUP BY class
HAVING COUNT(class) >= 5
```

---

## Complexity

Let `N` be the number of rows in `Courses`.

* **Time Complexity:** `O(N)` on average for grouping/counting, depending on the database execution plan.
* **Space Complexity:** `O(C)` for the grouped classes, where `C` is the number of distinct classes.

---

## Example

If the data contains:

```text
class
-----
Math
Math
Math
Math
Math
Science
Science
English
English
English
English
English
```

The counts are:

```text
Math     → 5
Science  → 2
English  → 5
```

After:

```sql
HAVING COUNT(class) >= 5
```

The result is:

```text
Math
English
```

---

## Practice

[LeetCode — 596. Classes With at Least 5 Students](https://leetcode.com/problems/classes-with-at-least-5-students/)

## Tags

`SQL` `GROUP BY` `COUNT` `HAVING` `Aggregation`
