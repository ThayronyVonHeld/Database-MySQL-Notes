# 📚 Aula 5 - Inserção de Dados com INSERT INTO

---

## 🎯 Objetivos da Aula

* Diferenciar claramente comandos DDL e DML
* Dominar a sintaxe completa do comando INSERT INTO
* Aprender técnicas avançadas de inserção de dados
* Implementar boas práticas na manipulação de informações
* Otimizar inserções para performance e manutenibilidade
* Configurar e utilizar o ambiente MySQL Workbench + WampServer

---

## 📊 Classificação de Comandos SQL: DDL vs. DML

### Visão Geral da Arquitetura SQL

```mermaid
graph TD
    A[Comandos SQL] --> B[DDL - Data Definition Language]
    A --> C[DML - Data Manipulation Language]
    A --> D[DQL - Data Query Language]
    A --> E[DCL - Data Control Language]
    A --> F[DTL - Data Transaction Language]
    
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

## 🎯 O Comando INSERT INTO: Sintaxe Completa

### Anatomia do INSERT INTO

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

## ⚡ Otimizações e Técnicas Avançadas

### 1. Auto-incremento e o Campo ID

```sql
CREATE TABLE produto (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- AUTO_INCREMENT aqui
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10,2)
);

-- ✅ FORMAS CORRETAS de inserir com AUTO_INCREMENT:

-- Opção A: Omitir completamente o campo id
INSERT INTO produto (nome, preco) 
VALUES ('Notebook', 2999.90);

-- Opção B: Usar NULL (o MySQL entende que deve gerar)
INSERT INTO produto (id, nome, preco) 
VALUES (NULL, 'Mouse', 89.90);

-- Opção C: Usar DEFAULT (mais explícito)
INSERT INTO produto (id, nome, preco) 
VALUES (DEFAULT, 'Teclado', 149.90);

-- ❌ FORMAS ERRADAS:
INSERT INTO produto (id, nome, preco) VALUES (0, 'Monitor', 999.90);  -- Pode gerar conflito
INSERT INTO produto VALUES (999, 'Tablet', 1999.90);  -- Forçando valor, pode quebrar sequência

-- 📊 Verificar o último ID gerado
SELECT LAST_INSERT_ID();  -- Retorna o último AUTO_INCREMENT gerado
```

### 2. Omissão de Campos (Forma Simplificada)

```sql
-- TABELA COM ORDEM ESPECÍFICA:
CREATE TABLE cidade (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100),
    uf CHAR(2),
    populacao INT
);

-- ✅ FORMA SIMPLIFICADA (quando conhece a ordem exata)
INSERT INTO cidade VALUES (NULL, 'São Paulo', 'SP', 12300000);
-- Equivalente a: INSERT INTO cidade (id, nome, uf, populacao) VALUES ...

-- ⚠️ PERIGOS da forma simplificada:
-- 1. Se a ordem da tabela mudar, suas inserções quebram
-- 2. Se esquecer um valor, todos os campos ficam desalinhados
-- 3. Menos legível para outros desenvolvedores

-- ✅ FORMA EXPLÍCITA (RECOMENDADA - mais segura)
INSERT INTO cidade (nome, uf, populacao) 
VALUES ('Rio de Janeiro', 'RJ', 6748000);
```

### 3. Inserção Múltipla (Bulk Insert)

```sql
-- ❌ FORMA INEFICIENTE (múltiplos comandos)
INSERT INTO produto (nome, preco) VALUES ('Produto 1', 10.00);
INSERT INTO produto (nome, preco) VALUES ('Produto 2', 20.00);
INSERT INTO produto (nome, preco) VALUES ('Produto 3', 30.00);

-- ✅ FORMA OTIMIZADA (single query)
INSERT INTO produto (nome, preco) 
VALUES 
    ('Produto 1', 10.00),
    ('Produto 2', 20.00),
    ('Produto 3', 30.00),
    ('Produto 4', 40.00),
    ('Produto 5', 50.00);

-- 📈 BENEFÍCIOS da inserção múltipla:
-- 1. Performance muito superior
-- 2. Uma única transação (mais seguro)
-- 3. Mais fácil de ler e manter
-- 4. Menor sobrecarga no servidor

-- 🚀 EXEMPLO REAL: Cadastro de cidades
INSERT INTO cidade (nome, uf, populacao) 
VALUES 
    ('Belo Horizonte', 'MG', 2512000),
    ('Salvador', 'BA', 2887000),
    ('Fortaleza', 'CE', 2669000),
    ('Brasília', 'DF', 3055000),
    ('Curitiba', 'PR', 1948000);
```

### 4. Inserção com SELECT (INSERT...SELECT)

```sql
-- Criar tabela temporária ou de backup
CREATE TABLE produto_backup LIKE produto;

-- Copiar todos os dados de uma tabela para outra
INSERT INTO produto_backup 
SELECT * FROM produto;

-- Copiar apenas alguns dados com filtro
INSERT INTO produto_backup (nome, preco)
SELECT nome, preco FROM produto 
WHERE preco > 100.00;

-- Criar resumo/agregado em outra tabela
CREATE TABLE resumo_categorias (
    categoria VARCHAR(50),
    total_produtos INT,
    preco_medio DECIMAL(10,2)
);

INSERT INTO resumo_categorias 
SELECT 
    categoria,
    COUNT(*) as total_produtos,
    AVG(preco) as preco_medio
FROM produto 
GROUP BY categoria;
```

---

## 🛡️ Boas Práticas na Inserção de Dados

### 1. Nunca Armazene Idade - Armazene Data de Nascimento

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

### 2. Utilize Constraints para Qualidade dos Dados

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

### 3. Tratamento de Valores Nulos vs. DEFAULT

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

## 🛠️ Ambiente de Trabalho: MySQL Workbench + WampServer

### Configuração do Ambiente

```text
🔧 WAMPSERVER (Windows)
├── Apache (Servidor Web)
├── MySQL (Servidor Banco de Dados) ← Usaremos este!
├── PHP (Linguagem de Programação)
└── Painel de Controle

🖥️ MYSQL WORKBENCH (Interface Gráfica)
├── Editor SQL
├── Design Visual de Tabelas
├── Administração
└── Modelagem de Dados
```

### Conexão Correta no Workbench

```sql
-- Configuração típica:
Hostname: localhost  ou  127.0.0.1
Port: 3306
Username: root
Password: [deixe em branco ou 'root' no Wamp]

-- Teste de conexão
SELECT @@version;  -- Mostra versão MySQL
SHOW DATABASES;    -- Lista bancos disponíveis
```

### Scripts de Prática Recomendados

```sql
-- 1. Criar banco de prática
CREATE DATABASE IF NOT EXISTS pratica_insert;
USE pratica_insert;

-- 2. Criar tabela de exemplo
CREATE TABLE aluno_pratica (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    matricula VARCHAR(20) UNIQUE,
    data_nascimento DATE NOT NULL,
    email VARCHAR(100),
    ativo BOOLEAN DEFAULT TRUE,
    data_cadastro DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 3. Exercícios de inserção
-- Exercício 1: Inserção simples
INSERT INTO aluno_pratica (nome, matricula, data_nascimento)
VALUES ('João Silva', '2024001', '2005-03-15');

-- Exercício 2: Inserção múltipla
INSERT INTO aluno_pratica (nome, matricula, data_nascimento, email) 
VALUES 
    ('Maria Santos', '2024002', '2004-07-22', 'maria@email.com'),
    ('Pedro Oliveira', '2024003', '2006-01-30', 'pedro@email.com'),
    ('Ana Costa', '2024004', '2005-11-08', 'ana@email.com');

-- Exercício 3: Testar constraints
-- Tente inserir matrícula duplicada (deve falhar)
INSERT INTO aluno_pratica (nome, matricula, data_nascimento)
VALUES ('Carlos Duplicado', '2024001', '2005-05-10');

-- Exercício 4: Inserção com DEFAULT
INSERT INTO aluno_pratica (nome, matricula, data_nascimento)
VALUES ('Teste Default', '2024005', '2005-09-12');
-- Verifique os valores DEFAULT aplicados
```

---

## 🚀 Exemplo Prático Completo

### Sistema de Biblioteca - Inserção de Dados

```sql
-- 1. Criar estrutura (DDL)
CREATE DATABASE biblioteca_completa;
USE biblioteca_completa;

CREATE TABLE autor (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    nacionalidade VARCHAR(50) DEFAULT 'Desconhecida',
    data_nascimento DATE,
    data_falecimento DATE NULL,
    CHECK (data_falecimento IS NULL OR data_falecimento > data_nascimento)
);

CREATE TABLE livro (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(200) NOT NULL,
    isbn VARCHAR(13) UNIQUE NOT NULL,
    ano_publicacao YEAR,
    paginas SMALLINT UNSIGNED,
    preco DECIMAL(6,2) CHECK (preco > 0),
    estoque INT DEFAULT 0,
    autor_id INT,
    FOREIGN KEY (autor_id) REFERENCES autor(id)
);

-- 2. Inserir autores (DML)
INSERT INTO autor (nome, nacionalidade, data_nascimento, data_falecimento) 
VALUES 
    ('Machado de Assis', 'Brasileira', '1839-06-21', '1908-09-29'),
    ('Clarice Lispector', 'Brasileira', '1920-12-10', '1977-12-09'),
    ('George Orwell', 'Britânica', '1903-06-25', '1950-01-21'),
    ('J.K. Rowling', 'Britânica', '1965-07-31', NULL),  -- Viva
    ('Stephen King', 'Americana', '1947-09-21', NULL);   -- Vivo

-- 3. Inserir livros (DML) - Forma múltipla otimizada
INSERT INTO livro (titulo, isbn, ano_publicacao, paginas, preco, estoque, autor_id) 
VALUES 
    ('Dom Casmurro', '9788535902775', 1899, 256, 29.90, 15, 1),
    ('Memórias Póstumas de Brás Cubas', '9788535911241', 1881, 368, 34.90, 8, 1),
    ('A Hora da Estrela', '9788535909552', 1977, 96, 24.90, 12, 2),
    ('1984', '9788535914846', 1949, 328, 39.90, 20, 3),
    ('A Revolução dos Bichos', '9788535915263', 1945, 152, 29.90, 18, 3),
    ('Harry Potter e a Pedra Filosofal', '9788532511010', 1997, 264, 49.90, 25, 4),
    ('It: A Coisa', '9788532526281', 1986, 1104, 79.90, 10, 5);

-- 4. Inserção especial: livros sem autor (para demonstração)
INSERT INTO livro (titulo, isbn, ano_publicacao, paginas, preco, estoque) 
VALUES 
    ('Desconhecido', '9780000000001', 2000, 100, 19.90, 5);

-- 5. Consulta para verificar inserções
SELECT 
    l.titulo, 
    a.nome as autor, 
    l.ano_publicacao, 
    l.preco,
    l.estoque
FROM livro l
LEFT JOIN autor a ON l.autor_id = a.id
ORDER BY l.titulo;
```

---

## 📋 Resumo Rápido

* **DDL vs DML**: DDL define estrutura, DML manipula dados
* **INSERT INTO**: Sintaxe: `INSERT INTO tabela (campos) VALUES (valores)`
* **Formatos**: Textos e datas com aspas simples, datas como `YYYY-MM-DD`
* **Auto-increment**: Omita campo ou use `NULL`/`DEFAULT`
* **Inserção múltipla**: Use `VALUES (), (), ()` para performance
* **Boas práticas**: Armazene data_nascimento, não idade; use constraints
* **Ambiente**: WampServer (MySQL) + MySQL Workbench (interface)
* **Performance**: Bulk inserts são muito mais rápidos

---

## 💡 Dica do Especialista

"Pense no INSERT como alimentar um formulário: cada campo precisa do tipo correto de dado. Use a forma explícita `(campos) VALUES (valores)` - é mais verboso, mas evita erros catastróficos quando a estrutura mudar."

> 🧠 **Exercício Obrigatório**:
> 1. Crie banco `empresa_dml`
> 2. Crie tabela `funcionario`: id(PK AI), nome(NN), cargo, salario, data_admissao(NN), departamento
> 3. Insira 10 funcionários de uma vez (múltipla)
> 4. Crie tabela `departamento`: id(PK AI), nome(NN), orcamento
> 5. Insira 5 departamentos
> 6. Atualize funcionários para usar IDs de departamento
     > **Dica**: Use INSERT...SELECT para eficiência!

---
