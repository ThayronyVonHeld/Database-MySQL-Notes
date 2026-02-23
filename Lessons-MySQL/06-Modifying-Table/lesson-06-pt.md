# 📚 Aula 6 — Alterando Estruturas de Tabelas no MySQL

---

## 🎯 Objetivos da Aula

* Aprender o comando ALTER TABLE para modificação de tabelas
* Compreender as diferenças entre ADD, MODIFY, CHANGE e DROP
* Implementar constraints pós-criação com segurança
* Utilizar comandos DDL com boas práticas e prevenção de erros
* Gerenciar integridade durante alterações estruturais

---

## 🧱 Alterando Estruturas com ALTER TABLE

Até agora, aprendemos a **criar tabelas**.
Mas sistemas reais evoluem — e o banco precisa evoluir junto.

É exatamente para isso que usamos o comando:

```sql
ALTER TABLE
```

Esse comando pertence à categoria **DDL (Data Definition Language)**, pois altera a **estrutura do banco de dados**.

---

## ➕ Adicionando Colunas

Por padrão, uma nova coluna é adicionada ao final da tabela.

```sql
ALTER TABLE aluno
ADD COLUMN email VARCHAR(100);
```

---

### Controlando a posição da coluna

```sql
-- Primeira coluna da tabela
ALTER TABLE aluno
ADD COLUMN matricula INT FIRST;

-- Após uma coluna específica
ALTER TABLE aluno
ADD COLUMN telefone VARCHAR(20) AFTER nome;
```

---

## ➖ Removendo Colunas

Remove completamente a coluna e seus dados:

```sql
ALTER TABLE aluno
DROP COLUMN telefone;
```

⚠️ Essa operação é permanente.

---

## 🔧 Modificando Colunas (MODIFY)

Altera o **tipo de dado** ou **restrições**, mantendo o nome da coluna.

```sql
ALTER TABLE aluno
MODIFY COLUMN nome VARCHAR(150) NOT NULL;
```

### 🚨 LIMITAÇÕES DO MODIFY:
- 1. Não pode mudar o nome da coluna
- 2. Algumas mudanças requerem recriação da tabela (lento em grandes tabelas)

---

## 🔄 Alterando Nome e Estrutura (CHANGE)

O comando `CHANGE` permite modificar:

* Nome da coluna
* Tipo de dado
* Constraints

```sql
ALTER TABLE aluno
CHANGE COLUMN nome nome_completo VARCHAR(150) NOT NULL;
```

Observe que é necessário informar:

```text
nome_antigo → nome_novo → tipo → restrições
```

---

## 🔁 Renomeando Tabelas

```sql
ALTER TABLE aluno
RENAME TO estudantes;
```

---

## 🗑️ Removendo Tabelas (DROP TABLE)

Apaga completamente a tabela:

```sql
DROP TABLE estudantes;
```

Isso remove:

```text
- Estrutura
- Registros
- Índices
- Chaves
```

⚠️ Não existe "desfazer".

---

## 🛡️ Parâmetros de Segurança

Para evitar erros:

```sql
CREATE TABLE IF NOT EXISTS cursos (
    id INT
);

DROP TABLE IF EXISTS cursos;
```
---

## 🔒 Constraints e Parâmetros Avançados

### IF NOT EXISTS / IF EXISTS - Segurança em DDL

✅ Evitar erros na criação

```
CREATE TABLE IF NOT EXISTS produto (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100)
);
```

#### Mesmo se executar múltiplas vezes, não dá erro
```
CREATE TABLE IF NOT EXISTS produto (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100));
-- Mesmo se executar múltiplas vezes, não dá erro
```

✅ Evitar erros na remoção

```
DROP TABLE IF EXISTS produto_inexistente;  -- Apenas warning, não erro
```

✅ EM CONJUNTO: Recriação segura de tabelas
```
-- ✅ EM CONJUNTO: Recriação segura de tabelas
DROP TABLE IF EXISTS temp_data;
CREATE TABLE temp_data (
    id INT PRIMARY KEY);
```

---

### UNIQUE

Impede valores duplicados.

```sql
ALTER TABLE cursos
ADD UNIQUE (nome);
```

Diferença importante:

```text
PRIMARY KEY → identifica registros
UNIQUE → evita duplicidade
```

Exemplo:
```sql
CREATE TABLE curso (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- PK: única, não nula, identificadora
    codigo VARCHAR(10) UNIQUE,          -- UNIQUE: única, pode ser nula
    nome VARCHAR(100) NOT NULL UNIQUE   -- Pode ter múltiplos UNIQUE
);
```
---

### UNSIGNED

Impede números negativos e economiza espaço.

```sql
ALTER TABLE cursos
MODIFY COLUMN carga_horaria INT UNSIGNED;
```

exemplo:
```sql
CREATE TABLE metricas (
-- Com UNSIGNED: 0 a 255 (1 byte)
visitas TINYINT UNSIGNED,

    -- Sem UNSIGNED: -128 a 127 (1 byte)
    temperatura TINYINT,
    
    -- Grande economia em milhões de registros
    populacao INT UNSIGNED,  -- 0 a ~4 bilhões
    altura SMALLINT UNSIGNED -- 0 a 65535 cm (655 metros)
);
```

Ideal para:

```text
- Idades
- Quantidades
- Contadores
- Estoque
```

---

### Adicionando PRIMARY KEY depois da criação

```sql
ALTER TABLE cursos
ADD PRIMARY KEY (id);
```

Exemplo:
```sql
-- ❌ TABELA SEM PRIMARY KEY (problema futuro garantido)
CREATE TABLE cliente_sem_pk (
    nome VARCHAR(100),
    cpf VARCHAR(11)
);

-- ✅ ADICIONAR PRIMARY KEY DEPOIS
ALTER TABLE cliente_sem_pk 
ADD COLUMN id INT FIRST;

UPDATE cliente_sem_pk SET id = @row_number := @row_number + 1;

ALTER TABLE cliente_sem_pk 
ADD PRIMARY KEY (id);

-- OU adicionar PK em coluna existente (se for única)
ALTER TABLE cliente_sem_pk 
ADD PRIMARY KEY (cpf);
```

---

## ⚠️ Conflitos com NOT NULL

Se uma tabela já possui registros, adicionar uma coluna obrigatória pode gerar erro.

Problema:

```sql
ALTER TABLE aluno
ADD COLUMN pais VARCHAR(50) NOT NULL;
```

Solução:

```sql
ALTER TABLE aluno
ADD COLUMN pais VARCHAR(50) NOT NULL DEFAULT 'Brasil';
```

O **DEFAULT** preenche os registros antigos automaticamente.

---

## 🔎 Verificando Alterações

Sempre confirme a estrutura após alterações:

```sql
DESCRIBE aluno;
DESC aluno;
```

Isso evita erros silenciosos.

---

## 📊 Resumo Rápido

* **ALTER TABLE** modifica estruturas existentes
* **ADD COLUMN** adiciona colunas
* **DROP COLUMN** remove colunas
* **MODIFY** altera tipo e restrições
* **CHANGE** altera nome e estrutura
* **RENAME TO** muda o nome da tabela
* **DROP TABLE** remove toda a tabela
* **UNIQUE** evita duplicação
* **UNSIGNED** impede números negativos
* **DESCRIBE** verifica alterações

> 💡Dica: Criar tabelas é o começo. Manter e evoluir a estrutura do banco é o trabalho real de quem desenvolve sistemas."
