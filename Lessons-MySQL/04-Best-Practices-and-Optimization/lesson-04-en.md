# 📚 Lesson 4 – Optimization and Best Practices in MySQL

---

## 🎯 Lesson Objectives

* Understand how to remove and recreate structures in MySQL during development
* Correctly implement UTF-8 character support
* Understand the importance of choosing appropriate primitive types
* Learn how constraints ensure data integrity
* Understand the role of the primary key in record identification
* Recognize important SQL syntax details

---

## 🗑️ Managing and Removing Structures

During the early development phase, when no important data exists yet, it is common to recreate the database structure.

### Removing a Database

```sql
DROP DATABASE database_name;
```

The **DROP** command completely removes the existing structure.

> ⚠️ Warning: This command permanently deletes the database and everything inside it.

---

### Controlled Deletion Commands

```sql
-- ⚠️ DANGEROUS COMMAND — USE WITH CAUTION!
DROP DATABASE school;  -- Deletes EVERYTHING: structure + data

-- Safer alternatives during development:
DROP DATABASE IF EXISTS school;
CREATE DATABASE school;

-- For specific tables:
DROP TABLE IF EXISTS student;
DROP TABLE IF EXISTS teacher, subject, class;

-- Partial deletion (data only):
TRUNCATE TABLE student;          -- Keeps structure, removes data
DELETE FROM student WHERE id > 100;  -- Selective deletion
```

---

### Safe Development Workflow

```sql
-- 1. First, check what exists
SHOW DATABASES;
SHOW TABLES;

-- 2. Comment your intention
/*
Recreating the 'school' database
because the structure needs to be redesigned.
Current data: test data only.
*/

-- 3. Execute safely
DROP DATABASE IF EXISTS school;
CREATE DATABASE school;
USE school;

-- 4. Confirm
SELECT DATABASE();
```

---

## 🌐 Improving CREATE DATABASE (UTF-8)

To avoid problems with accents and special characters, define encoding when creating the database.

### The Special Character Problem

```sql
-- ❌ PROBLEM
CREATE DATABASE school;
-- Insert: "João São Paulo"
-- Result: "JoÃ£o SÃ£o Paulo"

-- ✅ SOLUTION
CREATE DATABASE school
DEFAULT CHARACTER SET utf8mb4
DEFAULT COLLATE utf8_general_ci;
```

### CHARACTER SET

Defines the supported character set.

```text
UTF-8 supports:
- Accented characters
- Special symbols
- Multiple languages
```

### COLLATE

Defines **text comparison and sorting rules**.

---

### The Semicolon in SQL

The semicolon `;` marks the **end of a SQL command**, regardless of how many lines it uses.

---

## ⚙️ Primitive Type Optimization

### Principle: “Less is More”

Choosing the correct data type improves performance and reduces storage usage.

---

### Numeric Optimization — Save Bytes!

```sql
CREATE TABLE optimized_person (
    age TINYINT UNSIGNED,
    children TINYINT,
    zip_code MEDIUMINT UNSIGNED,
    
    salary DECIMAL(10,2),
    height DECIMAL(3,2),
    weight DECIMAL(5,2),
    
    birth_date DATE,
    
    age_calculated TINYINT
        AS (TIMESTAMPDIFF(YEAR, birth_date, CURDATE()))
        VIRTUAL
);
```

---

### ENUM vs VARCHAR for Fixed Values

```sql
CREATE TABLE enum_examples (
    gender ENUM('M', 'F', 'O') NOT NULL,
    
    state ENUM(
        'AC','AL','AP','AM','BA','CE','DF','ES','GO',
        'MA','MT','MS','MG','PA','PB','PR','PE','PI',
        'RJ','RN','RS','RO','RR','SC','SP','SE','TO'
    ),
    
    order_status ENUM(
        'pending',
        'processing',
        'shipped',
        'delivered',
        'canceled'
    ) DEFAULT 'pending',
    
    city_id SMALLINT
);
```

> ENUM is efficient for **small fixed lists**, but not scalable for large datasets.

---

## 🛡️ Constraints (Integrity Rules)

Constraints ensure **data consistency and reliability**.

---

### MySQL Constraint System

Common constraints include:

* NOT NULL → required field
* DEFAULT → automatic initial value
* UNIQUE → prevents duplicates
* CHECK → custom validation rule
* PRIMARY KEY → unique identifier
* FOREIGN KEY → referential integrity

---

### Practical Constraint Example

```sql
CREATE TABLE professional_client (
    id INT PRIMARY KEY AUTO_INCREMENT,
    
    name VARCHAR(100) NOT NULL,
    cpf VARCHAR(11) NOT NULL,
    
    email VARCHAR(100) UNIQUE NOT NULL,
    
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    active BOOLEAN DEFAULT TRUE,
    nationality VARCHAR(30) DEFAULT 'Brazil',
    
    age TINYINT CHECK (age >= 18),
    salary DECIMAL(10,2) CHECK (salary > 0),
    
    phone VARCHAR(15) UNIQUE,
    
    INDEX idx_name (name),
    INDEX idx_created (created_at)
);
```

---

### AUTO_INCREMENT in Action

```sql
INSERT INTO professional_client (name, cpf, email)
VALUES ('Maria Silva', '12345678901', 'maria@email.com');
-- id = 1

INSERT INTO professional_client (name, cpf, email)
VALUES ('João Santos', '98765432109', 'joao@email.com');
-- id = 2
```

---

## 🔑 Primary Key (PRIMARY KEY)

The **primary key** uniquely identifies each record.

```sql
id INT AUTO_INCREMENT PRIMARY KEY
```

Without a primary key, duplicate records and update/delete issues become common.

With a primary key, data becomes **traceable, consistent, and reliable**.

---

### AUTO_INCREMENT — Automatic Sequencer

```sql
CREATE TABLE auto_increment_example (
    id INT PRIMARY KEY AUTO_INCREMENT
);
```

Behavior:

* Starts at 1 by default
* Increments automatically
* TRUNCATE resets the counter
* Manual values are allowed

---

## 📝 Syntax Details

### Single Quotes

```sql
'text'
'value'
```

Used for literal values in SQL.

---

### Backticks

```sql
`table_name`
```

Allow special characters in identifiers, but are **not recommended in professional modeling**.

---

## 📊 Quick Summary

* DROP DATABASE removes structures during development
* UTF-8 prevents encoding issues
* Choosing the correct data type improves performance
* Constraints ensure data quality
* Every table should have a PRIMARY KEY
* SQL syntax is simple but important

---

> 💡 Tip: A well-designed database prevents problems before they happen. Choosing correct types, constraints, and keys is just as important as writing SQL queries.

