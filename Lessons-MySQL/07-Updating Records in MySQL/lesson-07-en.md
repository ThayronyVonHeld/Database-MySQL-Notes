# 📚 Lesson 7 — Updating Records in MySQL (UPDATE)

---

## 🎯 Lesson Objectives

* Understand database terminology: rows, records, and tuples
* Learn the complete structure of the **UPDATE** command
* Understand the importance of the **WHERE** clause
* Implement safety measures to avoid accidental changes
* Practice updating multiple columns at once
* Understand risks and learn error-prevention techniques

---

## 🧾 Rows, Records, and Tuples

In relational databases, some terms are equivalent:

```text
Row = Record = Tuple
Column = Field = Attribute
```

While the command:

```sql
ALTER TABLE
```

modifies the **structure of columns**, the command:

```sql
UPDATE
```

modifies the **content of records**.

---

## ✏️ The UPDATE Command

The `UPDATE` command allows you to change existing data in a table.

Basic structure:

```sql
UPDATE table_name
SET column1 = value1,
    column2 = value2,
    ...
WHERE condition;
```

---

## 📘 Step-by-Step Practical Example

### 1️⃣ Create a Sample Table

```sql
CREATE TABLE IF NOT EXISTS employee (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50),
    salary DECIMAL(10,2),
    department VARCHAR(50),
    hire_date DATE,
    active BOOLEAN DEFAULT TRUE
);
```

---

### 2️⃣ Insert Test Data

```sql
INSERT INTO employee (name, position, salary, department, hire_date) 
VALUES 
    ('John Silva', 'Analyst', 3500.00, 'IT', '2023-01-15'),
    ('Maria Santos', 'Manager', 5500.00, 'HR', '2022-03-10'),
    ('Pedro Oliveira', 'Developer', 4200.00, 'IT', '2023-06-20'),
    ('Ana Costa', 'Analyst', 3700.00, 'Finance', '2023-08-05');
```

---

### 3️⃣ Simple Update (Single Column)

```sql
-- John was promoted to Coordinator
UPDATE employee
SET position = 'Coordinator'
WHERE name = 'John Silva';  -- Updates only John
```

---

### 4️⃣ Multiple Column Update

```sql
-- Maria changed department and received a raise
UPDATE employee
SET department = 'Administration',
    salary = 6000.00,
    position = 'Senior Manager'
WHERE name = 'Maria Santos';
```

---

### 5️⃣ Update with Expressions

```sql
-- 10% salary increase for everyone in the IT department
UPDATE employee
SET salary = salary * 1.10  -- Mathematical expression
WHERE department = 'IT';
```

---

### 6️⃣ Update with MySQL Functions

```sql
-- Fix name formatting (capitalize first letter)
UPDATE employee
SET name = CONCAT(
    UPPER(SUBSTRING(name, 1, 1)),
    LOWER(SUBSTRING(name, 2))
);
```

---

### 7️⃣ Conditional Update with CASE

```sql
-- Different raise percentages based on position
UPDATE employee
SET salary = CASE
    WHEN position LIKE '%Manager%' THEN salary * 1.15
    WHEN position LIKE '%Coordinator%' THEN salary * 1.12
    WHEN position LIKE '%Analyst%' THEN salary * 1.10
    ELSE salary * 1.05
END
WHERE active = TRUE;
```

---

### 8️⃣ Verify Results

```sql
SELECT * FROM employee ORDER BY department, name;
```

---

## 🔑 The Importance of the WHERE Clause

The **WHERE** clause is the most important part of the `UPDATE` command.

Without it:

```sql
UPDATE courses
SET year = 2025;
```

Result:

```text
ALL records in the table will be modified.
```

This can cause irreversible data loss.

---

## 🔐 Security and the Use of the Primary Key

The safest way to update records is by using the **PRIMARY KEY**.

### Why Is the Primary Key Essential?

❌ DANGEROUS SCENARIO: Updating without a primary key

```sql
UPDATE employee
SET position = 'Intern'
WHERE name = 'John Silva';
```

Problems:

* What if there is another "John Silva"?
* What if "John Silva" changes their name later?

---

✅ SAFE SCENARIO: Always use the primary key

```sql
UPDATE employee
SET position = 'Coordinator'
WHERE id = 1;  -- ID is UNIQUE and IMMUTABLE
```

---

✅ BEST PRACTICE: Select before updating

```sql
-- First, check what will be updated
SELECT * FROM employee WHERE name LIKE 'John%';
```

Then update using the correct ID:

```sql
UPDATE employee
SET position = 'Coordinator'
WHERE id = 1;
```

---

## 🛡️ LIMIT as Extra Protection

We can limit the number of affected records:

```sql
UPDATE courses
SET workload = 25
WHERE year = 2024
LIMIT 1;
```

Even if the `WHERE` clause is incorrect, only one row will be modified.

---

## 🔒 Safe Updates (MySQL Workbench)

MySQL Workbench includes a safety mode called:

```text
Safe Updates
```

It prevents `UPDATE` and `DELETE` commands that:

```text
- Do not use WHERE
- Do not use a primary key
```

This helps prevent accidents during development.

---

## 💾 The Importance of Backups

Handling data requires responsibility.

Best practices:

```text
- Perform a backup before major changes
- Test commands with SELECT first
- Use WHERE with the primary key
- Use LIMIT whenever possible
```

Example of testing before an UPDATE:

```sql
SELECT * FROM courses
WHERE course_id = 3;
```

Then:

```sql
UPDATE courses
SET workload = 30
WHERE course_id = 3;
```

---

## 📊 Quick Summary

* `UPDATE` modifies existing records
* `SET` defines the new values
* `WHERE` defines which records will be changed
* The **primary key** is the safest way to update
* `LIMIT` adds extra protection
* Safe Updates prevents dangerous commands
* Backup is essential before major changes

---

> 💡 Tip: Before updating data, always ask yourself: *Am I absolutely sure which records will be affected?* If in doubt, run a `SELECT` first.
