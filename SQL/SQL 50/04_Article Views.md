# 1148. Article Views I

## 📌 Problem

Given a `Views` table, find the IDs of authors who have viewed **their own articles**.

An author has viewed their own article when:

```text
author_id = viewer_id
```

The result should contain:

* The author ID as `id`
* Each ID should appear only once
* Results should be sorted in ascending order

---

## 🎯 Requirement

Return the distinct `author_id` values where the author is also the viewer.

In other words:

```sql
author_id = viewer_id
```

---

## 💡 Solution

```sql
SELECT
    DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY author_id ASC;
```

---

## 🔍 Explanation

### 1. Select the author ID

```sql
SELECT author_id
```

We need the ID of the author who viewed their own article.

---

### 2. Rename the column

The expected output column name is `id`, so we use:

```sql
author_id AS id
```

`AS` is used to give a column an alias.

---

### 3. Find authors who viewed their own articles

```sql
WHERE author_id = viewer_id
```

This compares two columns from the same row.

For example:

| author_id | viewer_id |
| --------: | --------: |
|         1 |         1 |
|         2 |         3 |
|         4 |         4 |

The first and third rows qualify because:

```text
1 = 1 ✅
2 = 3 ❌
4 = 4 ✅
```

---

### 4. Remove duplicates

An author may have viewed multiple articles or the same article may appear in multiple rows.

For example:

| author_id | viewer_id |
| --------: | --------: |
|         1 |         1 |
|         1 |         1 |
|         2 |         2 |

Without `DISTINCT`, the result could contain:

```text
1
1
2
```

Using:

```sql
DISTINCT author_id
```

returns:

```text
1
2
```

---

### 5. Sort the result

```sql
ORDER BY author_id ASC
```

`ASC` means **ascending order**.

The smallest ID appears first.

---

## 🧪 Example

Suppose the `Views` table contains:

| article_id | author_id | viewer_id | view_date  |
| ---------: | --------: | --------: | ---------- |
|          1 |         3 |         5 | 2019-08-01 |
|          1 |         3 |         3 | 2019-08-02 |
|          2 |         7 |         7 | 2019-08-01 |
|          2 |         7 |         5 | 2019-08-02 |
|          3 |         4 |         4 | 2019-08-03 |

Rows where the author viewed their own article:

```text
author_id = viewer_id
```

are:

```text
3 = 3
7 = 7
4 = 4
```

Therefore, the result is:

| id |
| -: |
|  3 |
|  4 |
|  7 |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `SELECT`
* `DISTINCT`
* Column aliases using `AS`
* `WHERE`
* Comparing two columns
* `ORDER BY`
* `ASC`
* Removing duplicate records

---

## 📚 Key Takeaways

### 1. `DISTINCT` removes duplicates

```sql
SELECT DISTINCT author_id
FROM Views;
```

Use it when the same value can appear multiple times but should appear only once in the result.

### 2. You can compare two columns

```sql
WHERE author_id = viewer_id
```

This checks the relationship between values within each row.

### 3. Column aliases

```sql
author_id AS id
```

This changes the name of the column in the query output without changing the actual table.

### 4. Sorting

```sql
ORDER BY author_id ASC
```

Sorts the result from smallest to largest.

---

## 🔗 Practice

**LeetCode:** Article Views I

https://leetcode.com/problems/article-views-i/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELECT` `DISTINCT` `WHERE` `ORDER BY` `Alias` `Filtering`
