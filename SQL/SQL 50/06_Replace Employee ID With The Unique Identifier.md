# 1378. Replace Employee ID With The Unique Identifier

## 📌 Problem

Given two tables:

* `Employees`
* `EmployeeUNI`

Return the `unique_id` and `name` of every employee.

If an employee does not have a corresponding `unique_id`, the result should still contain the employee, with `unique_id` as `NULL`.

---

## 📊 Tables

### `Employees`

| Column | Type    |
| ------ | ------- |
| `id`   | int     |
| `name` | varchar |

### `EmployeeUNI`

| Column      | Type |
| ----------- | ---- |
| `id`        | int  |
| `unique_id` | int  |

The `id` column connects the two tables.

---

## 🎯 Requirement

Return:

* `unique_id`
* `name`

for **all employees**.

If an employee has a matching record in `EmployeeUNI`, return their `unique_id`.

If there is no matching record, return `NULL`.

---

## 💡 Solution

```sql
SELECT
    eu.unique_id AS unique_id,
    e.name AS name
FROM Employees e
LEFT JOIN EmployeeUNI eu
    ON e.id = eu.id;
```

---

## 🔍 Explanation

### 1. Start with the Employees table

```sql
FROM Employees e
```

We use `Employees` as the main table because we need information for **every employee**.

The alias:

```sql
e
```

is used as a shorter name for the `Employees` table.

---

### 2. Use a LEFT JOIN

```sql
LEFT JOIN EmployeeUNI eu
```

A `LEFT JOIN` returns:

* Every row from the left table (`Employees`)
* Matching rows from the right table (`EmployeeUNI`)
* `NULL` when there is no matching row

This is important because the problem requires **all employees**, even if they don't have a unique ID.

---

### 3. Match the employees

```sql
ON e.id = eu.id
```

This tells SQL how the two tables should be joined.

For example:

```text
Employees.id = EmployeeUNI.id
```

If the IDs match, SQL combines the information from both tables.

---

## 🧪 Example

### Employees

| id | name    |
| -: | ------- |
|  1 | Alice   |
|  7 | Bob     |
| 11 | Meir    |
| 90 | Winston |

### EmployeeUNI

| id | unique_id |
| -: | --------: |
|  3 |         1 |
| 11 |         2 |
| 90 |         3 |

After the `LEFT JOIN`:

| unique_id | name    |
| --------: | ------- |
|      NULL | Alice   |
|      NULL | Bob     |
|         2 | Meir    |
|         3 | Winston |

Alice and Bob don't have matching records in `EmployeeUNI`, so their `unique_id` is `NULL`.

---

## 🧠 Why LEFT JOIN Instead of INNER JOIN?

If we used:

```sql
INNER JOIN EmployeeUNI eu
    ON e.id = eu.id
```

employees without a matching record in `EmployeeUNI` would be removed.

For example:

```text
Employees
1 Alice
7 Bob
11 Meir
90 Winston

EmployeeUNI
11 → 2
90 → 3
```

With `INNER JOIN`:

```text
Meir
Winston
```

Only matching employees would remain.

But the problem requires **all employees**, so we use:

```sql
LEFT JOIN
```

---

## 📚 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `FROM`
* `LEFT JOIN`
* `ON`
* Table aliases
* Matching records between tables
* `NULL` values
* Difference between `LEFT JOIN` and `INNER JOIN`

---

## 🔑 Key Takeaways

### 1. LEFT JOIN keeps all rows from the left table

```sql
FROM Employees e
LEFT JOIN EmployeeUNI eu
    ON e.id = eu.id
```

Even if there is no matching row in `EmployeeUNI`, the employee remains in the result.

---

### 2. Table aliases make queries easier to read

Instead of:

```sql
Employees.id
EmployeeUNI.unique_id
Employees.name
```

we can use:

```sql
e.id
eu.unique_id
e.name
```

where:

```text
e  → Employees
eu → EmployeeUNI
```

---

### 3. Missing matches become NULL

When there is no matching `EmployeeUNI` record:

```text
eu.unique_id → NULL
```

This is expected behavior for a `LEFT JOIN`.

---

## 🔗 Practice

**LeetCode:** Replace Employee ID With The Unique Identifier

https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELECT` `LEFT JOIN` `JOIN` `ON` `NULL` `Table Alias`
