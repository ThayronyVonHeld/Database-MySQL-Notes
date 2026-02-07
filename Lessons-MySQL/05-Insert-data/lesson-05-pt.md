# 📚 Aula 5 — Inserindo Dados com INSERT INTO (MySQL)

---

## 🎯 Objetivos da Aula

* Entender a diferença entre comandos **DDL** e **DML**
* Aprender a inserir dados em tabelas com **INSERT INTO**
* Trabalhar corretamente com **datas, textos e números**
* Otimizar inserções para performance e manutenibilidade
* Implementar boas práticas na manipulação de informações.


---
## 📊 Classificação de Comandos SQL: DDL vs. DML

```mermaid
graph TD
    A[Comandos SQL] --> B[DDL - Data Definition Language]
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

### DDL vs. DML - Comparação Detalhada

| Característica | **DDL** (Data Definition) | **DML** (Data Manipulation) |
|----------------|---------------------------|----------------------------|
| **Foco** | Estrutura do banco | Conteúdo dos dados |
| **Quando usar** | Design/Setup inicial | Operações do dia a dia |
| **Auto-commit** | Sim (implícito) | Depende (controlado por transação) |
| **Exemplos** | `CREATE`, `ALTER`, `DROP` | `INSERT`, `UPDATE`, `DELETE` |
| **Analogia** | Plantar a árvore (estrutura) | Colher frutos (dados) |

---

### Exemplos Práticos da Diferença

```sql
-- 🏗️ DDL - CRIANDO A ESTRUTURA
CREATE DATABASE escola;
USE escola;

CREATE TABLE aluno (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    data_nascimento DATE,
    altura DECIMAL(3,2),
    ativo BOOLEAN DEFAULT TRUE
);

-- 📝 DML - INSERINDO OS DADOS
INSERT INTO aluno (nome, data_nascimento, altura) 
VALUES ('João Silva', '2005-05-15', 1.75);

-- 📝 Mais DML - ATUALIZANDO DADOS
UPDATE aluno SET altura = 1.78 WHERE id = 1;

-- 📝 Mais DML - REMOVENDO DADOS
DELETE FROM aluno WHERE id = 1;
```

---

## 🎯 O Comando INSERT INTO

O comando `INSERT INTO` é usado para inserir registros em uma tabela.

```sql
INSERT INTO nome_tabela 
    (campo1, campo2, campo3, ...) 
VALUES 
    (valor1, valor2, valor3, ...);
```

### Exemplo Prático Passo a Passo

```sql
-- 1. Criar a tabela (DDL)
CREATE TABLE funcionario (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    cargo VARCHAR(50),
    salario DECIMAL(10,2),
    data_admissao DATE,
    ativo BOOLEAN DEFAULT TRUE
);

-- 2. Inserir dados (DML) - FORMA EXPLÍCITA (RECOMENDADA)
INSERT INTO funcionario 
    (nome, cargo, salario, data_admissao) 
VALUES 
    ('Maria Santos', 'Analista', 3500.00, '2023-06-10');

-- 3. Verificar inserção
SELECT * FROM funcionario;
```

### Formatos de Dados no INSERT

```sql
-- 📝 TEXTO: Sempre aspas simples
INSERT INTO funcionario (nome) VALUES ('João "Joca" Silva');

-- 🔢 NÚMERO: Pode ter ou não aspas (MySQL aceita ambos)
INSERT INTO funcionario (salario) VALUES (2500.50);    -- Sem aspas
INSERT INTO funcionario (salario) VALUES ('2500.50');  -- Com aspas (também funciona)

-- 📅 DATA: Aspas simples + formato YYYY-MM-DD
INSERT INTO funcionario (data_admissao) VALUES ('2024-01-31');

-- ⚠️ DATAS INCORRETAS - Problemas comuns
INSERT INTO funcionario (data_admissao) VALUES ('31-01-2024');  -- ERRO!
INSERT INTO funcionario (data_admissao) VALUES ('2024-13-01');  -- ERRO! Mês 13
INSERT INTO funcionario (data_admissao) VALUES ('2024-02-30');  -- ERRO! Fevereiro tem 28/29

-- ✅ BOOLEAN: TRUE/FALSE ou 1/0
INSERT INTO funcionario (nome, ativo) VALUES ('Inativo', FALSE);
INSERT INTO funcionario (nome, ativo) VALUES ('Ativo', TRUE);
INSERT INTO funcionario (nome, ativo) VALUES ('Ativo2', 1);     -- Equivalente
INSERT INTO funcionario (nome, ativo) VALUES ('Inativo2', 0);   -- Equivalente
```

---

## 🤖 Otimizações: AUTO_INCREMENT e DEFAULT

Se a tabela foi criada assim:

```sql
CREATE TABLE aluno (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    nascimento DATE,
    sexo ENUM('M','F'),
    peso DECIMAL(5,2),
    altura DECIMAL(3,2),
    nacionalidade VARCHAR(20) DEFAULT 'Brasil'
);
```

Não precisamos informar o `id` manualmente:

```sql
INSERT INTO aluno (nome, nascimento, sexo, peso, altura)
VALUES ('Ana', '2005-07-21', 'F', 60.00, 1.65);
```

O MySQL irá:

```text
- Gerar o ID automaticamente
- Inserir "Brasil" como nacionalidade (DEFAULT)
```
---

## ⚡ Inserção Simplificada (Sem Informar Campos)

Se todos os valores forem inseridos exatamente na ordem da tabela:

```sql
INSERT INTO aluno
VALUES (DEFAULT, 'João', '2003-01-10', 'M', 80.00, 1.75, 'Brasil');
```

Embora funcione, **não é a forma mais segura** em projetos reais.

> 💡 Boa prática: sempre informar os campos explicitamente.

---

## 📚 Inserindo Vários Registros

O MySQL permite inserir múltiplas linhas em um único comando:

```sql
INSERT INTO aluno (nome, nascimento, sexo, peso, altura, nacionalidade)
VALUES
('Lucas', '2002-05-10', 'M', 70.00, 1.70, 'Brasil'),
('Marina', '2004-11-03', 'F', 55.00, 1.60, 'Brasil'),
('Pedro', '2001-02-18', 'M', 90.00, 1.85, 'Brasil');
```

Isso é mais eficiente e reduz o número de comandos enviados ao servidor.

---

## 🛡️ Boas Práticas na Inserção de Dados

As regras definidas na tabela continuam valendo durante a inserção:

```text
NOT NULL → impede campos obrigatórios vazios
DEFAULT → define valores automáticos
PRIMARY KEY → impede duplicação de identificadores
ENUM → restringe valores possíveis
```

### 1. Constraints para Qualidade dos Dados

```sql
CREATE TABLE cliente_protegido (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cpf VARCHAR(11) UNIQUE NOT NULL,      -- Único e obrigatório
    nome VARCHAR(100) NOT NULL,           -- Obrigatório
    email VARCHAR(100) UNIQUE,            -- Único se informado
    data_nascimento DATE NOT NULL,
    data_cadastro DATETIME DEFAULT CURRENT_TIMESTAMP, -- Automático
    nacionalidade VARCHAR(30) DEFAULT 'Brasil',       -- Padrão
    ativo BOOLEAN DEFAULT TRUE,           -- Padrão
    CHECK (data_nascimento < CURDATE())   -- Validação
);

-- ✅ Inserções que funcionam:
INSERT INTO cliente_protegido (cpf, nome, data_nascimento)
VALUES ('12345678901', 'Ana Silva', '1995-08-22');

-- ❌ Inserções que FALHAM (protegidas por constraints):
INSERT INTO cliente_protegido (cpf, nome) 
VALUES ('12345678902', NULL);  -- ERRO: nome NOT NULL

INSERT INTO cliente_protegido (cpf, nome, data_nascimento) 
VALUES ('12345678901', 'Outro', '2000-01-01');  -- ERRO: cpf duplicado

INSERT INTO cliente_protegido (cpf, nome, data_nascimento) 
VALUES ('12345678903', 'Futuro', '2030-01-01');  -- ERRO: CHECK constraint
```

### 2. Tratamento de Valores Nulos vs. DEFAULT

```sql
CREATE TABLE exemplo_default (
    id INT PRIMARY KEY AUTO_INCREMENT,
    status VARCHAR(20) DEFAULT 'pendente',
    quantidade INT DEFAULT 1,
    data_registro DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Diferença entre NULL e DEFAULT:
INSERT INTO exemplo_default (status, quantidade) 
VALUES (NULL, NULL);  
-- Resultado: status=NULL, quantidade=NULL, data_registro=CURRENT_TIMESTAMP

INSERT INTO exemplo_default (status, quantidade) 
VALUES (DEFAULT, DEFAULT);  
-- Resultado: status='pendente', quantidade=1, data_registro=CURRENT_TIMESTAMP

INSERT INTO exemplo_default () 
VALUES ();  
-- Resultado: todos os DEFAULT aplicados
```

---

### 3. Nunca Armazene Idade - Armazene Data de Nascimento

```sql
-- ❌ ERRADO: Idade muda todo ano!
CREATE TABLE pessoa_errada (
    nome VARCHAR(100),
    idade INT  -- Ano que vem estará errado!
);

-- ✅ CORRETO: Data de nascimento + cálculo dinâmico
CREATE TABLE pessoa_correta (
    nome VARCHAR(100),
    data_nascimento DATE,
    -- Idade calculada automaticamente (MySQL 5.7+)
    idade INT AS (TIMESTAMPDIFF(YEAR, data_nascimento, CURDATE())) VIRTUAL
);

INSERT INTO pessoa_correta (nome, data_nascimento) 
VALUES ('Carlos', '1990-05-20');

SELECT nome, idade FROM pessoa_correta;
-- Sempre atualizado!
```

---

## 📋 Resumo Rápido

* **DDL vs DML**: DDL define estrutura, DML manipula dados
* **INSERT INTO**: Sintaxe: `INSERT INTO tabela (campos) VALUES (valores)`
* **Formatos**: Textos e datas com aspas simples, datas como `YYYY-MM-DD`
* **Auto-increment**: Omita campo ou use `NULL`/`DEFAULT`
* **Inserção múltipla**: Use `VALUES (), (), ()` para performance
* **Boas práticas**: Armazene data_nascimento, não idade; use constraints

> 💡 Dica: "Criar tabelas é modelagem. Inserir dados é testar se a modelagem realmente funciona."

---