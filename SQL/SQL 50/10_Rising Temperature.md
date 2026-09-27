# 197. Rising Temperature

## 📌 Problem

Given a `Weather` table, find the `id` of dates where the temperature was higher than the temperature of the **previous day**.

A date should be included only when:

1. There is a weather record exactly one day before it.
2. The current day's temperature is higher than the previous day's temperature.

---

## 📊 Table

### `Weather`

| Column        | Type |
| ------------- | ---- |
| `id`          | int  |
| `recordDate`  | date |
| `temperature` | int  |

---

## 🧠 Intuition

The first thought is that I need to compare the temperature of one day with the temperature of the **previous day**.

Since both the current day's and previous day's data are available in the same `Weather` table, I can join the table with itself.

So I use:

```text id="2w3n4k"
Weather wt1 → Current day
Weather wt2 → Previous day
```

Then I can compare their temperatures.

To make sure `wt2` represents exactly the previous day, I use `DATEDIFF()`:

```sql id="p8w7q1"
DATEDIFF(wt1.recordDate, wt2.recordDate) = 1
```

After matching the two dates, I check whether the current day's temperature is greater:

```sql id="d6x4c2"
wt1.temperature > wt2.temperature
```

If both conditions are true, I return the current day's `id`.

---

## 💡 Approach

1. Use the `Weather` table twice by creating two aliases:

   * `wt1` for the current day
   * `wt2` for the previous day

2. Use a `JOIN` to compare records from the same table.

3. Use `DATEDIFF()` to make sure the two records are exactly one day apart.

4. Compare the temperatures:

   ```sql
   wt1.temperature > wt2.temperature
   ```

5. Return the `id` of the current day.

---

## 💻 Solution

```sql id="6x2k91"
SELECT
    wt1.id
FROM Weather wt1
JOIN Weather wt2
    ON DATEDIFF(wt1.recordDate, wt2.recordDate) = 1
WHERE wt1.temperature > wt2.temperature;
```

---

## 🔍 Query Breakdown

### 1. Use the Weather table twice

```sql id="q1m3x8"
FROM Weather wt1
JOIN Weather wt2
```

This is called a **SELF JOIN** because we are joining the `Weather` table with itself.

We use two aliases:

```text id="0o7g2f"
wt1 → Current day's record
wt2 → Previous day's record
```

---

### 2. Find the previous day

```sql id="n8x4f1"
ON DATEDIFF(wt1.recordDate, wt2.recordDate) = 1
```

`DATEDIFF()` calculates the difference between two dates.

For example:

```text id="j5p7c2"
2026-09-10 - 2026-09-09 = 1
```

Therefore:

```sql id="c7z1m4"
DATEDIFF(wt1.recordDate, wt2.recordDate) = 1
```

ensures that `wt2` represents the day immediately before `wt1`.

---

### 3. Compare temperatures

```sql id="k9v2s6"
WHERE wt1.temperature > wt2.temperature
```

This checks whether the current day's temperature is higher than the previous day's temperature.

For example:

```text id="4q8m2n"
Previous day → 20°C
Current day  → 25°C

25 > 20 → TRUE
```

So the current day's `id` is returned.

---

## 🧪 Example

### Weather

| id | recordDate | temperature |
| -: | ---------- | ----------: |
|  1 | 2015-01-01 |          10 |
|  2 | 2015-01-02 |          25 |
|  3 | 2015-01-03 |          20 |
|  4 | 2015-01-04 |          30 |

### Compare consecutive days

| Current Date | Current Temp | Previous Date | Previous Temp | Rising? |
| ------------ | -----------: | ------------- | ------------: | :-----: |
| 2015-01-02   |           25 | 2015-01-01    |            10 |    ✅    |
| 2015-01-03   |           20 | 2015-01-02    |            25 |    ❌    |
| 2015-01-04   |           30 | 2015-01-03    |            20 |    ✅    |

### Result

| id |
| -: |
|  2 |
|  4 |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `SELF JOIN`
* `JOIN`
* `DATEDIFF()`
* Table aliases
* Comparing rows within the same table
* Date comparison
* `WHERE`
* Comparison operators

---

## 📚 Key Takeaways

### 1. SELF JOIN

A self join means joining a table with itself.

```sql id="8j4p3m"
FROM Weather wt1
JOIN Weather wt2
```

This is useful when we need to compare one row with another row from the same table.

---

### 2. DATEDIFF()

`DATEDIFF()` returns the difference between two dates.

```sql id="v5c8q2"
DATEDIFF(date1, date2)
```

In this problem:

```sql id="x9k3m1"
DATEDIFF(wt1.recordDate, wt2.recordDate) = 1
```

means `wt1` is exactly one day after `wt2`.

---

### 3. Compare current and previous values

Once the rows are matched:

```sql id="n2f7c4"
wt1.temperature > wt2.temperature
```

checks whether the temperature increased.

---

## 🔗 Practice

**LeetCode:** Rising Temperature

https://leetcode.com/problems/rising-temperature/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `SELF JOIN` `JOIN` `DATEDIFF` `Date Functions` `Table Alias` `Comparison`
