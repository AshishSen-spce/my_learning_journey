# 584. Find Customer Referee

## 📌 Problem

Given a `Customer` table, find the names of customers whose `referee_id` is **not equal to 2**.

Customers with a `NULL` `referee_id` should also be included.

### Table: `Customer`

| Column       | Type    |
| ------------ | ------- |
| `id`         | int     |
| `name`       | varchar |
| `referee_id` | int     |

---

## 🎯 Requirement

Return the `name` of customers where:

```text
referee_id != 2
```

or where:

```text
referee_id IS NULL
```

---

## 💡 Solution

```sql
SELECT
    name
FROM Customer c
WHERE c.referee_id !=2
   OR c.referee_id IS NULL;
```

---

## 🔍 Explanation

There are two conditions in the `WHERE` clause.

### Condition 1 — Referee is not 2

```sql
c.referee_id != 2
```

This returns customers whose `referee_id` has a value other than `2`.

For example:

```text
referee_id = 1  → included
referee_id = 3  → included
referee_id = 5  → included
referee_id = 2  → excluded
```

### Condition 2 — Referee is NULL

```sql
c.referee_id IS NULL
```

This includes customers who do not have a referee assigned.

---

## ⚠️ Important SQL Concept: NULL

A common mistake is trying to write:

```sql
referee_id = NULL
```

or:

```sql
referee_id != NULL
```

This does **not** work in SQL.

`NULL` represents an unknown or missing value, so we use:

```sql
IS NULL
```

and:

```sql
IS NOT NULL
```

---

## 🧠 Why `OR` is Used

We want customers who satisfy **either** condition:

```sql
referee_id != 2
OR
referee_id IS NULL
```

Therefore, `OR` is required.

---

## 🧪 Example

Suppose the table contains:

| id | name | referee_id |
| -: | ---- | ---------: |
|  1 | Will |       NULL |
|  2 | Jane |       NULL |
|  3 | Alex |          2 |
|  4 | Bill |       NULL |
|  5 | Zack |          1 |
|  6 | Mark |          2 |

The result will be:

| name |
| ---- |
| Will |
| Jane |
| Bill |
| Zack |

`Alex` and `Mark` are excluded because their `referee_id = 2`.

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `FROM`
* `WHERE`
* `OR`
* `!=` / `<>`
* `NULL`
* `IS NULL`
* Filtering records
* SQL three-valued logic

---

## 📚 Key Takeaways

### 1. Use `!=` or `<>` for "not equal"

```sql
referee_id != 2
```

or:

```sql
referee_id <> 2
```

Both are commonly supported for "not equal."

### 2. Use `IS NULL` for NULL values

```sql
referee_id IS NULL
```

Do not use:

```sql
referee_id = NULL
```

### 3. Use `OR` when either condition can be true

```sql
WHERE condition_1
   OR condition_2
```

---

## 🔗 Practice

**LeetCode:** Find Customer Referee

https://leetcode.com/problems/find-customer-referee/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELECT` `WHERE` `OR` `NULL` `IS NULL` `Filtering`
