# 1527. Patients With a Condition

## Problem

Write a SQL query to find patients who have a condition that starts with `DIAB1`.

The condition can appear:

* At the beginning of the `conditions` column, or
* After a space when there are multiple conditions.

We need to return:

* `patient_id`
* `patient_name`
* `conditions`

---

## Intuition

The important thing to understand is that `DIAB1` should be treated as a **complete condition code**.

For example:

```text
DIAB100
```

should **not** match if we are looking for the condition `DIAB1`.

But these should match:

```text
DIAB1
DIAB1 ABC
ABC DIAB1
```

So we need two cases:

1. `DIAB1` appears at the beginning:

   ```sql
   conditions LIKE 'DIAB1%'
   ```

2. `DIAB1` appears after a space:

   ```sql
   conditions LIKE '% DIAB1%'
   ```

Then we combine both conditions using `OR`.

---

## Approach

### Step 1 — Check if DIAB1 is at the beginning

```sql
conditions LIKE 'DIAB1%'
```

This matches values such as:

```text
DIAB1
DIAB1 XYZ
DIAB1 ABC XYZ
```

---

### Step 2 — Check if DIAB1 appears after a space

```sql
conditions LIKE '% DIAB1%'
```

This matches values such as:

```text
ABC DIAB1
ABC XYZ DIAB1
```

The space before `DIAB1` is important because it ensures that `DIAB1` starts as a separate condition.

---

### Step 3 — Combine both conditions

```sql
WHERE
    conditions LIKE 'DIAB1%'
    OR conditions LIKE '% DIAB1%'
```

This covers both possible positions.

---

## Solution

```sql
SELECT
    patient_id,
    patient_name,
    conditions
FROM Patients
WHERE conditions LIKE 'DIAB1%'
   OR conditions LIKE '% DIAB1%';
```

---

## Example

Suppose the table contains:

```text
patient_id   patient_name   conditions
----------   ------------   ----------------
1            John           DIAB1
2            Alice          ABC DIAB1
3            Bob            DIAB2
4            David          XYZ DIAB1 ABC
5            Mike           DIAB100
```

The matching records are:

```text
John    → DIAB1
Alice   → ABC DIAB1
David   → XYZ DIAB1 ABC
```

`Mike` is not included because:

```text
DIAB100
```

is not the condition `DIAB1`.

---

## Why Not Just Use `LIKE '%DIAB1%'`?

We might think this is enough:

```sql
WHERE conditions LIKE '%DIAB1%'
```

But this can incorrectly match values such as:

```text
DIAB100
```

because `DIAB1` is simply a substring of `DIAB100`.

The required logic is therefore more precise:

```sql
conditions LIKE 'DIAB1%'
OR conditions LIKE '% DIAB1%'
```

---

## Query Flow

```text
Patients
   ↓
Check conditions
   ↓
DIAB1 at beginning?
   │
   └── Yes → Keep
   ↓
DIAB1 after a space?
   │
   └── Yes → Keep
   ↓
Return patient details
```

---

## Concepts Learned

* `LIKE`
* Wildcards `%`
* `OR`
* String pattern matching
* Searching for a value at the beginning of a string
* Searching for a value after a delimiter
* Avoiding false substring matches

---

## Key Takeaway

When searching for a **specific word/code inside a space-separated column**, be careful with:

```sql
LIKE '%value%'
```

because it can match the value as part of another word.

Instead, consider the possible positions of the value.

For this problem:

```sql
-- At the beginning
conditions LIKE 'DIAB1%'

-- After a space
conditions LIKE '% DIAB1%'
```

This ensures that `DIAB1` is treated as a separate condition.

---

## Practice

[LeetCode — 1527. Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition/)

## Tags

`SQL` `LIKE` `String Matching` `Wildcards` `OR` `Pattern Matching`
