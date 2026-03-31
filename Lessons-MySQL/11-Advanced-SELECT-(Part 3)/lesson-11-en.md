# 📚 Lesson 11 — Advanced SELECT (Part 3)

---

## 🎯 Lesson Objectives

* Differentiate **DISTINCT** and **GROUP BY**
* Group data using **GROUP BY**
* Use aggregation functions on groups
* Filter groups with **HAVING**
* Understand the difference between **WHERE** and **HAVING**
* Create **subqueries (SELECT inside SELECT)**
* Understand the **execution order** of an SQL query

---

# 🔍 DISTINCT vs GROUP BY

These two concepts may seem similar, but they have different purposes.

---

## 🧹 DISTINCT (Eliminate Repetitions)

Removes duplicate values.

```sql id="f5d3k2"
SELECT DISTINCT nacionalidade FROM alunos;
```

Result:

```text id="v9x2ja"
Brasil
Argentina
Chile
```

👉 Here you only see **unique values**, without knowing how many times they appear.

---

## 📦 GROUP BY (Group Data)

Groups similar records to allow **analysis**.

```sql id="m8p2k1"
SELECT nacionalidade FROM alunos
GROUP BY nacionalidade;
```

Result:

```text id="t4z9lb"
Brasil
Argentina
Chile
```

👉 Visually it may look the same as `DISTINCT`, but **GROUP BY allows much more**.

---

# 📊 GROUP BY + Aggregation Functions

Now comes the real power.

```sql id="q7h1nx"
SELECT nacionalidade, COUNT(*) 
FROM alunos
GROUP BY nacionalidade;
```

Result:

```text id="z3c8uw"
Brasil     | 3
Argentina  | 1
Chile      | 1
```

---

💡 Here you don’t just see the data — you **analyze the data**.

---

## 📌 Another Example

```sql id="r5y2em"
SELECT carga_horaria, COUNT(*)
FROM cursos
GROUP BY carga_horaria;
```

Result:

```text id="u8k3qd"
40 | 6 cursos
20 | 1 curso
30 | 2 cursos
```

---

# 🔎 Filtering Groups with HAVING

`HAVING` works like a:

```text id="x2n9wa"
WHERE for groups
```

---

## Example

```sql id="p6c1ls"
SELECT nacionalidade, COUNT(*) 
FROM alunos
GROUP BY nacionalidade
HAVING COUNT(*) > 2;
```

Result:

```text id="n4e7rt"
Brasil | 3
```

---

# ⚠️ WHERE vs HAVING

This difference is **CRITICAL**:

| WHERE                      | HAVING           |
| -------------------------- | ---------------- |
| Filters individual records | Filters groups   |
| Executed before GROUP BY   | Executed after   |
| Does not use aggregation   | Uses aggregation |

---

## Practical Comparison

### WHERE (before grouping)

```sql id="y9b3fa"
SELECT * FROM alunos
WHERE idade > 18;
```

---

### HAVING (after grouping)

```sql id="k2t8ov"
SELECT idade, COUNT(*)
FROM alunos
GROUP BY idade
HAVING COUNT(*) > 2;
```

---

# 🧠 Subqueries

A **subquery** is a `SELECT` inside another `SELECT`.

---

## 🎯 Classic Example

Courses with workload above the average:

```sql id="w1q6mz"
SELECT * FROM cursos
WHERE carga_horaria > (
    SELECT AVG(carga_horaria) FROM cursos
);
```

---

💡 What happens here:

1. The subquery calculates the average
2. The main SELECT uses this value as a filter

---

## 🧩 Another Example with HAVING

```sql id="c4v7ye"
SELECT carga_horaria, COUNT(*)
FROM cursos
GROUP BY carga_horaria
HAVING carga_horaria > (
    SELECT AVG(carga_horaria) FROM cursos
);
```

---

# ⚙️ SELECT Execution Order

Even if we write it like this:

```sql id="z7m2ds"
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ...
```

MySQL executes it in this order:

```text id="a9x1rf"
1. FROM       → defines the data source
2. WHERE      → filters records
3. GROUP BY   → groups the data
4. HAVING     → filters the groups
5. SELECT     → chooses the columns
6. ORDER BY   → sorts the final result
```

💡 Understanding this avoids MANY errors.

---

# 📋 Quick Summary

| Concept         | What it does                    | When to use                    |
| --------------- | ------------------------------- | ------------------------------ |
| **DISTINCT**    | Eliminates duplicate values     | Just list unique values        |
| **GROUP BY**    | Organizes data into groups      | Calculate statistics per group |
| **COUNT()**     | Counts records per group        | Know how many in each group    |
| **SUM()**       | Sums numeric values             | Total values per group         |
| **AVG()**       | Calculates average per group    | Statistical averages           |
| **MAX()/MIN()** | Highest/lowest value per group  | Extremes per group             |
| **WHERE**       | Filters ROWS (before GROUP BY)  | Filter individual records      |
| **HAVING**      | Filters GROUPS (after GROUP BY) | Filter aggregation results     |
| **Subquery**    | SELECT inside SELECT            | Dynamic and complex queries    |

---

> 💡 **Tip:** By mastering the SELECT command, you stand out A LOT in the market and are able to perform:
>
> * Complex reports
> * Professional dashboards
> * Smart APIs
> * Real data analysis
