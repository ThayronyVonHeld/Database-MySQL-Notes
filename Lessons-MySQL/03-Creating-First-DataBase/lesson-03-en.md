# 📚 Aula 3 - Estrutura de Bancos de Dados e SQL Básico

---

## 🎯 Objetivos da Aula

* Compreender a estrutura interna de um banco de dados relacional
* Relacionar banco, tabela, registro e campo de forma hierárquica
* Entender a importância do uso de comandos SQL manuais
* Conhecer os principais comandos básicos de SQL
* Identificar os principais tipos primitivos de dados do MySQL
* Compreender o papel da chave primária na integridade dos dados
* Reconhecer o ambiente básico de desenvolvimento com MySQL

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

## 🧩 Comandos Básicos de SQL

Apesar das interfaces gráficas facilitarem o uso do banco de dados, é **fundamental** aprender SQL manualmente.

> 💡 Linguagens como **Java** e **PHP** interagem diretamente com o banco por meio de comandos SQL.

---

### 🏗️ Criação e Seleção de Banco de Dados

```sql
CREATE DATABASE nome_do_banco;
USE nome_do_banco;
```

* Cria o banco de dados (o "navio")
* Define qual banco está ativo

---

### 📦 Criação de Tabelas

```sql
CREATE TABLE nome_da_tabela (
    campo1 TIPO,
    campo2 TIPO
);
```

Define:

* Nome da tabela
* Campos
* Tipos de dados

---

### 🔍 Consultas Estruturais

```sql
DESCRIBE nome_da_tabela;
```

Exibe:

* Estrutura da tabela
* Campos
* Tipos
* Restrições

---

### 📋 Comandos de Listagem

```sql
SHOW DATABASES;
SHOW TABLES;
```

Utilizados para listar bancos e tabelas existentes no servidor.

---

## 🧠 Tipos Primitivos de Dados

Ao criar uma tabela, cada campo precisa de um **tipo de dado**, o que influencia diretamente:

```text
- Espaço em disco
- Desempenho
- Precisão das informações
```

---

### 🔢 Tipos Numéricos

```text
Inteiros:
- TINYINT
- INT
- BIGINT

Reais:
- FLOAT
- DECIMAL
```

> 📌 Exemplo: **TINYINT** é ideal para idades, pois ocupa pouco espaço.

---

### 🔤 Tipos Literais (Caracteres)

#### CHAR (Comprimento Fixo)

```text
- Tamanho fixo
- Espaço não utilizado é preenchido
```

#### VARCHAR (Comprimento Variável)

```text
- Armazena apenas o necessário
- Mais eficiente
- Mais utilizado
```

#### TEXT

```text
- Textos longos
- Descrições
- Observações
```

---

### 📅 Tipos de Data e Tempo

```text
- DATE
- DATETIME
- TIME
- YEAR
```

Utilizados para controle temporal dos dados.

---

### 🧪 Outros Tipos

```text
- Lógicos: BOOLEAN, BIT
- Binários: BLOB (imagens, arquivos)
- Espaciais: dados volumétricos e geográficos
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

## 🛠️ Ambiente de Desenvolvimento

### 🖥️ Servidor MySQL

* Responsável por armazenar e processar os dados
* Executa em segundo plano

---

### 🧰 MySQL Workbench

Interface gráfica para:

```text
- Executar queries
- Criar tabelas
- Visualizar dados
- Administrar o banco
```

---

### 🖤 Terminal (Console)

* Alternativa direta e poderosa
* Muito usada em servidores
* Fundamental para domínio real do MySQL

---

## 📊 Resumo Rápido

* Bancos de dados organizam dados em tabelas
* Tabelas são compostas por registros e campos
* SQL é essencial mesmo com interfaces gráficas
* Tipos de dados impactam desempenho e armazenamento
* Chave primária garante integridade
* MySQL pode ser gerenciado via Workbench ou terminal

---

### 💡 Dica Final

"Dominar bancos de dados começa por entender sua estrutura. Antes de consultar dados complexos, é essencial saber exatamente como eles estão organizados."
