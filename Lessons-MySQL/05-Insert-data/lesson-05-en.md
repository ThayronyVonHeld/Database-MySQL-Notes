# 📚 Lesson 5 — Inserting Data with INSERT INTO (MySQL)

---

## 🎯 Lesson Objectives

* Understand the difference between **DDL** and **DML** commands
* Learn how to insert data into tables using **INSERT INTO**
* Work correctly with **dates, text, and numbers**
* Optimize insert operations for performance and maintainability
* Apply best practices when manipulating data

---

## 📊 SQL Command Classification: DDL vs. DML

```mermaid
graph TD
    A[SQL Commands] --> B[DDL - Data Definition Language]
    A --> C[DML - Data Manipulation Language]
    
    B --> B1[CREATE]
    B --> B2[ALTER]
    B --> B3[DROP]
    B --> B4[TRUNCATE]
    B --> B5[RENAME]
    
    C --> C1[INSERT]
    C --> C2[UPDATE]
    C --> C3[DELETE]
    
    style B fill:#2E7D32,color:#fff
    style C fill:#C62828,color:#fff
```

### DDL vs. DML — Detailed Comparison

| Characteristic  | **DDL (Data Definition)**     | **DML (Data Manipulation)**      |
| --------------- | ----------------------------- | -------------------------------- |
| **Focus**       | Database structure            | Data content                     |
| **When to use** | Initial design/setup          | Daily operations                 |
| **Auto-commit** | Yes (implicit)                | Depends (transaction-controlled) |
| **Examples**    | `CREATE`, `ALTER`, `DROP`     | `INSERT`, `UPDATE`, `DELETE`     |
| **Analogy**     | Planting the tree (structure) | Harvesting the fruits (data)     |

---

### Practical Example of the Difference

```sql
-- 🏗️ DDL - CREATING THE STRUCTURE
CREATE DATABASE school;
USE school;

CREATE TABLE student (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    birth_date DATE,
    height DECIMAL(3,2),
    active BOOLEAN DEFAULT TRUE
);

-- 📝 DML - INSERTING DATA
INSERT INTO student (name, birth_date, height) 
VALUES ('João Silva', '2005-05-15', 1.75);

-- 📝 More DML - UPDATING DATA
UPDATE student SET height = 1.78 WHERE id = 1;

-- 📝 More DML - DELETING DATA
DELETE FROM student WHERE id = 1;
```

---

## 🎯 The INSERT INTO Command

The `INSERT INTO` command is used to insert records into a table.

```sql
INSERT INTO table_name 
    (field1, field2, field3, ...) 
VALUES 
    (value1, value2, value3, ...);
```

---

### Step-by-Step Practical Example

```sql
-- 1. Create the table (DDL)
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE,
    active BOOLEAN DEFAULT TRUE
);

-- 2. Insert data (DML) — EXPLICIT FORM (RECOMMENDED)
INSERT INTO employee 
    (name, position, salary, hire_date) 
VALUES 
    ('Maria Santos', 'Analyst', 3500.00, '2023-06-10');

-- 3. Verify insertion
SELECT * FROM employee;
```

---

## Data Formats in INSERT

```sql
-- 📝 TEXT: Always single quotes
INSERT INTO employee (name) VALUES ('João "Joca" Silva');

-- 🔢 NUMBER: With or without quotes (MySQL accepts both)
INSERT INTO employee (salary) VALUES (2500.50);
INSERT INTO employee (salary) VALUES ('2500.50');

-- 📅 DATE: Single quotes + YYYY-MM-DD format
INSERT INTO employee (hire_date) VALUES ('2024-01-31');

-- ⚠️ INVALID DATES — Common mistakes
INSERT INTO employee (hire_date) VALUES ('31-01-2024');
INSERT INTO employee (hire_date) VALUES ('2024-13-01');
INSERT INTO employee (hire_date) VALUES ('2024-02-30');

-- ✅ BOOLEAN: TRUE/FALSE or 1/0
INSERT INTO employee (name, active) VALUES ('Inactive', FALSE);
INSERT INTO employee (name, active) VALUES ('Active', TRUE);
INSERT INTO employee (name, active) VALUES ('Active2', 1);
INSERT INTO employee (name, active) VALUES ('Inactive2', 0);
```

---

## 🤖 Optimizations: AUTO_INCREMENT and DEFAULT

If the table was created like this:

```sql
CREATE TABLE student (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    birth DATE,
    gender ENUM('M','F'),
    weight DECIMAL(5,2),
    height DECIMAL(3,2),
    nationality VARCHAR(20) DEFAULT 'Brazil'
);
```

We don’t need to manually provide the `id`:

```sql
INSERT INTO student (name, birth, gender, weight, height)
VALUES ('Ana', '2005-07-21', 'F', 60.00, 1.65);
```

MySQL will:

```
- Automatically generate the ID
- Insert "Brazil" as nationality (DEFAULT)
```

---

## ⚡ Simplified Insertion (Without Specifying Fields)

If all values are inserted in the exact table order:

```sql
INSERT INTO student
VALUES (DEFAULT, 'João', '2003-01-10', 'M', 80.00, 1.75, 'Brazil');
```

Although it works, **this is not the safest approach in real projects**.

> 💡 Best practice: always specify fields explicitly.

---

## 📚 Inserting Multiple Records

MySQL allows inserting multiple rows in a single command:

```sql
INSERT INTO student (name, birth, gender, weight, height, nationality)
VALUES
('Lucas', '2002-05-10', 'M', 70.00, 1.70, 'Brazil'),
('Marina', '2004-11-03', 'F', 55.00, 1.60, 'Brazil'),
('Pedro', '2001-02-18', 'M', 90.00, 1.85, 'Brazil');
```

This is more efficient and reduces the number of commands sent to the server.

---

## 🛡️ Best Practices When Inserting Data

The rules defined in the table still apply during insertion:

```
NOT NULL → prevents empty required fields
DEFAULT → defines automatic values
PRIMARY KEY → prevents duplicate identifiers
ENUM → restricts allowed values
```

---

### 1. Constraints for Data Quality

```sql
CREATE TABLE protected_client (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cpf VARCHAR(11) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    birth_date DATE NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    nationality VARCHAR(30) DEFAULT 'Brazil',
    active BOOLEAN DEFAULT TRUE,
    CHECK (birth_date < CURDATE())
);

-- ✅ Valid insertion
INSERT INTO protected_client (cpf, name, birth_date)
VALUES ('12345678901', 'Ana Silva', '1995-08-22');
```

---

### 2. NULL vs. DEFAULT

```sql
CREATE TABLE default_example (
    id INT PRIMARY KEY AUTO_INCREMENT,
    status VARCHAR(20) DEFAULT 'pending',
    quantity INT DEFAULT 1,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO default_example (status, quantity)
VALUES (NULL, NULL);

INSERT INTO default_example (status, quantity)
VALUES (DEFAULT, DEFAULT);

INSERT INTO default_example ()
VALUES ();
```

---

### 3. Never Store Age — Store Birth Date

```sql
-- ❌ WRONG
CREATE TABLE wrong_person (
    name VARCHAR(100),
    age INT
);

-- ✅ CORRECT
CREATE TABLE correct_person (
    name VARCHAR(100),
    birth_date DATE,
    age INT AS (TIMESTAMPDIFF(YEAR, birth_date, CURDATE())) VIRTUAL
);
```

---

## 📋 Quick Summary

* **DDL vs DML**: DDL defines structure, DML manipulates data
* **INSERT INTO** syntax:
  `INSERT INTO table (fields) VALUES (values)`
* **Formats**: Text and dates use single quotes; dates use `YYYY-MM-DD`
* **Auto-increment**: Omit the field or use `NULL`/`DEFAULT`
* **Multiple insertion** improves performance
* **Best practice**: store birth_date, not age; use constraints

---

💡 **Tip:**
“Creating tables is modeling. Inserting data is testing whether the model really works.”

---