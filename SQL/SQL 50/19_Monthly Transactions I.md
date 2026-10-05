# 1193. Monthly Transactions I

## Problem

Write a SQL query to find monthly transaction statistics for each country.

For every `month` and `country`, we need to calculate:

* Total number of transactions
* Number of approved transactions
* Total transaction amount
* Total amount of approved transactions

---

## Intuition

The main thing is to understand the required grouping.

We need the result **month-wise and country-wise**, so we need:

```sql
GROUP BY month, country
```

The transaction date contains the complete date, but the question asks for the **month**, so we first convert `trans_date` into:

```text
YYYY-MM
```

using:

```sql
DATE_FORMAT(trans_date, '%Y-%m')
```

Then we can calculate the required counts and amounts using aggregate functions.

For approved transactions, we can use `CASE WHEN` inside `COUNT()` and `SUM()`.

---

## Approach

### Step 1 — Extract the month

```sql
DATE_FORMAT(trans_date, '%Y-%m') AS month
```

For example:

```text
2019-01-01 → 2019-01
2019-01-15 → 2019-01
2019-02-10 → 2019-02
```

This allows us to aggregate transactions month-wise.

---

### Step 2 — Group by month and country

```sql
GROUP BY
    DATE_FORMAT(trans_date, '%Y-%m'),
    country
```

This creates groups like:

```text
2019-01 + US
2019-01 + India
2019-02 + US
2019-02 + India
```

---

### Step 3 — Count total transactions

```sql
COUNT(*) AS trans_count
```

`COUNT(*)` counts all transactions in each month-country group.

---

### Step 4 — Count approved transactions

```sql
COUNT(
    CASE
        WHEN state = 'approved' THEN 1
    END
) AS approved_count
```

The `CASE` returns `1` only when the transaction is approved.

For other states it returns `NULL`.

Since `COUNT()` does not count `NULL`, only approved transactions are counted.

---

### Step 5 — Calculate total transaction amount

```sql
SUM(amount) AS trans_total_amount
```

This calculates the total amount of all transactions in the group.

---

### Step 6 — Calculate approved transaction amount

```sql
SUM(
    CASE
        WHEN state = 'approved' THEN amount
        ELSE 0
    END
) AS approved_total_amount
```

Here:

* Approved transaction → add its `amount`
* Other transaction → add `0`

So we get the total amount only for approved transactions.

---

## Solution

```sql
SELECT
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(*) AS trans_count,
    COUNT(
        CASE
            WHEN state = 'approved' THEN 1
        END
    ) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(
        CASE
            WHEN state = 'approved' THEN amount
            ELSE 0
        END
    ) AS approved_total_amount
FROM Transactions
GROUP BY
    DATE_FORMAT(trans_date, '%Y-%m'),
    country;
```

---

## Example

Suppose we have:

```text
trans_date   country   state       amount
----------   -------   ----------  ------
2019-01-01   US        approved    1000
2019-01-02   US        declined     500
2019-01-10   US        approved     300
2019-01-15   India     approved     700
```

For US in January:

```text
trans_count            = 3
approved_count         = 2
trans_total_amount     = 1800
approved_total_amount  = 1300
```

So the result contains:

```text
month      country   trans_count   approved_count   trans_total_amount   approved_total_amount
---------  -------   -----------   ---------------  -------------------  ----------------------
2019-01    US        3             2                1800                 1300
2019-01    India     1             1                 700                  700
```

---

## Important Concept: Conditional Aggregation

One of the most important concepts in this problem is **conditional aggregation**.

Instead of filtering rows with `WHERE`, we can put conditions inside aggregate functions.

### Count conditionally

```sql
COUNT(
    CASE WHEN state = 'approved' THEN 1 END
)
```

### Sum conditionally

```sql
SUM(
    CASE WHEN state = 'approved' THEN amount ELSE 0 END
)
```

This allows us to calculate multiple metrics from the same group.

---

## Query Flow

```text
Transactions
      ↓
Convert trans_date → YYYY-MM
      ↓
Group by month + country
      ↓
      ├── COUNT(*) → Total transactions
      │
      ├── COUNT(CASE...) → Approved transactions
      │
      ├── SUM(amount) → Total amount
      │
      └── SUM(CASE...) → Approved amount
```

---

## Concepts Learned

* `DATE_FORMAT()`
* `GROUP BY`
* `COUNT()`
* `SUM()`
* `CASE WHEN`
* Conditional aggregation
* Date-based aggregation
* Multiple aggregate calculations

---

## Key Takeaway

When a SQL question asks for multiple metrics based on a condition, think about **conditional aggregation**.

For example:

```sql
COUNT(CASE WHEN condition THEN 1 END)
```

and:

```sql
SUM(CASE WHEN condition THEN amount ELSE 0 END)
```

Also remember:

```sql
DATE_FORMAT(date_column, '%Y-%m')
```

is useful when the requirement is to aggregate data **month-wise**.

---

## Practice

[LeetCode — 1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)

## Tags

`SQL` `GROUP BY` `COUNT` `SUM` `CASE WHEN` `DATE_FORMAT` `Conditional Aggregation`
