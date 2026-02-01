# 📚 Aula 2 - Introdução ao MySQL

---

## 🎯 Objetivos da Aula

* Conhecer a origem e a evolução histórica do MySQL
* Entender o modelo de licenciamento e o conceito de software livre
* Entender as subdivisões da linguagem SQL (DDL, DML, DQL, DCL, DTL)
* Entender o conceito de transações e os princípios ACID (D.I.C.A.)
* Conhecer as principais ferramentas utilizadas no ecossistema MySQL

---

## 🕰️ Origem e Evolução Histórica do MySQL

O **MySQL** surgiu em **1994, na Suécia**, criado por **Michael Widenius** (conhecido como *Monty*) e **David Axmark**. Diferente de muitas tecnologias da época, ele foi concebido desde o início como um software **gratuito e de código aberto**.

### 📜 Licenciamento e Filosofia Open Source

O MySQL foi distribuído sob a licença **GPL (General Public License)**, o que garante:

```text
- Liberdade para estudar o código-fonte
- Possibilidade de modificar o software
- Direito de redistribuição
```

> 💡 Esse modelo foi essencial para a rápida adoção do MySQL por empresas, universidades e desenvolvedores independentes.

---

### 🏢 Mudanças Corporativas

A trajetória do MySQL envolve importantes aquisições no setor de tecnologia:

* **2007** – Aquisição pela **Sun Microsystems** (empresa responsável pela criação do Java)
* **2009** – Aquisição da Sun pela **Oracle**, que se torna a atual mantenedora do MySQL

Essa mudança gerou preocupação na comunidade open source.

---

### 🌱 O Surgimento do MariaDB

Como resposta à aquisição pela Oracle, **Monty Widenius** criou o **MariaDB**, um *fork* do MySQL.

```text
Fork = uma nova linha de desenvolvimento
baseada no código original, mas independente
```

Apesar disso:

* O **MySQL** continua sendo a solução mais consolidada
* É amplamente utilizado por grandes organizações

### 🌍 Usuários de Destaque

* Google
* NASA
* Wikipedia
* Facebook (em larga escala)

---

## 🧩 A Linguagem SQL no MySQL

O MySQL utiliza a **SQL (Structured Query Language)**, que é subdividida de acordo com a finalidade dos comandos.

---

### 🏗️ DDL – Data Definition Language

Usada para **definir e modificar a estrutura** do banco de dados.

```sql
-- Exemplos práticos
CREATE DATABASE escola;          -- Criar banco
CREATE TABLE aluno (...);        -- Criar tabela
ALTER TABLE aluno ADD COLUMN ...;-- Modificar tabela
DROP TABLE aluno;                -- Remover tabela
TRUNCATE TABLE aluno;            -- Esvaziar tabela
```

---

### ✏️ DML – Data Manipulation Language

Responsável pela **manipulação direta dos dados** armazenados.

```sql
-- Exemplos práticos
INSERT INTO aluno VALUES (...);  -- Inserir dados
UPDATE aluno SET nome = ...;     -- Atualizar dados
DELETE FROM aluno WHERE ...;     -- Remover dados
```

---

### 🔍 DQL – Data Query Language

Focada em **consultas aos dados**.

```sql
-- Exemplos práticos
SELECT * FROM aluno;             -- Consultar tudo
SELECT nome, idade FROM aluno;   -- Colunas específicas
SELECT * FROM aluno WHERE ...;   -- Com filtros
```
> 📌 Embora alguns autores incluam o SELECT na DML, didaticamente ele é tratado como DQL.

---

## 🔐 DCL – Data Control Language

Gerencia **permissões e controle de acesso** ao banco de dados.

```sql
-- Exemplos práticos
GRANT SELECT ON escola.* TO usuario;  -- Dar permissão
REVOKE DELETE ON escola.* FROM usuario; -- Remover permissão
```

---

### 🔄 DTL – Data Transaction Language

Relacionada ao **controle de transações**, garantindo segurança nas operações.

```sql
-- Exemplos práticos
START TRANSACTION;               -- Iniciar transação
COMMIT;                          -- Confirmar alterações
ROLLBACK;                        -- Desfazer alterações
```

---

## 🔒 Transações e o Conceito D.I.C.A. (ACID)

Para garantir confiabilidade, o MySQL segue os princípios conhecidos como **ACID**, apresentados aqui pelo acrônimo **D.I.C.A.**
