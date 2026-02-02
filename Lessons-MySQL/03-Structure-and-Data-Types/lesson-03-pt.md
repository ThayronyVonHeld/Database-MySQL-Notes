# 📚 Aula 3 - Estrutura e Tipos de Dados no MySQL

---

## 🎯 Objetivos da Aula

* Compreender a estrutura interna de um banco de dados relacional
* Relacionar banco, tabela, registro e campo de forma hierárquica
* Conhecer os comandos básicos de criação e manipulação de estruturas
* Entender a importância fundamental das chaves primárias
* Identificar os principais tipos primitivos de dados do MySQL
* Criar nossas primeiras tabelas com estrutura adequada

---

## 🚢 Analogia Estrutural: O Navio e os Containers

Para facilitar a compreensão da organização de um banco de dados, utilizamos uma analogia com o mundo real.

### 🛳️ O Navio – Banco de Dados

O **navio** representa o **banco de dados** como um todo:

```text
- Um local centralizado
- Organizado
- Capaz de armazenar grandes volumes de dados
```

---

### 📦 Os Containers – Tabelas

Dentro do navio existem **containers**, que representam as **tabelas**.

```text
- Cada container armazena um tipo específico de informação
- Exemplo:
  - Tabela Pessoas
  - Tabela Jogos
```

Cada tabela guarda dados com **características semelhantes**.

---

### 🧾 Registros e Campos

Dentro das tabelas, encontramos:

* **Registros (linhas):** representam uma entidade específica

    * Exemplo: uma pessoa, um jogo, um produto
* **Campos (colunas):** representam as características padronizadas

    * Nome, idade, peso, data de nascimento

> 📌 Em resumo:
> **Bancos de dados são conjuntos de tabelas; tabelas são conjuntos de registros; registros são compostos por campos.**

---

## ⌨️ Comandos Básicos de SQL

Apesar das interfaces gráficas facilitarem o uso do banco de dados, é **fundamental** aprender SQL manualmente.

> 💡 Linguagens como **Java** e **PHP** interagem diretamente com o banco por meio de comandos SQL.

---

### 🏗️ Criação, Listagem e Seleção de Banco de Dados

```sql
-- 1. Criar um banco de dados (navio)
CREATE DATABASE escola;

-- 2. Listar todos os bancos existentes
SHOW DATABASES;

-- 3. Selecionar/Usar um banco específico
USE escola;

-- 4. Excluir um banco de dados (CUIDADO!)
DROP DATABASE escola;

-- 5. Verificar qual banco está em uso
SELECT DATABASE();
```

---

### 📦 Criação de Tabelas e Consultas Estruturais

```sql
-- 1. Criar uma tabela (container)
CREATE TABLE aluno (
    id INT,
    nome VARCHAR(100),
    idade INT
);

-- 2. Listar tabelas do banco atual
SHOW TABLES;

-- 3. Ver estrutura detalhada de uma tabela
DESCRIBE aluno;      -- Forma completa
DESC aluno;          -- Forma abreviada

-- 4. Excluir uma tabela
DROP TABLE aluno;

-- 5. Renomear uma tabela
RENAME TABLE aluno TO estudantes;
```

---

## 📊 Tipos Primitivos de Dados

Ao criar uma tabela, cada campo precisa de um **tipo de dado**, o que influencia diretamente:

```text
- Espaço em disco
- Desempenho
- Precisão das informações
```

---

### 🔢 Tipos Numéricos

```sql
-- INTEIROS - Escolha pelo tamanho necessário
CREATE TABLE exemplos_numericos (
    -- Para idades (0-120) - 1 byte
    idade TINYINT,           -- 0 a 255 (3 bytes economizados!)
    
    -- Para IDs normais - 4 bytes
    id INT 
    
    -- Para números muito grandes (ex: CPF sem formatação)
    cpf BIGINT,                     -- 8 bytes
    
    -- Para preços/dinheiro - PRECISÃO!
    preco DECIMAL(10, 2),             -- Ex: 99999999.99
    preco_aprox FLOAT(8, 2)           -- Para cálculos científicos
);
```

**Regra de Ouro**: Use o menor tipo que atenda sua necessidade!

---

### 🔤 Tipos Literais (Caracteres)

```sql
CREATE TABLE exemplos_texto (
    -- CHAR: Tamanho fixo (preenchido com espaços)
    sexo CHAR(1),                     -- 'M' ou 'F' - SEMPRE 1 byte
    uf CHAR(2),                       -- 'SP', 'RJ' - SEMPRE 2 bytes
    
    -- VARCHAR: Tamanho variável (mais comum)
    nome VARCHAR(100),                -- Até 100 caracteres
    endereco VARCHAR(255),            -- Tamanho comum para endereços
    
    -- TEXT: Para textos MUITO longos
    descricao TEXT,                   -- Até 65,535 caracteres
    historia LONGTEXT                 -- Até 4GB de texto!
);
```

**Comparação CHAR vs VARCHAR**:
```sql
-- CHAR(10) com valor 'OK'
'OK        '  -- 10 bytes (8 espaços extras)

-- VARCHAR(10) com valor 'OK'
'OK'          -- 2 bytes + 1 byte de controle = 3 bytes
```

---

### 📅 Tipos de Data e Tempo

```sql
CREATE TABLE exemplos_data (
    -- Apenas data
    data_nascimento DATE,             -- '2005-05-15'
    
    -- Data e hora completas
    data_cadastro DATETIME,           -- '2024-01-31 14:30:00'
    data_atualizacao TIMESTAMP,       -- Auto-atualiza com NOW()
    
    -- Apenas hora
    hora_entrada TIME,                -- '08:30:00'
    
    -- Apenas ano
    ano_fabricacao YEAR               -- 2024
);
```

---

### 🧪 Outros Tipos

```sql
CREATE TABLE exemplos_especiais (
    -- Boolean (na verdade é TINYINT(1))
    ativo BOOLEAN DEFAULT TRUE,       -- TRUE = 1, FALSE = 0
    
    -- Binários (imagens, PDFs, etc)
    foto BLOB,                        -- Até 65KB
    documento MEDIUMBLOB,             -- Até 16MB
    
    -- Valores pré-definidos
    status ENUM('ativo', 'inativo', 'pendente'),
    tamanho SET('P', 'M', 'G', 'GG')
);
```
---

## 🔑 A Importância da Chave Primária

A **chave primária (PRIMARY KEY)** identifica **unicamente** cada registro de uma tabela.

### ⚠️ Sem chave primária:

```text
- Registros duplicados
- Dados inconsistentes
- Falta de controle
```

### ✅ Com chave primária:

```text
- Identificação única
- Integridade dos dados
- Base para relacionamentos futuros
```

---

## 📊 Resumo Rápido

* Bancos de dados organizam dados em tabelas
* Tabelas são compostas por registros e campos
* SQL é essencial mesmo com interfaces gráficas
* Tipos de dados impactam desempenho e armazenamento
* Chave primária garante integridade
* MySQL pode ser gerenciado via Workbench ou terminal

---

> 💡 Dica: "Pergunte-se sempre: 'Que operações farei com este dado?' Isso define o tipo ideal. Um CPF é número, mas você nunca fará cálculos com ele - use VARCHAR! Uma idade é pequena - use TINYINT! Economize bytes, ganhe performance."