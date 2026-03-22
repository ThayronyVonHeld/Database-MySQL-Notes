# 📚 Lesson 10 — Advanced SELECT (Part 2)

---

## 🎯 Lesson Objectives

* Use the **LIKE** operator for pattern searches
* Work with wildcard characters (**%** and **_**)
* Apply **NOT LIKE** for exclusions
* Use **DISTINCT** to eliminate duplicates
* Understand and apply **aggregation functions**
* Perform more intelligent and efficient queries

---

# 🔍 The LIKE Operator

So far, we have used exact comparisons with `=`.

But in many cases, we want to search for **text patterns**, such as:

```text
- Names that start with "A"
- Emails that end with ".com"
- Words that contain a specific letter
```

For that, we use the operator:

```sql
LIKE
```

---

# 🔤 Wildcard Characters

### 1. Percentage (%) — Any Sequence of Characters

Represents:

```text
0, 1, or multiple characters
```

### Examples

Starts with "A":

```sql
SELECT * FROM alunos
WHERE nome LIKE 'A%';
```

Ends with "A":

```sql
SELECT * FROM alunos
WHERE nome LIKE '%A';
```

Contains "A" in any position:

```sql
SELECT * FROM alunos
WHERE nome LIKE '%A%';
```

---

### 2. Underscore (_)

Represents:

```text
exactly 1 character
```

### Example

```sql
SELECT * FROM alunos
WHERE nome LIKE '_a%';
```

This returns names where:

```text
- First character: any
- Second character: "a"
```

Example result:

```text
Maria
Carlos
```

---

# 🚫 NOT LIKE

Used to **exclude patterns**.

```sql
SELECT * FROM alunos
WHERE nome NOT LIKE 'A%';
```

Result:

```text
All names that DO NOT start with "A"
```

---

# ⚠️ Case Sensitivity (Important)

In MySQL, by default:

```text
LIKE → is NOT case-sensitive
```

That is:

```sql
WHERE nome LIKE 'a%'
```

is equivalent to:

```sql
WHERE nome LIKE 'A%'
```

Additionally:

```text
- Accent support depends on collation (UTF-8 usually works well)
```

---

# 🧹 Eliminating Duplicates with DISTINCT

Sometimes a query returns repeated values.

Example:

```sql
SELECT nacionalidade FROM alunos;
```

Result:

```text
Brasil
Brasil
Argentina
Brasil
Chile
```

---

## Using DISTINCT

```sql
SELECT DISTINCT nacionalidade FROM alunos;
```

Result:

```text
Brasil
Argentina
Chile
```

💡 `DISTINCT` works like a **"duplicate filter"**.

---

# 📊 Aggregation Functions

They allow performing **calculations directly in the database**.

---

### 🔢 COUNT() — Counting

Counts how many records exist.

```sql
SELECT COUNT(*) FROM alunos;
```

Result:

```text
Total number of students
```

---

With filter:

```sql
SELECT COUNT(*) FROM alunos
WHERE nacionalidade = 'Brasil';
```

Result: Total number of students with Brazilian nationality

---

## 🔝 MAX() — Maximum Value

```sql
SELECT MAX(carga_horaria) FROM cursos;
```

Result:

```text
Highest workload
```

---

## 🔻 MIN() — Minimum Value

```sql
SELECT MIN(carga_horaria) FROM cursos;
```

---

## ➕ SUM() — Sum

```sql
SELECT SUM(carga_horaria) FROM cursos;
```

Result:

```text
Total sum of workloads
```

---

## 📉 AVG() — Average

```sql
SELECT AVG(carga_horaria) FROM cursos;
```

Result:

```text
Average workload
```

---

# 🧠 Important Notes

✔ Aggregation functions return **a single value**

✔ They can be combined with `WHERE`

✔ They automatically ignore `NULL` values

---

## 📋 Summary

* **LIKE**: Searches for patterns in text (not exact equality)
* **`%`** (percentage): Replaces zero or more characters
* **`_`** (underscore): Replaces exactly one character
* **NOT LIKE**: Excludes specific patterns
* **DISTINCT**: Eliminates duplicate values in the result
* **COUNT**: Counts records
* **MAX**: Finds the highest value
* **MIN**: Finds the lowest value
* **SUM**: Sums numeric values
* **AVG**: Calculates arithmetic mean
* **Case insensitive**: MySQL is not case-sensitive by default
* **Accents**: Normally supported with UTF-8

---

> 💡 **Tip:** With `LIKE`, `DISTINCT`, and aggregation functions, you start moving beyond the basics and into the level where the database **really works for you**.

> These features are widely used in: Reports, Dashboards, APIs, Data Analysis
