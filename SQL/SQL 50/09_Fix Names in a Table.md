# 1667. Fix Names in a Table

## 📌 Problem

Given a `Users` table, fix the format of each user's name.

The name should follow this format:

* The **first character** should be uppercase.
* All remaining characters should be lowercase.

The result should also be sorted by `user_id` in ascending order.

---

## 🎯 Requirement

For example:

```text
alice → Alice
BOB → Bob
jOhN → John
```

Return:

* `user_id`
* Correctly formatted `name`

---

## 🧠 Intuition

First, I need to separate the first character from the remaining characters.

Then:

* Convert the first character to uppercase.
* Convert the remaining characters to lowercase.
* Combine both parts together.

For example:

```text
aLICE
```

can be divided into:

```text
a    → first character
LICE → remaining characters
```

Then:

```text
a    → A
LICE → lice
```

Finally, combine them:

```text
A + lice = Alice
```

---

## 💡 Approach

1. Use `LEFT(name, 1)` to get the first character.
2. Use `UPPER()` to convert the first character to uppercase.
3. Use `SUBSTRING(name, 2)` to get everything after the first character.
4. Use `LOWER()` to convert the remaining characters to lowercase.
5. Use `CONCAT()` to combine both parts.
6. Use `ORDER BY user_id` to sort the result.

---

## 🔍 Solution

```sql
SELECT
    user_id,
    CONCAT(
        UPPER(LEFT(name, 1)),
        LOWER(SUBSTRING(name, 2))
    ) AS name
FROM Users
ORDER BY user_id;
```

---

## 🔎 Query Breakdown

### 1. Get the first character

```sql
LEFT(name, 1)
```

Returns the first character of the name.

Example:

```text
alice
 ↓
a
```

---

### 2. Convert the first character to uppercase

```sql
UPPER(LEFT(name, 1))
```

Example:

```text
a → A
```

---

### 3. Get the remaining characters

```sql
SUBSTRING(name, 2)
```

This starts from the second character.

Example:

```text
alice
 ↓
lice
```

---

### 4. Convert the remaining characters to lowercase

```sql
LOWER(SUBSTRING(name, 2))
```

Example:

```text
LICE → lice
```

---

### 5. Combine both parts

```sql
CONCAT(
    UPPER(LEFT(name, 1)),
    LOWER(SUBSTRING(name, 2))
)
```

For:

```text
aLICE
```

the result becomes:

```text
Alice
```

---

### 6. Sort by user ID

```sql
ORDER BY user_id;
```

By default, `ORDER BY` sorts in ascending order.

So:

```text
3
1
2
```

becomes:

```text
1
2
3
```

---

## 🧪 Example

### Input

| user_id | name  |
| ------: | ----- |
|       1 | aLICE |
|       2 | bOB   |
|       3 | jOhN  |

### Output

| user_id | name  |
| ------: | ----- |
|       1 | Alice |
|       2 | Bob   |
|       3 | John  |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `CONCAT()`
* `UPPER()`
* `LOWER()`
* `LEFT()`
* `SUBSTRING()`
* `AS`
* `ORDER BY`
* String manipulation
* Combining multiple SQL functions

---

## 📚 Key Takeaways

### `LEFT()`

Gets characters from the beginning of a string:

```sql
LEFT(name, 1)
```

### `SUBSTRING()`

Gets part of a string starting from a specified position:

```sql
SUBSTRING(name, 2)
```

### `UPPER()`

Converts text to uppercase:

```sql
UPPER('alice')
```

Result:

```text
ALICE
```

### `LOWER()`

Converts text to lowercase:

```sql
LOWER('ALICE')
```

Result:

```text
alice
```

### `CONCAT()`

Combines multiple strings:

```sql
CONCAT('A', 'lice')
```

Result:

```text
Alice
```

---

## 🔗 Practice

**LeetCode:** Fix Names in a Table

https://leetcode.com/problems/fix-names-in-a-table/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `String Functions` `CONCAT` `UPPER` `LOWER` `LEFT` `SUBSTRING` `ORDER BY`
