# 570. Managers with at Least 5 Direct Reports

## 📌 Problem

Given an `Employee` table, find the names of managers who have **at least 5 direct reports**.

A manager is considered to have a direct report when an employee's `managerId` is equal to that manager's `id`.

---

## 🧠 Intuition

The first thing I need to find is:

> Which `managerId` appears at least 5 times in the `Employee` table?

If the same `managerId` appears 5 or more times, it means that manager has at least 5 direct reports.

So I can:

1. Group employees by `managerId`.
2. Count how many employees belong to each manager.
3. Keep only managers whose count is at least `5`.
4. Use those `managerId` values as a filter in the outer query.
5. Get the corresponding manager names.

The important part is that the question asks for the **manager's name**, not just the `managerId`.

---

## 💡 Approach

### Step 1 — Find managers with at least 5 reports

Use the `Employee` table:

```sql id="g8k3xp"
SELECT managerId
FROM Employee
GROUP BY managerId
HAVING COUNT(*) >= 5
```

This groups employees by their `managerId`.

For example:

```text id="4f1z7n"
managerId | COUNT(*)
----------|---------
1         | 6
2         | 3
5         | 8
```

After applying:

```sql id="8x4m2q"
HAVING COUNT(*) >= 5
```

we get:

```text id="n7s2ka"
1
5
```

These are the managers who have at least 5 direct reports.

---

### Step 2 — Use the result as a filter

Now we need the names of those managers.

The outer query:

```sql id="q3x8v1"
SELECT e.name
FROM Employee e
```

gets employee names.

Then we filter their IDs:

```sql id="j9k2pw"
WHERE e.id IN (...)
```

The subquery provides the IDs of managers who have at least 5 direct reports.

---

## 💻 Solution

```sql id="x5v7mz"
SELECT
    e.name AS name
FROM Employee e
WHERE e.id IN (
    SELECT managerId
    FROM Employee
    GROUP BY managerId
    HAVING COUNT(*) >= 5
);
```

---

## 🔍 Query Breakdown

### 1. Outer Query

```sql id="n2k8qd"
SELECT e.name AS name
FROM Employee e
```

We want the **names** of the managers.

---

### 2. Filter Manager IDs

```sql id="q7m4vx"
WHERE e.id IN (...)
```

The `IN` operator checks whether the employee's ID exists in the list returned by the subquery.

---

### 3. Subquery

```sql id="r8p1wy"
SELECT managerId
FROM Employee
```

We retrieve the `managerId` values.

---

### 4. GROUP BY managerId

```sql id="m3c9ka"
GROUP BY managerId
```

This groups employees according to their manager.

For example:

```text id="f7w2nz"
Employee A → Manager 1
Employee B → Manager 1
Employee C → Manager 1

Employee D → Manager 2
Employee E → Manager 2
```

becomes:

```text id="q1x6vb"
Manager 1 → 3 employees
Manager 2 → 2 employees
```

---

### 5. HAVING COUNT(*) >= 5

```sql id="c4v8mt"
HAVING COUNT(*) >= 5
```

`HAVING` filters the **groups** created by `GROUP BY`.

This keeps only managers who have 5 or more employees reporting to them.

---

## 🧪 Example

Suppose the table contains:

| id | name    | managerId |
| -: | ------- | --------: |
|  1 | Alice   |      NULL |
|  2 | Bob     |         1 |
|  3 | Charlie |         1 |
|  4 | David   |         1 |
|  5 | Eve     |         1 |
|  6 | Frank   |         1 |
|  7 | Grace   |         1 |
|  8 | Harry   |         2 |

Manager `1` has:

```text id="v7d2mx"
Bob
Charlie
David
Eve
Frank
Grace
```

That's **6 direct reports**.

Manager `2` has only:

```text id="e5x9kw"
Harry
```

So the subquery returns:

```text id="r2m6qb"
managerId
---------
1
```

The outer query then finds:

```text id="z3k7pq"
id = 1
name = Alice
```

### Result

| name  |
| ----- |
| Alice |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* Subqueries
* `IN`
* `GROUP BY`
* `HAVING`
* `COUNT()`
* Filtering grouped data
* Self-referencing relationships
* Manager/employee hierarchy

---

## 📚 Key Takeaways

### 1. GROUP BY creates groups

```sql id="g7x2np"
GROUP BY managerId
```

Groups employees based on their manager.

---

### 2. HAVING filters groups

```sql id="n5q8vr"
HAVING COUNT(*) >= 5
```

`HAVING` is used after `GROUP BY` when we need to filter based on an aggregate result.

A useful distinction:

```text id="4f8m2c"
WHERE  → filters rows
HAVING → filters groups
```

---

### 3. IN with a subquery

```sql id="k9v3ds"
WHERE e.id IN (
    SELECT managerId
    FROM Employee
    ...
)
```

The inner query returns a list of manager IDs, and the outer query finds the corresponding employee names.

---

### 4. Same table can represent relationships

The `Employee` table contains both:

```text id="p4z7cx"
id
managerId
```

So:

```text id="x6m1qa"
Employee.id
      ↑
      |
Employee.managerId
```

creates a manager → employee relationship within the same table.

---

## 🔗 Practice

**LeetCode:** Managers with at Least 5 Direct Reports

https://leetcode.com/problems/managers-with-at-least-5-direct-reports/

---

## ✅ Difficulty

**Medium**

---

## 🏷️ Tags

`SQL` `MySQL` `Subquery` `IN` `GROUP BY` `HAVING` `COUNT` `Employee Hierarchy`
