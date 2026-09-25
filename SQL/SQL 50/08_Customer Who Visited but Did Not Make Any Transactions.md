# 1581. Customer Who Visited but Did Not Make Any Transactions

## 📌 Problem

Given two tables:

* `Visits`
* `Transactions`

Find the customers who visited the store but **did not make any transactions** during their visit.

The result should contain:

* `customer_id`
* Number of visits where the customer did not make a transaction

---

## 📊 Tables

### `Visits`

| Column        | Type |
| ------------- | ---- |
| `visit_id`    | int  |
| `customer_id` | int  |

### `Transactions`

| Column           | Type |
| ---------------- | ---- |
| `transaction_id` | int  |
| `visit_id`       | int  |
| `amount`         | int  |

The `visit_id` connects the two tables.

---

## 🧠 Intuition

The main idea is:

> First, find all `visit_id` values that have a transaction. Then exclude those visits from the `Visits` table.

So the logic is:

```text
Transactions
     ↓
Get visit_id
     ↓
Use those visit_id values as a filter
     ↓
Find visits NOT present in Transactions
     ↓
Group by customer
     ↓
Count their visits
```

---

## 🎯 Approach

### Step 1 — Fetch all visit IDs that have transactions

We use a subquery:

```sql id="2q8d4h"
SELECT visit_id
FROM Transactions
```

This gives us the list of visits where a transaction was made.

For example:

```text id="o0v6jc"
Transactions

visit_id
--------
1
3
5
7
```

These visits already have transactions.

---

### Step 2 — Exclude those visits

From the `Visits` table, we only want visits whose `visit_id` is **not present** in the transaction list.

We use:

```sql id="j7o7xw"
WHERE v.visit_id NOT IN (
    SELECT visit_id
    FROM Transactions
)
```

This means:

```text
Visit ID NOT IN Transaction Visit IDs
```

For example:

```text id="j4z9xx"
Visits:

1
2
3
4
5
6

Transactions:

1
3
5
```

After filtering:

```text id="8z5n6r"
2
4
6
```

These are visits where no transaction was made.

---

### Step 3 — Group by customer

A customer may have visited multiple times without making a transaction.

Therefore, we group the remaining visits by `customer_id`:

```sql id="1fhjv7"
GROUP BY 1
```

Here:

```text id="4gzq9y"
1 → customer_id
```

So this is equivalent to:

```sql id="c8w1yv"
GROUP BY customer_id
```

---

### Step 4 — Count the visits

Finally, we count how many visits each customer made without transactions:

```sql id="m7x2kd"
COUNT(v.visit_id)
```

We give the result the required name:

```sql id="4h2n8c"
COUNT(v.visit_id) AS count_no_trans
```

---

## 💡 Solution

```sql id="b6kq3r"
SELECT
    v.customer_id AS customer_id,
    COUNT(v.visit_id) AS count_no_trans
FROM Visits v
WHERE v.visit_id NOT IN (
    SELECT visit_id
    FROM Transactions
)
GROUP BY 1;
```

---

## 🧪 Example

### Visits

| visit_id | customer_id |
| -------: | ----------: |
|        1 |          23 |
|        2 |           9 |
|        4 |          30 |
|        5 |           9 |
|        6 |          23 |

### Transactions

| transaction_id | visit_id | amount |
| -------------: | -------: | -----: |
|            100 |        1 |    500 |
|            101 |        4 |    200 |

The transaction visit IDs are:

```text id="7n5c9k"
1
4
```

So we exclude visits `1` and `4`.

Remaining visits:

| visit_id | customer_id |
| -------: | ----------: |
|        2 |           9 |
|        5 |           9 |
|        6 |          23 |

After grouping and counting:

| customer_id | count_no_trans |
| ----------: | -------------: |
|           9 |              2 |
|          23 |              1 |

---

## 🔍 Full Query Breakdown

### Select

```sql id="v1h1z7"
SELECT
    v.customer_id AS customer_id,
    COUNT(v.visit_id) AS count_no_trans
```

Returns the customer and the number of visits without transactions.

### From

```sql id="0sgpkl"
FROM Visits v
```

We start with the `Visits` table because we want to identify visits that didn't result in a transaction.

### Filter

```sql id="y0z8tw"
WHERE v.visit_id NOT IN (
    SELECT visit_id
    FROM Transactions
)
```

The subquery provides all visit IDs that have transactions.

`NOT IN` removes those visits.

### Group

```sql id="b6j7p3"
GROUP BY 1
```

Groups the result by `customer_id`.

This can also be written as:

```sql id="2m8qyo"
GROUP BY v.customer_id
```

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* Subqueries
* `NOT IN`
* `SELECT`
* `WHERE`
* `COUNT()`
* `GROUP BY`
* Column aliases
* Filtering using another table
* Aggregating filtered records

---

## 📚 Key Takeaways

### 1. Subquery

A query can be placed inside another query:

```sql id="z3j6r2"
WHERE visit_id NOT IN (
    SELECT visit_id
    FROM Transactions
)
```

The inner query runs conceptually to provide the values used by the outer query.

---

### 2. `NOT IN`

`NOT IN` is useful when you want to exclude values returned by another query:

```sql id="xw7u3m"
WHERE visit_id NOT IN (...)
```

---

### 3. `COUNT()` with `GROUP BY`

When we want to count records for each customer:

```sql id="c7z9yk"
SELECT
    customer_id,
    COUNT(visit_id)
FROM Visits
GROUP BY customer_id;
```

---

### 4. `GROUP BY 1`

Your solution uses:

```sql id="8ev0oj"
GROUP BY 1
```

The `1` refers to the first column in the `SELECT` list:

```sql id="9pmx8j"
v.customer_id
```

So:

```sql id="f2qq7r"
GROUP BY 1
```

is equivalent to:

```sql id="kq8d1w"
GROUP BY v.customer_id
```

For learning and readability, explicitly writing `GROUP BY v.customer_id` is often easier to understand.

---

## 🔗 Practice

**LeetCode:** Customer Who Visited but Did Not Make Any Transactions

https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `Subquery` `NOT IN` `GROUP BY` `COUNT` `WHERE` `Aggregation` `Filtering`
