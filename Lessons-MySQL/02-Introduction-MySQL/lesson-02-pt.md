# 📚 Aula 2 - Introdução ao MySQL

---

## 🎯 Objetivos da Aula

* Conhecer a origem e a evolução histórica do MySQL
* Entender o modelo de licenciamento e o conceito de software livre
* Compreender a relação entre MySQL, Oracle e MariaDB
* Identificar as subdivisões da linguagem SQL utilizadas pelo MySQL
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

```text
Exemplos:
- CREATE DATABASE
- CREATE TABLE
- ALTER TABLE
- DROP TABLE
```

---

### ✏️ DML – Data Manipulation Language

Responsável pela **manipulação direta dos dados** armazenados.

```text
Exemplos:
- INSERT
- UPDATE
- DELETE
```

---

### 🔍 DQL – Data Query Language

Focada em **consultas aos dados**.

```text
Comando principal:
- SELECT
```

> 📌 Embora alguns autores incluam o SELECT na DML, didaticamente ele é tratado como DQL.

---

### 🔐 DCL – Data Control Language

Gerencia **permissões e controle de acesso** ao banco de dados.

```text
Exemplos:
- GRANT
- REVOKE
```

---

### 🔄 DTL – Data Transaction Language

Relacionada ao **controle de transações**, garantindo segurança nas operações.

```text
Exemplos:
- COMMIT
- ROLLBACK
- SAVEPOINT
```

---

## 🧠 Transações e o Conceito D.I.C.A. (ACID)

Para garantir confiabilidade, o MySQL segue os princípios conhecidos como **ACID**, apresentados aqui pelo acrônimo **D.I.C.A.**

---

### D – Durabilidade

Após a confirmação de uma transação, os dados devem **permanecer armazenados**, mesmo em caso de falhas.

---

### I – Isolamento

Transações simultâneas **não devem interferir umas nas outras**.

```text
Exemplo:
Dois usuários atualizando dados ao mesmo tempo
não podem causar inconsistência
```

---

### C – Consistência

O banco de dados deve sempre sair de um **estado válido** para outro estado válido.

---

### A – Atomicidade

Princípio do **tudo ou nada**.

```text
- Se todas as operações da transação ocorrerem: OK
- Se qualquer operação falhar: tudo é desfeito (ROLLBACK)
```

> 💡 Funciona como um "Ctrl + Z" interno do banco de dados.

---

## 🛠️ Ferramentas do Ecossistema MySQL

### 🖥️ MySQL Server

* O serviço responsável por armazenar e processar os dados
* Executa em segundo plano
* É o núcleo do banco de dados

---

### 🧰 MySQL Workbench

Ferramenta gráfica que permite:

```text
- Criar e gerenciar bancos de dados
- Executar comandos SQL
- Administrar usuários
- Visualizar diagramas
```

> ✅ Mais produtivo e amigável que o uso exclusivo do terminal.

---

### 📚 Documentação Oficial

* [https://dev.mysql.com/doc/](https://dev.mysql.com/doc/)

### ⬇️ Instalação

* [https://www.mysql.com/downloads/](https://www.mysql.com/downloads/)

---

## 📊 Resumo Rápido

* MySQL surgiu em 1994 com foco em software livre
* É mantido atualmente pela Oracle
* MariaDB é um fork criado como alternativa comunitária
* SQL é subdividida em DDL, DML, DQL, DCL e DTL
* Transações seguem os princípios D.I.C.A. (ACID)
* MySQL Server e Workbench são as principais ferramentas

---

### 💡 Dica Final

"Aprender MySQL não é apenas aprender comandos SQL, mas entender como os dados são protegidos, organizados e manipulados de forma segura dentro de um sistema."
