# 595. Big Countries

## 📌 Problem

Given a `World` table, find the countries that are considered **big**.

A country is considered big if:

* Its area is at least `3,000,000`, **OR**
* Its population is at least `25,000,000`.

### Table: `World`

| Column       | Type    |
| ------------ | ------- |
| `name`       | varchar |
| `continent`  | varchar |
| `area`       | int     |
| `population` | int     |
| `gdp`        | bigint  |

---

## 🎯 Requirement

Return the following information for all big countries:

* `name`
* `population`
* `area`

A country should be included when:

```text
area >= 3,000,000
OR
population >= 25,000,000
```

---

## 💡 Solution

```sql
SELECT
    name,
    population,
    area
FROM World
WHERE area >= 3000000
   OR population >= 25000000;
```

---

## 🔍 Explanation

We need to check two different conditions.

### Condition 1 — Large Area

```sql
area >= 3000000
```

This selects countries whose area is at least `3,000,000`.

The `>=` operator means **greater than or equal to**.

For example:

```text
area = 4,000,000 → included
area = 3,000,000 → included
area = 2,500,000 → not included
```

### Condition 2 — Large Population

```sql
population >= 25000000
```

This selects countries whose population is at least `25,000,000`.

For example:

```text
population = 30,000,000 → included
population = 25,000,000 → included
population = 20,000,000 → not included
```

---

## 🧠 Why `OR` is Used

The problem says a country is big if it satisfies **either** condition.

Therefore, we use:

```sql
WHERE area >= 3000000
   OR population >= 25000000
```

This means:

```text
Large Area
     OR
Large Population
```

A country does **not** need to satisfy both conditions.

---

## 🧪 Example

Suppose the table contains:

| name      | population |      area |
| --------- | ---------: | --------: |
| Country A | 10,000,000 | 4,000,000 |
| Country B | 30,000,000 | 1,000,000 |
| Country C | 20,000,000 | 2,000,000 |
| Country D | 25,000,000 | 3,000,000 |

The result will be:

| name      | population |      area |
| --------- | ---------: | --------: |
| Country A | 10,000,000 | 4,000,000 |
| Country B | 30,000,000 | 1,000,000 |
| Country D | 25,000,000 | 3,000,000 |

### Why?

* Country A → area condition satisfied ✅
* Country B → population condition satisfied ✅
* Country C → neither condition satisfied ❌
* Country D → both conditions satisfied ✅

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `FROM`
* `WHERE`
* `OR`
* Comparison operators
* `>=`
* Filtering rows
* Multiple conditions

---

## 📚 Key Takeaways

### 1. `>=` means greater than or equal to

```sql
area >= 3000000
```

This includes exactly `3,000,000` as well as values greater than it.

### 2. `OR` means either condition can be true

```sql
WHERE condition_1
   OR condition_2
```

If either condition is true, the row is returned.

### 3. Multiple columns can be returned

```sql
SELECT
    name,
    population,
    area
```

Only the columns required by the problem are selected.

---

## 🔗 Practice

**LeetCode:** Big Countries

https://leetcode.com/problems/big-countries/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELECT` `WHERE` `OR` `Comparison` `Filtering`
