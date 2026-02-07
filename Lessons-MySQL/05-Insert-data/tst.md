# 📚 Aula 5 — Inserindo Dados com INSERT INTO (MySQL)

---

## 🎯 Objetivos da Aula

* Entender a diferença entre comandos **DDL** e **DML**
* Aprender a inserir dados em tabelas com **INSERT INTO**
* Trabalhar corretamente com **datas, textos e números**
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

