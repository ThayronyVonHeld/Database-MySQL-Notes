# 📚 Aula 6 — Alterando Estruturas de Tabelas no MySQL

---

## 🎯 Objetivos da Aula

* Entender como modificar tabelas já existentes no MySQL
* Aprender a utilizar o comando **ALTER TABLE**
* Adicionar, remover e modificar colunas
* Renomear tabelas e campos
* Compreender o uso de **UNIQUE**, **UNSIGNED** e **PRIMARY KEY** pós-criação
* Aprender a utilizar comandos destrutivos com segurança
* Consolidar o uso de **DESCRIBE** para verificação estrutural

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

---

### UNSIGNED

Impede números negativos.

```sql
ALTER TABLE cursos
MODIFY COLUMN carga_horaria INT UNSIGNED;
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
* **ADD** adiciona colunas
* **DROP COLUMN** remove colunas
* **MODIFY** altera tipo e restrições
* **CHANGE** altera nome e estrutura
* **RENAME TO** muda o nome da tabela
* **DROP TABLE** remove toda a tabela
* **UNIQUE** evita duplicação
* **UNSIGNED** impede números negativos
* **DESCRIBE** verifica alterações

---

> 💡 **Dica:**
> "Criar tabelas é o começo. Manter e evoluir a estrutura do banco é o trabalho real de quem desenvolve sistemas."

---