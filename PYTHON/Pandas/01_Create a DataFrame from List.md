# 2877. Create a DataFrame from List

## 📌 Problem

Given a list of lists containing student information, create a **Pandas DataFrame** from the provided data.

The DataFrame should contain the required column names.

---

## 🧠 Intuition

The first thought is that the given data is already available as a list of lists.

So instead of manually creating each row, we can directly pass the list to `pandas.DataFrame()`.

We just need to provide the column names using the `columns` parameter.

The basic idea is:

```text
List of Lists
      ↓
pd.DataFrame()
      ↓
DataFrame with column names
```

---

## 💡 Approach

1. Import the `pandas` library.
2. Pass the given list to `pd.DataFrame()`.
3. Provide the required column names using the `columns` parameter.
4. Return the resulting DataFrame.

---

## 💻 Solution

```python
import pandas as pd

def createDataframe(student_data: List[List[int]]) -> pd.DataFrame:
    return pd.DataFrame(
        student_data,
        columns=["student_id", "age"]
    )
```

---

## 🔍 Code Breakdown

### 1. Import Pandas

```python
import pandas as pd
```

`pandas` is a Python library commonly used for:

* Data manipulation
* Data analysis
* Working with tabular data
* DataFrames

We use `pd` as the standard alias for Pandas.

---

### 2. Create the DataFrame

```python
pd.DataFrame(
    student_data,
    columns=["student_id", "age"]
)
```

`pd.DataFrame()` converts the list of lists into a DataFrame.

For example:

```python
student_data = [
    [1, 15],
    [2, 11],
    [3, 18]
]
```

The resulting DataFrame is:

```text
   student_id  age
0           1   15
1           2   11
2           3   18
```

---

### 3. Define column names

```python
columns=["student_id", "age"]
```

This assigns names to the DataFrame columns.

Without specifying column names, Pandas would use default numeric column names:

```text
0
1
```

Using `columns` makes the DataFrame meaningful and matches the expected output.

---

## 🧪 Example

### Input

```python
student_data = [
    [1, 15],
    [2, 11],
    [3, 18]
]
```

### Output

```text
   student_id  age
0           1   15
1           2   11
2           3   18
```

---

## 🧠 Python / Pandas Concepts Learned

This problem helps understand:

* Python lists
* List of lists
* Pandas
* `pd.DataFrame()`
* DataFrame columns
* `columns` parameter
* Returning a DataFrame

---

## 📚 Key Takeaways

### 1. Creating a DataFrame

The basic syntax is:

```python
pd.DataFrame(data)
```

For example:

```python
data = [
    [1, "Ashish"],
    [2, "Rahul"]
]

df = pd.DataFrame(data)
```

---

### 2. Creating a DataFrame with column names

```python
df = pd.DataFrame(
    data,
    columns=["id", "name"]
)
```

This is a common way to convert structured Python data into tabular form.

---

### 3. DataFrame

A Pandas DataFrame is a two-dimensional table consisting of:

```text
Rows
 +
Columns
 =
DataFrame
```

It is similar to a table in a SQL database.

---

## 🔗 Practice

**LeetCode:** Create a DataFrame from List

https://leetcode.com/problems/create-a-dataframe-from-list/

---

## ✅ Difficulty

**Easy**

---

## 🏷️ Tags

`Python` `Pandas` `DataFrame` `List` `Data Manipulation`
