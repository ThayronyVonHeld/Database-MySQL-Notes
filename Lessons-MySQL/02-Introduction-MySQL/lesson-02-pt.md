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

---

### D – Durabilidade

Após a confirmação de uma transação, os dados devem **permanecer armazenados**, mesmo em caso de falhas.

```sql
-- Exemplo em MySQL
START TRANSACTION;
UPDATE conta SET saldo = saldo - 100 WHERE id = 1;
UPDATE conta SET saldo = saldo + 100 WHERE id = 2;
COMMIT;  -- Agora é permanente!
```
---

### I – Isolamento

Transações simultâneas **não devem interferir umas nas outras**.

```sql
-- Usuário A (às 10:00:00)
START TRANSACTION;
SELECT saldo FROM conta WHERE id = 1;  -- Vê R$ 500,00

-- Usuário B (às 10:00:01)
START TRANSACTION;
UPDATE conta SET saldo = 400 WHERE id = 1;
COMMIT;

-- Usuário A ainda vê R$ 500,00 até COMMIT
```
---

### C – Consistência

O banco de dados deve sempre sair de um **estado válido** para outro estado válido.

```sql
-- Estado válido: Saldo nunca negativo
CREATE TABLE conta (
    id INT PRIMARY KEY,
    saldo DECIMAL(10,2) CHECK (saldo >= 0)  -- Restrição
);

-- Transação rejeitada se violar consistência
UPDATE conta SET saldo = -50 WHERE id = 1;  -- ERRO!
```

---

### A – Atomicidade

Princípio do **tudo ou nada**.

```sql
START TRANSACTION;
-- Operação 1: OK
UPDATE estoque SET quantidade = quantidade - 1 WHERE produto_id = 5;

-- Operação 2: FALHA (produto não existe)
INSERT INTO venda (produto_id, quantidade) VALUES (999, 1);

-- Como a segunda falhou, tudo é desfeito
ROLLBACK;  -- Atomicidade em ação!
```
> 💡 Funciona como um "Ctrl + Z" interno do banco de dados.
---

### Por que transações são importantes?
```text
Exemplo do Caixa Eletrônico:
1. Você solicita R$ 100,00
2. Sistema verifica saldo (tem R$ 500,00)
3. Sistema debita R$ 100,00 da sua conta
4. Sistema libera R$ 100,00 no caixa

Se falhar no passo 3 ou 4: Problema!
```

---

## 🛠️ Ferramentas do Ecossistema MySQL

### 🖥️ MySQL Server

```text
Função: O motor do banco de dados
Características:
- Serviço que roda em background
- Escuta conexões (normalmente porta 3306)
- Processa comandos SQL
- Gerencia dados em disco
```

---

### 🧰 MySQL Workbench (Interface Gráfica)

```text
Vantagens sobre terminal:
- Interface visual amigável
- Editor SQL com highlight
- Design visual de tabelas
- Administração gráfica
- Exportação/Importação visual
- Modelagem de dados (EER Diagrams)
```

> ✅ Mais produtivo e amigável que o uso exclusivo do terminal.

---

### 🧠 Terminal/CLI (Command Line Interface)
```bash
# Comandos básicos no terminal
mysql --version                    # Verificar versão
mysql -u root -p                   # Conectar ao servidor
mysql -h localhost -u usuario -p   # Conectar com host

# Dentro do MySQL CLI
SHOW DATABASES;                    # Listar bancos
USE nome_banco;                    # Selecionar banco
SHOW TABLES;                       # Listar tabelas
EXIT; ou \q                        # Sair
```

---

### 🗃️ Documentação Oficial
```text
Site: https://dev.mysql.com/doc/
Conteúdo:
- Manual completo
- Tutoriais passo a passo
- Referência de comandos
- Exemplos práticos
- Notas de versão
```

---

## 📥 Instalação Prática

### Passo a Passo para Windows

```text
1. Acesse: https://www.mysql.com/downloads/
2. Selecione: "MySQL Community (GPL) Downloads"
3. Escolha: "MySQL Community Server"
4. Baixe o instalador (Windows MSI Installer)
5. Execute o instalador:
   - Escolha "Developer Default"
   - Siga as instruções
   - Anote a senha do root!
6. Instale também o MySQL Workbench
```

### Verificação da Instalação
```bash
# Abra o terminal (CMD) e digite:
mysql --version
# Deve mostrar: mysql  Ver 8.0.x for Win64...

# Inicie o MySQL Workbench
# Conecte usando:
Hostname: localhost
Port: 3306
Username: root
Password: [sua senha]
```

> 💡 Dica: Pense no MySQL como o 'armazém' da sua aplicação POO. As classes são os 'catálogos' (tabelas), os objetos são os 'produtos' (registros), e os métodos são os 'funcionários' que organizam e recuperam esses produtos quando necessário."
