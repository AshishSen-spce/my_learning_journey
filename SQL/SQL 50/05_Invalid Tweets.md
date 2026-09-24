# 1683. Invalid Tweets

## 📌 Problem

Given a `Tweets` table, find the `tweet_id` of all tweets that are **invalid**.

A tweet is considered invalid when the number of characters in its `content` is **strictly greater than 15**.

---

## 🎯 Requirement

Return the `tweet_id` for every tweet where:

```text
length(content) > 15
```

---

## 💡 Solution

```sql
SELECT
    tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
```

---

## 🔍 Explanation

### 1. Select the required column

The problem asks us to return the `tweet_id`, so we use:

```sql
SELECT tweet_id
```

---

### 2. Calculate the content length

MySQL provides the `LENGTH()` function to calculate the length of a string in **bytes**.

```sql
LENGTH(content)
```

For the given LeetCode problem, the tweet content consists of standard characters, so this works for determining whether the content exceeds 15 characters.

---

### 3. Filter invalid tweets

The problem defines an invalid tweet as one whose content has more than 15 characters:

```sql
WHERE LENGTH(content) > 15
```

The `>` operator means **strictly greater than**.

Therefore:

```text
15 characters → Valid
16 characters → Invalid
20 characters → Invalid
```

---

## 🧪 Example

Suppose the `Tweets` table contains:

| tweet_id | content                   |
| -------: | ------------------------- |
|        1 | Let us code               |
|        2 | This is a very long tweet |
|        3 | SQL is fun                |
|        4 | Learning SQL and Python   |

If the content length is greater than 15, the tweet is invalid.

The result would contain the corresponding IDs:

| tweet_id |
| -------: |
|        2 |
|        4 |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `FROM`
* `WHERE`
* String functions
* `LENGTH()`
* Comparison operators
* Filtering rows based on calculated values

---

## 📚 Key Takeaways

### 1. `LENGTH()` function

```sql
LENGTH(content)
```

Returns the length of the string.

Example:

```sql
SELECT LENGTH('Hello');
```

Result:

```text
5
```

---

### 2. Using a function inside `WHERE`

SQL functions can be used directly inside filtering conditions:

```sql
WHERE LENGTH(content) > 15
```

The database calculates the length for each row and keeps only the rows that satisfy the condition.

---

### 3. Understanding `>`

The condition:

```sql
LENGTH(content) > 15
```

does **not** include tweets with exactly 15 characters.

It only includes values greater than 15.

```text
15 → ❌
16 → ✅
17 → ✅
```

---

## 🔗 Practice

**LeetCode:** Invalid Tweets

https://leetcode.com/problems/invalid-tweets/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELECT` `WHERE` `LENGTH` `String Functions` `Filtering`
