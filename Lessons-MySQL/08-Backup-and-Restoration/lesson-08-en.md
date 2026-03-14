# 📚 Lesson 8 — Backup and Restoration in MySQL (Dump)

---

## 🎯 Lesson Objectives

* Understand the importance of backup in databases
* Understand the concept of **Dump**
* Learn how to export databases in MySQL Workbench
* Practice importing backups for data recovery
* Differentiate between database server and interface tool
* Develop responsibility when handling data

---

## 💾 Why Backup is Essential

Unlike text editors, MySQL **does not have Control + Z**.

Real Examples:

```sql
-- SCENARIO 1: DELETE without WHERE (every DBA's nightmare)
DELETE FROM clientes;  
-- Result: ALL clients disappeared!

-- SCENARIO 2: Accidental DROP DATABASE
DROP DATABASE empresa;
-- Result: The entire company disappeared from the server!

-- SCENARIO 3: UPDATE with wrong condition
UPDATE produtos SET preco = 0 WHERE categoria = 'eletronicos';
-- Result: All electronic products are now free!

-- SCENARIO 4: Environment mistake
-- Tried to run a production script in the development environment
-- But ran it in the wrong environment... and deleted real data!
```

Without backup:

```text
There is no recovery.
```

That is why every professional must master the backup process.

---

## 🧠 What is a Dump?

In the database context, a backup is called:

```text
DUMP
```

### Anatomy of a Dump File

```sql
-- Real example of what a dump.sql file looks like
-- (This is TEXT, not a binary image!)

-- MariaDB dump 10.19  Distrib 10.4.28-MariaDB
-- Host: localhost    Database: escola
-- ------------------------------------------------------

-- Create the database if it does not exist
CREATE DATABASE IF NOT EXISTS `escola`;
USE `escola`;

-- Remove existing tables to recreate them cleanly
DROP TABLE IF EXISTS `aluno`;

-- Recreate the table structure
CREATE TABLE `aluno` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `nome` varchar(100) NOT NULL,
    `email` varchar(100) DEFAULT NULL,
    `data_nascimento` date DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4;

-- Insert the data again
INSERT INTO `aluno` VALUES 
(1, 'João Silva', 'joao@email.com', '2005-03-15'),
(2, 'Maria Santos', 'maria@email.com', '2004-07-22'),
(3, 'Pedro Oliveira', 'pedro@email.com', '2006-01-30');

-- Dump completed on 2024-01-31 15:30:00
```

### Why is Dump a Text File?

```text
✅ ADVANTAGES OF TEXT FORMAT (.sql):
- Human readable (can open in Notepad!)
- Editable (fix data before restoring)
- Versionable (git diff shows changes)
- Portable (works on any MySQL/MariaDB)
- Compressible (can become .zip, .gz)
- Searchable (grep to find something)

❌ DISADVANTAGES:
- Large files can be heavy
- Import is slower than binary formats
```

---

## 📤 Export (Generating the Backup)

In MySQL Workbench:

```text
Server → Data Export
```

---

### 🔎 Object Selection

You can choose:

```text
- The entire database (Schema)
- Only specific tables
```

---

### ⚙️ Export Options

#### Dump Structure and Data

```text
Exports structure + records
```

#### Dump Structure Only

```text
Exports only the tables (no data)
```

#### Dump Data Only

```text
Exports only the records
```

---

### 📄 Self-contained File

Recommended option:

```text
Export to Self-contained File
```

Generates a single `.sql` file, making transport and storage easier.

---

### 🏗️ Include Create Schema

This option is fundamental.

If checked, the dump will include:

```sql
CREATE DATABASE nome_do_banco;
```

This allows the database to be automatically restored on another server.

---

## 📥 Import (Restoring the Database)

In MySQL Workbench:

```text
Server → Data Import
```

Steps:

1. Select the `.sql` file
2. Choose the destination
3. Click **Start Import**

After the import:

```text
Refresh the Schemas list
```

The database will reappear on the server.

---

## 🖥️ Tool vs Server

It is fundamental to understand:

```text
MySQL Workbench ≠ Database
```

Workbench is only a graphical interface.

The real database is in the:

```text
MySQL Server
```

In a study environment, it may be running through:

```text
WAMP Server
```

The dump allows moving data between different servers.

---

## 🔄 Real Professional Workflow

```text
1. Developer creates database locally
2. Generates dump (.sql)
3. Sends it to the production server
4. Imports it on the final server
```

Or even:

```text
Download a test database from the internet
Import it for study
Explore queries
```

---

## ⚠️ Best Practices

Before:

```text
- Large changes
- Mass updates
- Structural deletions
```

Always:

```text
✔ Make a backup
✔ Test in a development environment
✔ Confirm the correct server
```

---

## 📋 Quick Summary

* Dump is a `.sql` file with reconstruction commands
* MySQL does not have undo
* Export is done in **Server → Data Export**
* Import is done in **Server → Data Import**
* Workbench is a tool, not a server
* Backup is a professional responsibility

---

> 💡 Tip: There are two types of database administrators: those who have already lost data and those who will. The difference is who has a backup and who does not.
