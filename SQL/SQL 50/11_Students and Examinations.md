# 1280. Students and Examinations

## 📌 Problem

Given three tables:

* `Students`
* `Subjects`
* `Examinations`

We need to find how many times each student attended an exam for each subject.

The result should contain:

* `student_id`
* `student_name`
* `subject_name`
* Number of exams attended by the student for that subject

Even if a student did **not attend any exam** for a particular subject, that student-subject combination should still appear with `0`.

---

## 🧠 Intuition

The first important thing to understand is the relationship between `Students` and `Subjects`.

One student can take multiple subjects, and one subject can belong to multiple students.

So we need **every possible student-subject combination** first.

For example:

```text
Students
---------
Student 1
Student 2

Subjects
--------
Math
Science
```

We need:

```text
Student 1 → Math
Student 1 → Science
Student 2 → Math
Student 2 → Science
```

This is why we use a:

```sql
CROSS JOIN
```

After creating all student-subject combinations, we use `LEFT JOIN` with the `Examinations` table to find whether that student actually attended an exam for that subject.

---

## 💡 Approach

### Step 1 — Create all student-subject combinations

Use:

```sql
CROSS JOIN Subjects sj
```

This creates every possible combination between students and subjects.

If there are:

```text
4 Students
3 Subjects
```

then the result contains:

```text
4 × 3 = 12 combinations
```

---

### Step 2 — Match examination records

After creating all combinations, join the `Examinations` table:

```sql
LEFT JOIN Examinations e
    ON s.student_id = e.student_id
   AND sj.subject_name = e.subject_name
```

There are two matching conditions:

```text
Student must match
AND
Subject must match
```

This allows us to find the exams attended by that particular student for that particular subject.

---

### Step 3 — Use LEFT JOIN

We use `LEFT JOIN` instead of `INNER JOIN` because we also need students who attended **zero exams** for a subject.

For example:

```text
Student 1 → Math → 3 exams
Student 1 → Science → 0 exams
```

The `Student 1 + Science` row must still appear.

Because it is a `LEFT JOIN`, the missing examination record becomes `NULL`.

---

### Step 4 — Count the examinations

We use:

```sql
COUNT(e.student_id)
```

Because `COUNT(column)` does not count `NULL` values.

Therefore:

```text
3 matching exam rows → COUNT = 3
No matching exam row → COUNT = 0
```

This is useful for this problem.

---

### Step 5 — GROUP BY

We need one result for each:

```text
student + subject
```

So we group by:

```sql
GROUP BY
    s.student_id,
    s.student_name,
    sj.subject_name
```

---

### Step 6 — ORDER BY

Finally, we sort the result by:

```sql
ORDER BY
    s.student_id,
    s.student_name,
    sj.subject_name
```

---

## 💻 Solution

```sql
SELECT
    s.student_id,
    s.student_name,
    sj.subject_name,
    COUNT(e.student_id) AS attended_exams
FROM Students s
CROSS JOIN Subjects sj
LEFT JOIN Examinations e
    ON s.student_id = e.student_id
   AND sj.subject_name = e.subject_name
GROUP BY
    s.student_id,
    s.student_name,
    sj.subject_name
ORDER BY
    s.student_id,
    s.student_name,
    sj.subject_name;
```

---

## 🔍 Query Breakdown

### 1. Students

```sql
FROM Students s
```

This is our starting table.

---

### 2. CROSS JOIN Subjects

```sql
CROSS JOIN Subjects sj
```

This creates every possible student-subject combination.

Conceptually:

```text
Students × Subjects
```

---

### 3. LEFT JOIN Examinations

```sql
LEFT JOIN Examinations e
    ON s.student_id = e.student_id
   AND sj.subject_name = e.subject_name
```

This finds the examination records belonging to that specific:

```text
Student + Subject
```

combination.

---

### 4. COUNT()

```sql
COUNT(e.student_id) AS attended_exams
```

Counts how many examination records matched.

Because `e.student_id` is `NULL` when no exam exists, `COUNT()` returns `0` for that combination.

---

## 🧪 Example

### Students

| student_id | student_name |
| ---------: | ------------ |
|          1 | Alice        |
|          2 | Bob          |

### Subjects

| subject_name |
| ------------ |
| Math         |
| Science      |

### Examinations

| student_id | subject_name |
| ---------: | ------------ |
|          1 | Math         |
|          1 | Math         |
|          1 | Science      |
|          2 | Math         |

---

### Step 1 — CROSS JOIN

We first get:

| student_id | student_name | subject_name |
| ---------: | ------------ | ------------ |
|          1 | Alice        | Math         |
|          1 | Alice        | Science      |
|          2 | Bob          | Math         |
|          2 | Bob          | Science      |

---

### Step 2 — Match Examinations

After the `LEFT JOIN`:

```text
Alice + Math    → 2 exams
Alice + Science → 1 exam
Bob + Math      → 1 exam
Bob + Science   → 0 exams
```

---

### Final Result

| student_id | student_name | subject_name | attended_exams |
| ---------: | ------------ | ------------ | -------------: |
|          1 | Alice        | Math         |              2 |
|          1 | Alice        | Science      |              1 |
|          2 | Bob          | Math         |              1 |
|          2 | Bob          | Science      |              0 |

---

## 🧠 SQL Concepts Learned

This problem helps understand:

* `CROSS JOIN`
* `LEFT JOIN`
* Multiple JOIN conditions
* `COUNT()`
* `GROUP BY`
* `ORDER BY`
* One-to-many relationships
* Many-to-many relationships
* Handling missing records
* `NULL` behavior with `COUNT()`

---

## 📚 Key Takeaways

### 1. CROSS JOIN creates all combinations

```sql
Students
CROSS JOIN
Subjects
```

If there are:

```text
N students
M subjects
```

the result contains:

```text
N × M rows
```

This is useful when the requirement says **every student should be shown for every subject**.

---

### 2. LEFT JOIN preserves missing combinations

```sql
LEFT JOIN Examinations
```

ensures that even students who did not attend an exam for a subject remain in the result.

---

### 3. COUNT(column) ignores NULL

```sql
COUNT(e.student_id)
```

If there is no matching examination:

```text
e.student_id = NULL
```

and `COUNT()` does not count that value.

Therefore, the result becomes:

```text
0
```

---

### 4. JOIN conditions can contain multiple columns

```sql
ON s.student_id = e.student_id
AND sj.subject_name = e.subject_name
```

Both conditions must match.

This is important when a single key is not enough to identify the relationship between the records.

---

## 🔗 Practice

**LeetCode:** Students and Examinations

https://leetcode.com/problems/students-and-examinations/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`SQL` `MySQL` `CROSS JOIN` `LEFT JOIN` `COUNT` `GROUP BY` `ORDER BY` `NULL` `Relationships`
