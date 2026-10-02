# 1251. Average Selling Price

## 📌 Problem

Given two tables:

* `Prices`
* `UnitsSold`

Find the **average selling price** for each product.

The selling price of a product can change depending on the date range.

For every product, calculate the average selling price based on:

```text
Total selling amount
--------------------
Total units sold
```

The result should be rounded to **2 decimal places**.

If a product has no units sold, its average price should be `0`.

---

## 🧠 Intuition

The important thing to understand is that the price of a product is valid only for a specific date range.

So we cannot simply join the tables using only `product_id`.

We need two conditions:

```text id="z7q3md"
Product ID should match
        AND
Purchase date should fall between start_date and end_date
```

Once we find the correct price for each sale, we calculate the weighted average selling price:

```text id="h3p8xk"
Total Revenue
-------------
Total Units
```

Where:

```text id="x5n2cq"
Revenue = units × price
```

---

## 💡 Approach

### Step 1 — Join Prices and UnitsSold

Start with the `Prices` table:

```sql id="f8c3vw"
FROM Prices p
LEFT JOIN UnitsSold u
```

We use `LEFT JOIN` because we need every product from the `Prices` table, even if it has no sales.

---

### Step 2 — Match the Product

The first join condition is:

```sql id="m2k7qp"
u.product_id = p.product_id
```

This ensures that the sale belongs to the same product.

---

### Step 3 — Match the Price Based on Date

The price should only be used when the purchase happened during the price's valid period:

```sql id="b6v9zr"
u.purchase_date BETWEEN p.start_date AND p.end_date
```

So the complete join condition is:

```sql id="r4x8mc"
LEFT JOIN UnitsSold u
    ON u.product_id = p.product_id
   AND u.purchase_date BETWEEN p.start_date AND p.end_date
```

This is the key part of the problem.

---

## 💰 Calculating Average Selling Price

The average price is **not** simply:

```text
AVG(price)
```

because different numbers of units may have been sold at different prices.

Instead, we calculate a weighted average:

```text id="9s4m2x"
(units × price) / total units
```

In SQL:

```sql id="t7k3wp"
SUM(u.units * p.price) / SUM(u.units)
```

---

## 🔢 Example

Suppose a product was sold:

| Price | Units |
| ----: | ----: |
|    10 |     2 |
|    20 |     3 |

Total revenue:

```text id="q4j7mz"
(10 × 2) + (20 × 3)
= 20 + 60
= 80
```

Total units:

```text id="x8p2kd"
2 + 3 = 5
```

Average selling price:

```text id="a6m9vc"
80 / 5 = 16
```

So the average selling price is:

```text id="8q3n1f"
16.00
```

---

## 🧮 ROUND()

The result needs to be rounded to two decimal places:

```sql id="n5x7qb"
ROUND(
    SUM(u.units * p.price) / SUM(u.units),
    2
)
```

---

## 🛡️ Handling Products With No Sales

Some products may not have any matching records in `UnitsSold`.

Because we use a `LEFT JOIN`, these products remain in the result, but:

```text id="w6c1py"
SUM(u.units) → NULL
```

Therefore, the calculation can result in `NULL`.

The problem requires `0` instead.

So we use:

```sql id="k9r4mx"
IFNULL(..., 0)
```

This converts:

```text id="n1p7vz"
NULL → 0
```

---

## 💻 Solution

```sql id="4h7m2q"
SELECT
    p.product_id,
    IFNULL(
        ROUND(
            SUM(u.units * p.price) / SUM(u.units),
            2
        ),
        0
    ) AS average_price
FROM Prices p
LEFT JOIN UnitsSold u
    ON u.product_id = p.product_id
   AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;
```

---

## 🔍 Query Breakdown

### `SUM(u.units * p.price)`

Calculates the total revenue:

```text id="c5m8xn"
Units × Price
```

---

### `SUM(u.units)`

Calculates the total number of units sold.

---

### Division

```sql id="p3x9vk"
SUM(u.units * p.price) / SUM(u.units)
```

Calculates the weighted average selling price.

---

### `ROUND(..., 2)`

```sql id="y7k2mc"
ROUND(..., 2)
```

Rounds the average price to two decimal places.

---

### `IFNULL(..., 0)`

```sql id="r8n4qb"
IFNULL(..., 0)
```

Returns `0` when there are no matching sales.

---

### `GROUP BY`

```sql id="v2m6px"
GROUP BY p.product_id
```

Calculates the average price separately for each product.

---

## 🧪 Example

### Prices

| product_id | start_date | end_date   | price |
| ---------: | ---------- | ---------- | ----: |
|          1 | 2019-02-17 | 2019-02-28 |     5 |
|          1 | 2019-03-01 | 2019-03-22 |    20 |

### UnitsSold

| product_id | purchase_date | units |
| ---------: | ------------- | ----: |
|          1 | 2019-02-25    |   100 |
|          1 | 2019-03-10    |    50 |

The matching prices are:

```text id="j4c8mz"
2019-02-25 → price 5
2019-03-10 → price 20
```

Revenue:

```text id="p6x1qw"
100 × 5 = 500
50 × 20 = 1000

Total = 1500
```

Units:

```text id="n8m3cy"
100 + 50 = 150
```

Average:

```text id="s5q9vk"
1500 / 150 = 10
```

Result:

| product_id | average_price |
| ---------: | ------------: |
|          1 |         10.00 |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `LEFT JOIN`
* Multiple JOIN conditions
* `BETWEEN`
* Date-range matching
* `SUM()`
* Weighted average
* `ROUND()`
* `IFNULL()`
* `GROUP BY`
* Aggregation

---

## 📚 Key Takeaways

### 1. Date range can be part of JOIN logic

```sql id="b3m7xp"
AND u.purchase_date BETWEEN p.start_date AND p.end_date
```

This ensures the correct price is selected based on when the product was purchased.

---

### 2. Weighted average

When quantities differ, don't simply use:

```sql id="a8k2mv"
AVG(price)
```

Instead use:

```sql id="z4p6qx"
SUM(units * price) / SUM(units)
```

This gives the correct average selling price based on the number of units sold.

---

### 3. IFNULL handles missing data

```sql id="m7c3nx"
IFNULL(calculation, 0)
```

This is useful when a product has no sales and the aggregate calculation returns `NULL`.

---

## 🔗 Practice

**LeetCode:** Average Selling Price

https://leetcode.com/problems/average-selling-price/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `LEFT JOIN` `BETWEEN` `Date Range` `SUM` `Weighted Average` `ROUND` `IFNULL` `GROUP BY` `Aggregation`
