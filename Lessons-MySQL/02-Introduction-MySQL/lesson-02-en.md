# 📚 Lesson 2 – Introduction to MySQL

---

## 🎯 Lesson Objectives

* Learn about the origin and historical evolution of MySQL
* Understand the licensing model and the concept of free/open-source software
* Understand the subdivisions of the SQL language (DDL, DML, DQL, DCL, DTL)
* Understand the concept of transactions and the ACID principles (D.I.C.A.)
* Learn about the main tools used in the MySQL ecosystem

---

## 🕰️ Origin and Historical Evolution of MySQL

**MySQL** emerged in **1994, in Sweden**, created by **Michael Widenius** (known as *Monty*) and **David Axmark**. Unlike many technologies of its time, it was designed from the beginning as **free and open-source software**.

### 📜 Licensing and Open Source Philosophy

MySQL was distributed under the **GPL (General Public License)**, which guarantees:

```text
- Freedom to study the source code
- Ability to modify the software
- Right to redistribute it
```

> 💡 This model was essential for the rapid adoption of MySQL by companies, universities, and independent developers.

---

### 🏢 Corporate Changes

MySQL’s trajectory includes important acquisitions in the technology sector:

* **2007** – Acquired by **Sun Microsystems** (the company behind Java)
* **2009** – Sun was acquired by **Oracle**, which became the current maintainer of MySQL

This change raised concerns within the open-source community.

---

### 🌱 The Emergence of MariaDB

In response to Oracle’s acquisition, **Monty Widenius** created **MariaDB**, a *fork* of MySQL.

```text
Fork = a new development line
based on the original code, but independent
```

Despite this:

* **MySQL** remains the most consolidated solution
* It is widely used by large organizations

### 🌍 Notable Users

* Google
* NASA
* Wikipedia
* Facebook (at large scale)

---

## 🧩 The SQL Language in MySQL

MySQL uses **SQL (Structured Query Language)**, which is subdivided according to the purpose of its commands.

---

### 🏗️ DDL – Data Definition Language

Used to **define and modify the database structure**.

```sql
-- Practical examples
CREATE DATABASE school;           -- Create database
CREATE TABLE student (...);       -- Create table
ALTER TABLE student ADD COLUMN ...; -- Modify table
DROP TABLE student;               -- Remove table
TRUNCATE TABLE student;           -- Empty table
```

---

### ✏️ DML – Data Manipulation Language

Responsible for the **direct manipulation of stored data**.

```sql
-- Practical examples
INSERT INTO student VALUES (...); -- Insert data
UPDATE student SET name = ...;    -- Update data
DELETE FROM student WHERE ...;    -- Remove data
```

---

### 🔍 DQL – Data Query Language

Focused on **querying data**.

```sql
-- Practical examples
SELECT * FROM student;            -- Query all data
SELECT name, age FROM student;    -- Specific columns
SELECT * FROM student WHERE ...;  -- With filters
```

> 📌 Although some authors include SELECT within DML, didactically it is treated as DQL.

---

## 🔐 DCL – Data Control Language

Manages **permissions and access control** in the database.

```sql
-- Practical examples
GRANT SELECT ON school.* TO user;     -- Grant permission
REVOKE DELETE ON school.* FROM user;  -- Revoke permission
```

---

### 🔄 DTL – Data Transaction Language

Related to **transaction control**, ensuring operational safety.

```sql
-- Practical examples
START TRANSACTION;               -- Start transaction
COMMIT;                          -- Confirm changes
ROLLBACK;                        -- Undo changes
```

---

## 🔒 Transactions and the D.I.C.A. (ACID) Concept

To ensure reliability, MySQL follows the principles known as **ACID**, presented here using the acronym **D.I.C.A.**

---

### D – Durability

After a transaction is committed, the data must **remain stored**, even in case of failures.

```sql
-- MySQL example
START TRANSACTION;
UPDATE account SET balance = balance - 100 WHERE id = 1;
UPDATE account SET balance = balance + 100 WHERE id = 2;
COMMIT;  -- Now it is permanent!
```

---

### I – Isolation

Simultaneous transactions **must not interfere with each other**.

```sql
-- User A (at 10:00:00)
START TRANSACTION;
SELECT balance FROM account WHERE id = 1;  -- Sees $500.00

-- User B (at 10:00:01)
START TRANSACTION;
UPDATE account SET balance = 400 WHERE id = 1;
COMMIT;

-- User A still sees $500.00 until COMMIT
```

---

### C – Consistency

The database must always move from one **valid state** to another valid state.

```sql
-- Valid state: Balance is never negative
CREATE TABLE account (
    id INT PRIMARY KEY,
    balance DECIMAL(10,2) CHECK (balance >= 0) -- Constraint
);

-- Transaction rejected if consistency is violated
UPDATE account SET balance = -50 WHERE id = 1; -- ERROR!
```

---

### A – Atomicity

The **all-or-nothing** principle.

```sql
START TRANSACTION;
-- Operation 1: OK
UPDATE inventory SET quantity = quantity - 1 WHERE product_id = 5;

-- Operation 2: FAILS (product does not exist)
INSERT INTO sale (product_id, quantity) VALUES (999, 1);

-- Since the second failed, everything is undone
ROLLBACK; -- Atomicity in action!
```

> 💡 Works like an internal “Ctrl + Z” of the database.

---

### Why are transactions important?

```text
ATM Example:
1. You request $100.00
2. The system checks your balance ($500.00 available)
3. The system debits $100.00 from your account
4. The ATM releases $100.00

If step 3 or 4 fails: Problem!
```

---

## 🛠️ MySQL Ecosystem Tools

### 🖥️ MySQL Server

```text
Role: The database engine
Characteristics:
- Service running in the background
- Listens for connections (usually port 3306)
- Processes SQL commands
- Manages data on disk
```

---

### 🧰 MySQL Workbench (Graphical Interface)

```text
Advantages over the terminal:
- User-friendly visual interface
- SQL editor with syntax highlighting
- Visual table design
- Graphical administration
- Visual export/import
- Data modeling (EER Diagrams)
```

> ✅ More productive and user-friendly than using the terminal alone.

---

### 🧠 Terminal / CLI (Command Line Interface)

```bash
# Basic terminal commands
mysql --version                    # Check version
mysql -u root -p                   # Connect to server
mysql -h localhost -u user -p      # Connect with host

# Inside the MySQL CLI
SHOW DATABASES;                    # List databases
USE database_name;                 # Select database
SHOW TABLES;                       # List tables
EXIT; or \q                        # Exit
```

---

### 🗃️ Official Documentation

```text
Website: https://dev.mysql.com/doc/
Content:
- Complete manual
- Step-by-step tutorials
- Command reference
- Practical examples
- Release notes
```

---

## 📥 Practical Installation

### Step-by-Step for Windows

```text
1. Go to: https://www.mysql.com/downloads/
2. Select: "MySQL Community (GPL) Downloads"
3. Choose: "MySQL Community Server"
4. Download the installer (Windows MSI Installer)
5. Run the installer:
   - Choose "Developer Default"
   - Follow the instructions
   - Write down the root password!
6. Also install MySQL Workbench
```

### Installation Verification

```bash
# Open the terminal (CMD) and type:
mysql --version
# Expected output: mysql  Ver 8.0.x for Win64...

# Open MySQL Workbench
# Connect using:
Hostname: localhost
Port: 3306
Username: root
Password: [your password]
```

> 💡 Tip: Think of MySQL as the *warehouse* of your OOP application. Classes are the *catalogs* (tables), objects are the *products* (records), and methods are the *employees* that organize and retrieve these products when needed.

