# 📚 Lesson 6 — Modifying Table Structures in MySQL

---

## 🎯 Lesson Objectives

* Learn how to use the `ALTER TABLE` command to modify tables
* Understand the differences between `ADD`, `MODIFY`, `CHANGE`, and `DROP`
* Safely implement constraints after table creation
* Use DDL commands following best practices and error prevention
* Manage data integrity during structural changes

---

## 🧱 Modifying Structures with ALTER TABLE

So far, we have learned how to **create tables**.
But real systems evolve — and the database must evolve with them.

That’s exactly why we use the command:

```sql
ALTER TABLE
```

This command belongs to the **DDL (Data Definition Language)** category because it changes the **database structure**.

---

## ➕ Adding Columns

By default, a new column is added to the end of the table.

```sql
ALTER TABLE aluno
ADD COLUMN email VARCHAR(100);
```

---

### Controlling Column Position

```sql
-- First column in the table
ALTER TABLE aluno
ADD COLUMN matricula INT FIRST;

-- After a specific column
ALTER TABLE aluno
ADD COLUMN telefone VARCHAR(20) AFTER nome;
```

---

## ➖ Removing Columns

Completely removes the column and its data:

```sql
ALTER TABLE aluno
DROP COLUMN telefone;
```

⚠️ This operation is permanent.

---

## 🔧 Modifying Columns (MODIFY)

Changes the **data type** or **constraints**, while keeping the column name.

```sql
ALTER TABLE aluno
MODIFY COLUMN nome VARCHAR(150) NOT NULL;
```

### 🚨 LIMITATIONS OF MODIFY:

1. It cannot change the column name
2. Some changes require table recreation (slow for large tables)

---

## 🔄 Changing Name and Structure (CHANGE)

The `CHANGE` command allows you to modify:

* Column name
* Data type
* Constraints

```sql
ALTER TABLE aluno
CHANGE COLUMN nome nome_completo VARCHAR(150) NOT NULL;
```

Notice that you must specify:

```text
old_name → new_name → type → constraints
```

---

## 🔁 Renaming Tables

```sql
ALTER TABLE aluno
RENAME TO estudantes;
```

---

## 🗑️ Removing Tables (DROP TABLE)

Completely deletes the table:

```sql
DROP TABLE estudantes;
```

This removes:

```text
- Structure
- Records
- Indexes
- Keys
```

⚠️ There is no “undo”.

---

## 🛡️ Safety Parameters

To avoid errors:

```sql
CREATE TABLE IF NOT EXISTS cursos (
    id INT
);

DROP TABLE IF EXISTS cursos;
```

---

## 🔒 Constraints and Advanced Parameters

### IF NOT EXISTS / IF EXISTS — DDL Safety

✅ Avoid errors during creation

```sql
CREATE TABLE IF NOT EXISTS produto (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100)
);
```

#### Even if executed multiple times, it will not fail

```sql
CREATE TABLE IF NOT EXISTS produto (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100)
);
-- Even if executed multiple times, it will not fail
```

✅ Avoid errors during deletion

```sql
DROP TABLE IF EXISTS produto_inexistente;  -- Only a warning, not an error
```

✅ TOGETHER: Safe table recreation

```sql
DROP TABLE IF EXISTS temp_data;
CREATE TABLE temp_data (
    id INT PRIMARY KEY
);
```

---

### UNIQUE

Prevents duplicate values.

```sql
ALTER TABLE cursos
ADD UNIQUE (nome);
```

Important difference:

```text
PRIMARY KEY → identifies records
UNIQUE → prevents duplication
```

Example:

```sql
CREATE TABLE curso (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- PK: unique, not null, identifier
    codigo VARCHAR(10) UNIQUE,          -- UNIQUE: unique, may be null
    nome VARCHAR(100) NOT NULL UNIQUE   -- Can have multiple UNIQUE constraints
);
```

---

### UNSIGNED

Prevents negative numbers and saves storage space.

```sql
ALTER TABLE cursos
MODIFY COLUMN carga_horaria INT UNSIGNED;
```

Example:

```sql
CREATE TABLE metricas (
    -- With UNSIGNED: 0 to 255 (1 byte)
    visitas TINYINT UNSIGNED,

    -- Without UNSIGNED: -128 to 127 (1 byte)
    temperatura TINYINT,
    
    -- Significant savings in millions of records
    populacao INT UNSIGNED,  -- 0 to ~4 billion
    altura SMALLINT UNSIGNED -- 0 to 65535 cm (655 meters)
);
```

Ideal for:

```text
- Ages
- Quantities
- Counters
- Inventory
```

---

### Adding a PRIMARY KEY After Creation

```sql
ALTER TABLE cursos
ADD PRIMARY KEY (id);
```

Example:

```sql
-- ❌ TABLE WITHOUT PRIMARY KEY (future problem guaranteed)
CREATE TABLE cliente_sem_pk (
    nome VARCHAR(100),
    cpf VARCHAR(11)
);

-- ✅ ADD PRIMARY KEY LATER
ALTER TABLE cliente_sem_pk 
ADD COLUMN id INT FIRST;

UPDATE cliente_sem_pk SET id = @row_number := @row_number + 1;

ALTER TABLE cliente_sem_pk 
ADD PRIMARY KEY (id);

-- OR add PK to an existing column (if unique)
ALTER TABLE cliente_sem_pk 
ADD PRIMARY KEY (cpf);
```

---

## ⚠️ NOT NULL Conflicts

If a table already contains records, adding a required column may cause an error.

Problem:

```sql
ALTER TABLE aluno
ADD COLUMN pais VARCHAR(50) NOT NULL;
```

Solution:

```sql
ALTER TABLE aluno
ADD COLUMN pais VARCHAR(50) NOT NULL DEFAULT 'Brasil';
```

The **DEFAULT** value automatically fills existing records.

---

## 🔎 Verifying Changes

Always confirm the structure after modifications:

```sql
DESCRIBE aluno;
DESC aluno;
```

This prevents silent errors.

---

## 📊 Quick Summary

* **ALTER TABLE** modifies existing structures
* **ADD COLUMN** adds columns
* **DROP COLUMN** removes columns
* **MODIFY** changes type and constraints
* **CHANGE** modifies name and structure
* **RENAME TO** renames the table
* **DROP TABLE** deletes the entire table
* **UNIQUE** prevents duplication
* **UNSIGNED** prevents negative numbers
* **DESCRIBE** verifies changes

> 💡 Tip: Creating tables is just the beginning. Maintaining and evolving the database structure is the real job of a systems developer.
