# 📚 Lesson 9 — Querying Data with SELECT in MySQL

---

## 🎯 Lesson Objectives

* Master the SELECT command, the most important tool in SQL
* Learn how to select specific columns and all columns
* Understand result ordering with ORDER BY
* Use the WHERE clause to filter records precisely
* Apply range operators (BETWEEN) and list operators (IN)
* Combine conditions using logical operators AND and OR
* Understand the classification of SELECT as DQL (Data Query Language)

---

# 🔎 The SELECT Command

The **SELECT** command is the **most used in SQL**.

It is used to **query data stored in tables**.

Unlike commands that **modify the database**, `SELECT` only **reads data**.

---

## 📊 Selecting All Columns

The `*` character represents **all columns in the table**.

```sql
SELECT * FROM cursos;
```

In other words, this query returns **all records in the table**.

> ⚠️ In large systems, using `*` can be **inefficient**, because it returns more data than necessary.

---

# 🎯 Selecting Specific Columns

We can choose exactly **which columns we want to view**.

### Fundamental SELECT Structure

```sql
SELECT nome, ano
FROM cursos;
```

Result:

```
nome | ano
----------
Java | 2023
SQL  | 2024
C    | 2022
```

It is also possible to **change the order of the columns** in the result.

```sql
SELECT ano, nome
FROM cursos;
```

---

# 📈 Ordering Results with ORDER BY

By default, MySQL **does not guarantee a specific order** in a query.

To organize results we use:

```sql
ORDER BY
```

---

### Ascending Order (ASC)

This is the **default order**.

```sql
SELECT * FROM cursos
ORDER BY nome ASC; // ASC is optional (default)
```

Example:

```
SELECT nome, preco, carga_horaria
FROM curso
ORDER BY nome ASC;  
```

Result:

```
-- +--------------------+--------+---------------+
-- | nome               | preco  | carga_horaria |
-- +--------------------+--------+---------------+
-- | Banco de Dados MySQL| 520.00 |            50 |
-- | Excel Básico       | 220.00 |            20 |
-- | HTML e CSS         | 320.00 |            30 |
```

---

#### Order by Price (lowest to highest)

```
SELECT nome, preco, area
FROM curso
ORDER BY preco;  -- implicit ASC
```

Result: From cheapest to most expensive

---

### Descending Order (DESC)

Displays data **from highest to lowest** or **in reverse order**.

```sql
SELECT * FROM cursos
ORDER BY ano DESC;
```

Example:

#### Order by Price

```sql
SELECT nome, preco, area
FROM curso
ORDER BY preco DESC;
```

Result: From most expensive to cheapest

```
-- +--------------------+--------+-----------------+
-- | nome               | preco  | area            |
-- +--------------------+--------+-----------------+
-- | Machine Learning   | 890.00 | Programação     |
-- | Python para Dados  | 580.00 | Programação     |
-- | Redes de Computadores| 550.00| Infraestrutura  |
```

---

#### Order by Year (most recent first)

```
SELECT nome, ano_lancamento, preco
FROM curso
ORDER BY ano_lancamento DESC, preco DESC;
```

Result: Year 2023 first, within it, the most expensive first

---

### ⚠️ WARNING: DESC vs DESCRIBE

🚨 COMMON CONFUSION:

```sql
DESC curso;  -- DESCRIBE command (table structure)
-- Shows: Field, Type, Null, Key, Default, Extra

-- ORDER BY ... DESC;  -- DESCending parameter (descending order)
SELECT * FROM curso ORDER BY preco DESC;  -- Sorting
```

They are COMPLETELY different things!

---

### Multiple Ordering (By Multiple Columns)

We can organize results by **more than one criterion**.

```sql
SELECT area, nome, preco
FROM curso
ORDER BY area ASC, nome ASC;
```

Result: First groups by area, then sorts names within each area

```
-- +-----------------+--------------------+--------+
-- | area            | nome               | preco  |
-- +-----------------+--------------------+--------+
-- | Banco de Dados  | Banco de Dados MySQL|520.00 |
-- | Design          | Photoshop          |380.00 |
-- | Idiomas         | Inglês Técnico     |350.00 |
-- | Infraestrutura  | Redes de Computadores|550.00 |
-- | Produtividade   | Excel Básico       |220.00 |
-- | Programação     | Java Fundamentos   |450.00 |
-- | Programação     | JavaScript Avançado|490.00 |
-- | Programação     | Machine Learning   |890.00 |
-- | Programação     | Python para Dados  |580.00 |
-- | Web Design      | HTML e CSS         |320.00 |
-- +-----------------+--------------------+--------+
```

---

# 🔍 Filtering Records with WHERE

The **WHERE** clause allows you to **filter which records should appear in the result**.

Example:

```sql
SELECT * FROM cursos
WHERE ano = 2023;
```

Result Set: The result returned by the query.

```
id | nome | carga_horaria | ano
--------------------------------
1  | Java | 40             | 2023
```

---

### ⚙️ Relational Operators

To build filters we use comparison operators.

| Operator | Meaning               |
| -------- | --------------------- |
| =        | Equal                 |
| != or <> | Different             |
| >        | Greater than          |
| <        | Less than             |
| >=       | Greater than or equal |
| <=       | Less than or equal    |

### Examples

Greater than:

```sql
SELECT * FROM cursos
WHERE carga_horaria > 40;
```

Less than or equal:

```sql
SELECT * FROM cursos
WHERE ano <= 2023;
```

Different:

```sql
SELECT * FROM cursos
WHERE ano <> 2024;
```

---

# 📦 Range Operators

### 📏 BETWEEN Operator

Selects values **within a range**.

Important: **the limits are included**.

```sql
SELECT * FROM cursos
WHERE ano BETWEEN 2022 AND 2024;
```

Result: Courses released between 2021 and 2022 (inclusive)

Equivalent to:

```sql
WHERE ano >= 2022 AND ano <= 2024
```

---

### 📋 IN Operator

Allows checking whether a value is **within a specific list**.

Example:

```sql
SELECT * FROM cursos
WHERE ano IN (2022, 2024);
```

Result:

```
Courses from 2022 and 2024
```

Anything **outside this list is ignored**.

---

# 🧠 Logical Operators

They allow combining multiple conditions.

### AND

All conditions must be **true at the same time**.

```sql
SELECT * FROM cursos
WHERE ano = 2023 AND carga_horaria > 30;
```

Only records that satisfy **both conditions** appear.

---

### OR

Only **one condition needs to be true**.

```sql
SELECT * FROM cursos
WHERE ano = 2023 OR ano = 2024;
```

Result:

```
Courses from 2023 or 2024
```

---

## Complex Combination with AND and OR

> ⚠️ Precedence: AND has priority over OR (just like multiplication has priority over addition)

Example: We want Programming OR Design courses, but only active ones

```sql
SELECT nome, area, ativo
FROM curso
WHERE (area = 'Programação' OR area = 'Design')
  AND ativo = TRUE;
```

* Parentheses are ESSENTIAL!
* Without parentheses, it would be interpreted as:

```
WHERE area = 'Programação' OR (area = 'Design' AND ativo = TRUE)
```

---

# 🏷️ SELECT Classification

There is a theoretical discussion about the classification of this command.

Some authors classify it as:

```
DML — Data Manipulation Language
```

Because it **manipulates data by querying records**.

Others classify it as:

```
DQL — Data Query Language
```

Because the command **only queries information**, without changing anything in the database.

### In Practice: Classification Does Not Change Syntax

* Regardless of the classification, SELECT works the same
* The important thing is knowing how to use it!

---

## 📋 Quick Summary

* **SELECT**: Command used to query data (classified as DQL)
* **Column selection**: `*` for all columns, or list specific columns
* **ORDER BY**: Organizes results (`ASC` ascending/default, `DESC` descending)
* **WHERE**: Filters records using conditions
* **Relational operators**: `=`, `!=`, `<>`, `<`, `>`, `<=`, `>=`
* **BETWEEN**: Values within a range (inclusive)
* **IN**: Values within a specific list
* **AND**: All conditions must be true
* **OR**: At least one condition must be true
* **Result Set**: The set of results generated by the query
* **DQL vs DML**: SELECT does not modify data, it only queries

---

> 💡**Tip:** Much of the work with databases **is not inserting data**, but **correctly querying information**.

> Mastering **SELECT** is essential for working with **reports, APIs, dashboards, and enterprise systems**.
