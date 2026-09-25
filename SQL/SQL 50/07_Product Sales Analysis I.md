# 1068. Product Sales Analysis I

## 📌 Problem

Given two tables:

* `Sales`
* `Product`

Return the product name, sales year, and price for every sale.

The two tables are connected using the `product_id` column.

---

## 📊 Tables

### `Sales`

| Column       | Type |
| ------------ | ---- |
| `seller_id`  | int  |
| `product_id` | int  |
| `buyer_id`   | int  |
| `year`       | int  |
| `quantity`   | int  |
| `price`      | int  |

### `Product`

| Column         | Type    |
| -------------- | ------- |
| `product_id`   | int     |
| `product_name` | varchar |

---

## 🎯 Requirement

Return the following columns:

* `product_name`
* `year`
* `price`

The `product_name` needs to be obtained from the `Product` table using `product_id`.

---

## 💡 Solution

```sql
SELECT
    P.product_name AS product_name,
    S.year AS year,
    S.price
FROM Sales S
LEFT JOIN Product P
    ON S.product_id = P.product_id;
```

---

## 🔍 Explanation

### 1. Select the required columns

```sql
SELECT
    P.product_name,
    S.year,
    S.price
```

We need:

* Product name → from `Product`
* Year → from `Sales`
* Price → from `Sales`

---

### 2. Start with the Sales table

```sql
FROM Sales S
```

`Sales` is the main table because each record represents a sale.

We give it the alias:

```text
S → Sales
```

This allows us to write:

```sql
S.year
S.price
S.product_id
```

instead of repeatedly writing `Sales.year`, etc.

---

### 3. Join the Product table

```sql
LEFT JOIN Product P
```

The `Product` table contains the product name.

We give it the alias:

```text
P → Product
```

---

### 4. Match the two tables

```sql
ON S.product_id = P.product_id
```

The `product_id` column is used to connect the two tables.

For example:

```text
Sales
product_id = 100

        ↓

Product
product_id = 100
product_name = "Laptop"
```

The matching product name can then be returned.

---

## 🧪 Example

### Product

| product_id | product_name |
| ---------: | ------------ |
|          1 | S8           |
|          2 | G4           |
|          3 | iPhone       |

### Sales

| seller_id | product_id | year | quantity | price |
| --------: | ---------: | ---: | -------: | ----: |
|         1 |          1 | 2019 |        2 |  2000 |
|         2 |          2 | 2018 |        1 |   800 |
|         3 |          3 | 2020 |        3 |  3000 |

After joining the tables:

| product_name | year | price |
| ------------ | ---: | ----: |
| S8           | 2019 |  2000 |
| G4           | 2018 |   800 |
| iPhone       | 2020 |  3000 |

---

## 🧠 Why Are We Using a JOIN?

The required information exists in **two different tables**.

```text
Sales
 ├── product_id
 ├── year
 └── price

Product
 ├── product_id
 └── product_name
```

We need to combine them using:

```sql
ON S.product_id = P.product_id
```

This is a common SQL pattern when information is normalized across multiple tables.

---

## 🧠 LEFT JOIN

The solution uses:

```sql
LEFT JOIN Product P
    ON S.product_id = P.product_id
```

A `LEFT JOIN` keeps all records from the `Sales` table and brings matching information from `Product`.

If there were no matching product, the `product_name` would be `NULL`.

---

## 📚 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `FROM`
* `LEFT JOIN`
* `ON`
* Table aliases
* Joining tables using a common key
* Selecting columns from multiple tables

---

## 🔑 Key Takeaways

### 1. JOIN connects related tables

```sql
ON S.product_id = P.product_id
```

The common `product_id` allows us to combine information from `Sales` and `Product`.

### 2. Table aliases

```sql
Sales S
Product P
```

Then we can reference columns using:

```sql
S.year
S.price
P.product_name
```

### 3. Columns can come from different tables

```sql
SELECT
    P.product_name,
    S.year,
    S.price
```

This is one of the most common patterns when writing SQL queries involving multiple tables.

---

## 🔗 Practice

**LeetCode:** Product Sales Analysis I

https://leetcode.com/problems/product-sales-analysis-i/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELECT` `LEFT JOIN` `JOIN` `ON` `Table Alias` `Data Retrieval`
