# 📚 Lesson 13 — Relationships in Practice (FK, Integrity and JOINs)

---

* Understand the role of **Engines** in MySQL
* Understand the ACID concept and its importance for transactions
* Implement Foreign Keys (FK) in practice
* Ensure Referential Integrity between tables
* Use **JOINs** to combine tables
* Differentiate **INNER JOIN**, **LEFT JOIN** and **RIGHT JOIN**
* Use **aliases** to improve queries

---

## ⚙️ Engines (Storage Engines)

When we create a table in MySQL, we need to define **how it will be stored**.

---

## Main Engines

### 🔹 InnoDB (Default)

```text
✔ Supports foreign keys (FK)
✔ Supports transactions (ACID)
✔ Ensures referential integrity
```

👉 It is the most used today.

---

### 🔹 MyISAM

```text
❌ Does not support FK
❌ Does not follow ACID
✔ Simpler (legacy)
```

---

### 🔹 ExtraDB

```text
✔ Modern alternative
✔ Transaction support
```

---

💡 In practice:

```text
Use InnoDB almost always
```

---

### How to define the Engine

* Explicit: Define the engine at creation

```sql
CREATE TABLE cliente (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
) ENGINE = InnoDB;
```

* Check the engine of a table

```sql
SELECT ENGINE 
FROM information_schema.TABLES 
WHERE TABLE_NAME = 'cliente';
```

* Change the engine after creation

```sql
ALTER TABLE cliente ENGINE = InnoDB;
```

* See all available engines

```
SHOW ENGINES;
```

---

## 🧠 The ACID Concept

For a database to be reliable, it must follow 4 rules:

---

## 🔹 Atomicity

```text
All or nothing
Complete transaction or rollback
```

If something fails:

👉 the database undoes everything (rollback)

---

## 🔹 Consistency

```text
The database never becomes "broken"
```

It always maintains valid rules.

---

## 🔹 Isolation

```text
Transactions do not interfere with each other
```

One does not affect another.

---

## 🔹 Durability

```text
Saved → it stays saved
```

Even with power failure, the data remains.

---

## 🔗 Implementing Foreign Key (FK)

Now let’s go to practice.

---

## 📌 Main rule (1:N)

```text
PK from the 1 side → goes to the N side as FK
```

---

## 🧩 Example

Table cursos:

```sql
CREATE TABLE cursos (
    id INT PRIMARY KEY,
    nome VARCHAR(100)
);
```

Table alunos:

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    curso_id INT
);
```

---

## 🔧 Adding the FK

```sql
ALTER TABLE alunos
ADD FOREIGN KEY (curso_id)
REFERENCES cursos(id);
```

---

## ⚠️ Important rule

```text
Type and size MUST be the same
```

---

## 🛡️ Referential Integrity

The FK ensures that the database **does not allow inconsistencies**.

---

## ❌ Error example

```text
Student is linked to the course "MySQL"
```

If you try to delete the course:

```sql
DELETE FROM cursos WHERE id = 1;
```

👉 The database will NOT allow it.

---

💡 This prevents:

```text
- Orphan data
- Broken relationships
- System bugs
```

---

# 🔄 JOIN (Combining Tables)

JOIN allows **combining data from multiple tables**.

---

## ❌ Problem

We have: Data separated into two tables

```text
SELECT * FROM aluno;
-- +----+--------------+-------------------+----------+
-- | id | nome         | email             | curso_id |
-- +----+--------------+-------------------+----------+
-- | 1  | João Silva   | joao@email.com    | 1        |
-- | 2  | Maria Santos | maria@email.com   | 1        |
-- | 3  | Pedro        | pedro@email.com   | 2        |
-- +----+--------------+-------------------+----------+

SELECT * FROM curso;
-- +----+----------------+---------------+
-- | id | nome           | carga_horaria |
-- +----+----------------+---------------+
-- | 1  | MySQL Completo | 50            |
-- | 2  | Java Fundamentos| 60           |
-- +----+----------------+---------------+
```

👉 How to show the course name together with the student?

---

## ✅ Solution: JOIN

* INNER JOIN: Shows only students with a valid course

```sql
SELECT 
    aluno.nome AS aluno_nome,
    curso.nome AS curso_nome,
    curso.carga_horaria
FROM aluno
INNER JOIN curso ON aluno.curso_id = curso.id
ORDER BY curso.nome, aluno.nome;

```

* Result:

```sql
-- +--------------+------------------+---------------+
-- | aluno_nome   | curso_nome       | carga_horaria |
-- +--------------+------------------+---------------+
-- | João Silva   | MySQL Completo   | 50            |
-- | Maria Santos | MySQL Completo   | 50            |
-- | Pedro        | Java Fundamentos | 60            |
-- +--------------+------------------+---------------+
```

* Students without a course do NOT appear

---

# ⚠️ ON Clause (Mandatory)

```text
Defines HOW the tables are connected
```

Without it:

👉 it becomes a **cartesian product** (serious error)

---

❌ Example: WITHOUT ON

```
SELECT * FROM aluno, curso;
```

* Result: 3 students × 2 courses = 6 rows (wrong combinations!)

```sql
-- +----+---------+----------+----+------------------+
-- | id | nome    | curso_id | id | nome             |
-- +----+---------+----------+----+------------------+
-- | 1  | João    | 1        | 1  | MySQL Completo   | ← Correct
-- | 1  | João    | 1        | 2  | Java Fundamentos | ← Wrong!
-- | 2  | Maria   | 1        | 1  | MySQL Completo   | ← Correct
-- | 2  | Maria   | 1        | 2  | Java Fundamentos | ← Wrong!
```

---

# 🔗 Types of JOIN

---

## 🔹 INNER JOIN

Shows only data that has a match.

```sql
SELECT a.nome, c.nome
FROM alunos a
INNER JOIN cursos c
ON a.curso_id = c.id;
```

---

💡 Returns:

```text
Only students who have a valid course
```

---

## 🔹 LEFT JOIN

Prioritizes the left table.

```sql
SELECT a.nome, c.nome
FROM alunos a
LEFT JOIN cursos c
ON a.curso_id = c.id;
```

---

💡 Returns:

```text
All students
Even those who do NOT have a course
```

---

## 🔹 RIGHT JOIN

Prioritizes the right table.

```sql
SELECT a.nome, c.nome
FROM alunos a
RIGHT JOIN cursos c
ON a.curso_id = c.id;
```

---

💡 Returns:

```text
All courses
Even without students
```

---

# 🧠 Mental Visualization

```text
INNER JOIN → intersection  
LEFT JOIN  → everything from the left  
RIGHT JOIN → everything from the right  
```

---

## 📝 Aliases with AS

### Why use Aliases?

To simplify queries:

* ❌ WITHOUT ALIAS (long and repetitive code)

```sql
SELECT 
    aluno.nome, 
    curso.nome, 
    curso.carga_horaria,
    aluno.email
FROM aluno
INNER JOIN curso ON aluno.curso_id = curso.id
ORDER BY curso.nome, aluno.nome;
```

* ✅ WITH ALIAS (cleaner and more professional)

```sql
SELECT
  a.nome AS aluno_nome,
  c.nome AS curso_nome,
  c.carga_horaria,
  a.email
FROM aluno AS a
INNER JOIN curso AS c 
ON a.curso_id = c.id
ORDER BY c.nome, a.nome;
```

---

### AS Syntax

* AS is optional (space also works)

```sql
SELECT a.nome aluno_nome, c.nome curso_nome
FROM aluno a
INNER JOIN curso c 
ON a.curso_id = c.id;
```

* For columns with spaces, use quotes

```sql
SELECT a.nome AS "Student Name"
FROM aluno a;
```

---

## 🧪 Checking Structure

```sql
DESC alunos;
```

In MySQL Workbench, you will see:

```text
MUL → indicates foreign key
```

---

## 📋 Quick Summary

| Concept                   | Description                                    |
| ------------------------- | ---------------------------------------------- |
| **InnoDB**                | Default engine that supports FK and ACID       |
| **ACID**                  | Atomicity, Consistency, Isolation, Durability  |
| **FK (Foreign Key)**      | Links tables, references PK of another table   |
| **Referential Integrity** | Prevents deletion of records with dependencies |
| **INNER JOIN**            | Shows only matching records                    |
| **LEFT JOIN**             | Shows ALL from left + matches                  |
| **RIGHT JOIN**            | Shows ALL from right + matches                 |
| **Alias (AS)**            | Nicknames for tables/columns                   |
| **ON**                    | defines the relationship                       |

---

> 💡**Tip**: ON in JOIN is not optional — without it, you will get a catastrophic cartesian product!
