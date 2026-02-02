# 📚 Lesson 3 – Structure and Data Types in MySQL

---

## 🎯 Lesson Objectives

* Understand the internal structure of a relational database
* Relate database, table, record, and field in a hierarchical way
* Learn the basic commands for creating and managing structures
* Understand the fundamental importance of primary keys
* Identify the main primitive data types in MySQL
* Create our first tables with a proper structure

---

## 🚢 Structural Analogy: The Ship and the Containers

To make database organization easier to understand, we use a real-world analogy.

### 🛳️ The Ship – Database

The **ship** represents the **database** as a whole:

```text
- A centralized location
- Organized
- Capable of storing large volumes of data
```

---

### 📦 The Containers – Tables

Inside the ship, there are **containers**, which represent **tables**.

```text
- Each container stores a specific type of information
- Example:
  - People table
  - Games table
```

Each table stores data with **similar characteristics**.

---

### 🧾 Records and Fields

Inside tables, we find:

* **Records (rows):** represent a specific entity

  * Example: a person, a game, a product
* **Fields (columns):** represent standardized characteristics

  * Name, age, weight, date of birth

> 📌 In summary:
> **Databases are collections of tables; tables are collections of records; records are composed of fields.**

---

## ⌨️ Basic SQL Commands

Even though graphical interfaces make database usage easier, it is **essential** to learn SQL manually.

> 💡 Languages such as **Java** and **PHP** interact directly with the database through SQL commands.

---

### 🏗️ Creating, Listing, and Selecting Databases

```sql
-- 1. Create a database (ship)
CREATE DATABASE school;

-- 2. List all existing databases
SHOW DATABASES;

-- 3. Select / use a specific database
USE school;

-- 4. Delete a database (BE CAREFUL!)
DROP DATABASE school;

-- 5. Check which database is currently in use
SELECT DATABASE();
```

---

### 📦 Creating Tables and Structural Queries

```sql
-- 1. Create a table (container)
CREATE TABLE student (
    id INT,
    name VARCHAR(100),
    age INT
);

-- 2. List tables in the current database
SHOW TABLES;

-- 3. View detailed table structure
DESCRIBE student;     -- Full form
DESC student;         -- Short form

-- 4. Delete a table
DROP TABLE student;

-- 5. Rename a table
RENAME TABLE student TO students;
```

---

## 📊 Primitive Data Types

When creating a table, each field needs a **data type**, which directly affects:

```text
- Disk space usage
- Performance
- Information accuracy
```

---

### 🔢 Numeric Types

```sql
-- INTEGERS – Choose based on required size
CREATE TABLE numeric_examples (
    -- For ages (0–120) – 1 byte
    age TINYINT,              -- 0 to 255 (3 bytes saved!)
    
    -- For regular IDs – 4 bytes
    id INT,
    
    -- For very large numbers (e.g., unformatted national IDs)
    national_id BIGINT,       -- 8 bytes
    
    -- For prices/money – PRECISION!
    price DECIMAL(10, 2),     -- e.g., 99999999.99
    approx_price FLOAT(8, 2)  -- For scientific calculations
);
```

**Golden Rule**: Use the smallest type that meets your needs!

---

### 🔤 Character (Text) Types

```sql
CREATE TABLE text_examples (
    -- CHAR: Fixed length (padded with spaces)
    gender CHAR(1),            -- 'M' or 'F' – ALWAYS 1 byte
    state CHAR(2),             -- 'CA', 'NY' – ALWAYS 2 bytes
    
    -- VARCHAR: Variable length (most common)
    name VARCHAR(100),         -- Up to 100 characters
    address VARCHAR(255),      -- Common size for addresses
    
    -- TEXT: For VERY long texts
    description TEXT,          -- Up to 65,535 characters
    history LONGTEXT           -- Up to 4GB of text!
);
```

**CHAR vs VARCHAR Comparison**:

```sql
-- CHAR(10) with value 'OK'
'OK        '  -- 10 bytes (8 extra spaces)

-- VARCHAR(10) with value 'OK'
'OK'          -- 2 bytes + 1 control byte = 3 bytes
```

---

### 📅 Date and Time Types

```sql
CREATE TABLE date_examples (
    -- Date only
    birth_date DATE,              -- '2005-05-15'
    
    -- Full date and time
    created_at DATETIME,          -- '2024-01-31 14:30:00'
    updated_at TIMESTAMP,         -- Auto-updates with NOW()
    
    -- Time only
    entry_time TIME,              -- '08:30:00'
    
    -- Year only
    manufacture_year YEAR         -- 2024
);
```

---

### 🧪 Other Types

```sql
CREATE TABLE special_examples (
    -- Boolean (actually TINYINT(1))
    active BOOLEAN DEFAULT TRUE,  -- TRUE = 1, FALSE = 0
    
    -- Binary data (images, PDFs, etc.)
    photo BLOB,                   -- Up to 65KB
    document MEDIUMBLOB,          -- Up to 16MB
    
    -- Predefined values
    status ENUM('active', 'inactive', 'pending'),
    size SET('S', 'M', 'L', 'XL')
);
```

---

## 🔑 The Importance of the Primary Key

The **primary key (PRIMARY KEY)** uniquely identifies each record in a table.

### ⚠️ Without a primary key:

```text
- Duplicate records
- Inconsistent data
- Lack of control
```

### ✅ With a primary key:

```text
- Unique identification
- Data integrity
- Foundation for future relationships
```

---

## 📊 Quick Summary

* Databases organize data into tables
* Tables are composed of records and fields
* SQL is essential even when using graphical interfaces
* Data types impact performance and storage
* Primary keys ensure integrity
* MySQL can be managed via Workbench or the terminal

---

> 💡 Tip: “Always ask yourself: *‘What operations will I perform with this data?’* That defines the ideal type. A national ID is numeric, but you never perform calculations on it — use VARCHAR! An age is small — use TINYINT! Save bytes, gain performance.”

