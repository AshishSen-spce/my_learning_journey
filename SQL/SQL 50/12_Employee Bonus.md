# 577. Employee Bonus

## 📌 Problem

Given two tables:

* `Employee`
* `Bonus`

Find the employees whose bonus is **less than 1000** or who **do not have a bonus**.

The result should contain:

* Employee name
* Bonus amount

---

## 🧠 Intuition

This is a very straightforward problem because most of the solution is directly given in the question.

We need to:

1. Get the employee name from the `Employee` table.
2. Get the bonus from the `Bonus` table.
3. Connect both tables using `empId`.
4. Keep employees whose bonus is less than `1000`.
5. Also include employees who don't have any bonus.

Since employees without a bonus should also be included, we need to use a `LEFT JOIN`.

---

## 💡 Approach

### Step 1 — Join Employee and Bonus

Use:

```sql
LEFT JOIN Bonus b
    ON b.empId = e.empId
```

This keeps every employee, even when they don't have a matching record in the `Bonus` table.

---

### Step 2 — Filter the bonus

The question asks for employees where:

```text
bonus < 1000
```

So:

```sql
b.bonus < 1000
```

But employees without a bonus should also be included.

For those employees:

```text
b.bonus = NULL
```

Therefore, we also need:

```sql
b.bonus IS NULL
```

---

### Step 3 — Combine both conditions

Since either condition can be true, we use `OR`:

```sql
WHERE b.bonus < 1000
   OR b.bonus IS NULL
```

---

## 💻 Solution

```sql id="v7j3kx"
SELECT
    e.name,
    b.bonus
FROM Employee e
LEFT JOIN Bonus b
    ON b.empId = e.empId
WHERE b.bonus < 1000
   OR b.bonus IS NULL;
```

---

## 🔍 Query Breakdown

### 1. Select employee name and bonus

```sql id="x8p2qk"
SELECT
    e.name,
    b.bonus
```

The required output contains the employee's name and bonus.

---

### 2. Start with Employee

```sql id="r9c4mz"
FROM Employee e
```

We start with `Employee` because we need to consider all employees.

---

### 3. LEFT JOIN Bonus

```sql id="k2d7vx"
LEFT JOIN Bonus b
    ON b.empId = e.empId
```

The `empId` column connects the two tables.

A `LEFT JOIN` ensures that employees without a bonus are still included.

For example:

```text id="a6f1pz"
Employee
empId = 1

Bonus
No record for empId = 1
```

After the `LEFT JOIN`:

```text id="4g9m2c"
name  → Employee name
bonus → NULL
```

---

### 4. Apply the filter

```sql id="w3n8qt"
WHERE b.bonus < 1000
   OR b.bonus IS NULL
```

This gives us two categories:

```text id="5jv2bn"
Bonus < 1000
        OR
No Bonus
```

---

## 🧪 Example

### Employee

| empId | name    |
| ----: | ------- |
|     1 | Alice   |
|     2 | Bob     |
|     3 | Charlie |
|     4 | David   |

### Bonus

| empId | bonus |
| ----: | ----: |
|     1 |   500 |
|     2 |  1500 |
|     3 |   800 |

After the `LEFT JOIN`:

| name    | bonus |
| ------- | ----: |
| Alice   |   500 |
| Bob     |  1500 |
| Charlie |   800 |
| David   |  NULL |

After applying:

```sql
WHERE bonus < 1000
   OR bonus IS NULL
```

Result:

| name    | bonus |
| ------- | ----: |
| Alice   |   500 |
| Charlie |   800 |
| David   |  NULL |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `LEFT JOIN`
* `ON`
* `WHERE`
* `OR`
* `IS NULL`
* Filtering joined data
* Handling missing records
* Table aliases

---

## 📚 Key Takeaways

### 1. LEFT JOIN keeps unmatched employees

```sql id="q3x8nm"
LEFT JOIN Bonus b
    ON b.empId = e.empId
```

This is important because an employee may not have a record in the `Bonus` table.

---

### 2. NULL needs special handling

You cannot write:

```sql id="t7m4pc"
b.bonus = NULL
```

Instead use:

```sql id="z5k9rw"
b.bonus IS NULL
```

---

### 3. OR handles multiple valid conditions

```sql id="k6p1yd"
WHERE b.bonus < 1000
   OR b.bonus IS NULL
```

This means an employee is included when either:

* Their bonus is less than `1000`
* They don't have a bonus

---

## 🔗 Practice

**LeetCode:** Employee Bonus

https://leetcode.com/problems/employee-bonus/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `LEFT JOIN` `WHERE` `OR` `IS NULL` `NULL` `Filtering`
