# Recyclable and Low Fat Products

## 📌 Problem

Given a `Products` table, find the `product_id` of all products that are:

* **Low fat**
* **Recyclable**

### Table: `Products`

| Column       | Type    |
| ------------ | ------- |
| `product_id` | int     |
| `low_fats`   | varchar |
| `recyclable` | varchar |

The columns `low_fats` and `recyclable` contain:

* `Y` → Yes
* `N` → No

---

## 🎯 Requirement

Return the `product_id` for products where:

```text
low_fats = 'Y'
AND
recyclable = 'Y'
```

---

## 💡 Solution

```sql
SELECT
    product_id
FROM Products p
WHERE p.low_fats = 'Y'
  AND p.recyclable = 'Y';
```

---

## 🔍 Explanation

We need both conditions to be true for a product.

### Condition 1

```sql
p.low_fats = 'Y'
```

Checks whether the product is low fat.

### Condition 2

```sql
p.recyclable = 'Y'
```

Checks whether the product is recyclable.

### Combining conditions

We use the `AND` operator:

```sql
WHERE p.low_fats = 'Y'
  AND p.recyclable = 'Y'
```

`AND` means **both conditions must be satisfied**.

---

## 🧪 Example

Suppose the table contains:

| product_id | low_fats | recyclable |
| ---------: | :------: | :--------: |
|          1 |     Y    |      Y     |
|          2 |     Y    |      N     |
|          3 |     N    |      Y     |
|          4 |     Y    |      Y     |

The result will be:

| product_id |
| ---------: |
|          1 |
|          4 |

Products `1` and `4` satisfy **both** requirements.

---

## 🧠 SQL Concepts Learned

This problem is useful for understanding:

* `SELECT`
* `FROM`
* `WHERE`
* `AND`
* Filtering rows
* String comparison
* Table aliases

---

## 📚 Key Takeaway

When a SQL problem asks for records that must satisfy **multiple conditions**, use the `AND` operator.

General pattern:

```sql
SELECT column_name
FROM table_name
WHERE condition_1
  AND condition_2;
```

For this problem:

```sql
WHERE low_fats = 'Y'
  AND recyclable = 'Y'
```

---

## 🔗 Practice

**LeetCode:** Recyclable and Low Fat Products

https://leetcode.com/problems/recyclable-and-low-fat-products/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELECT` `WHERE` `AND` `Filtering`
