# 📚 Aula 13 — Relacionamentos na Prática (FK, Integridade e JOINs)

---

* Compreender o papel das **Engines** no MySQL
* Entender o conceito ACID e sua importância para transações
* Implementar Chaves Estrangeiras (FK) na prática
* Garantir a Integridade Referencial entre tabelas
* Utilizar as junções **JOINs** para unir tabelas
* Diferenciar **INNER JOIN**, **LEFT JOIN** e **RIGHT JOIN**
* Utilizar **aliases (apelidos)** para melhorar consultas

---

## ⚙️ Engines (Motores de Armazenamento)

Quando criamos uma tabela no MySQL, precisamos definir **como ela será armazenada**.

---

## Principais Engines

### 🔹 InnoDB (Padrão)

```text
✔ Suporta chaves estrangeiras (FK)
✔ Suporta transações (ACID)
✔ Garante integridade referencial
```

👉 É a mais usada hoje.

---

### 🔹 MyISAM

```text
❌ Não suporta FK
❌ Não segue ACID
✔ Mais simples (legado)
```

---

### 🔹 ExtraDB

```text
✔ Alternativa moderna
✔ Suporte a transações
```

---

💡 Na prática:

```text
Use InnoDB quase sempre
```

---

### Como definir a Engine

-  Explícito: Define a engine na criação

```sql
CREATE TABLE cliente (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
) ENGINE = InnoDB;
```

- Verificar a engine de uma tabela

```sql
SELECT ENGINE 
FROM information_schema.TABLES 
WHERE TABLE_NAME = 'cliente';
```
- Alterar a engine depois de criada

```sql
ALTER TABLE cliente ENGINE = InnoDB;
```

- Ver todas as engines disponíveis
```
SHOW ENGINES;
```

---

## 🧠 O Conceito de ACID

Para que um banco seja confiável, ele precisa seguir 4 regras:

---

## 🔹 Atomicidade

```text
Tudo ou nada
Transação completa ou reversão
```

Se algo falhar:

👉 o banco desfaz tudo (rollback)

---

## 🔹 Consistência

```text
O banco nunca fica "quebrado"
```

Sempre mantém regras válidas.

---

## 🔹 Isolamento

```text
Transações não se misturam
```

Uma não interfere na outra.

---

## 🔹 Durabilidade

```text
Salvou → está salvo
```

Mesmo com queda de energia, os dados permanecem.

---

## 🔗 Implementando Chave Estrangeira (FK)

Agora vamos para a prática.

---

## 📌 Regra principal (1:N)

```text
PK do lado 1 → vai para o lado N como FK
```

---

## 🧩 Exemplo

Tabela cursos:

```sql
CREATE TABLE cursos (
    id INT PRIMARY KEY,
    nome VARCHAR(100)
);
```

Tabela alunos:

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    curso_id INT
);
```

---

## 🔧 Adicionando a FK

```sql
ALTER TABLE alunos
ADD FOREIGN KEY (curso_id)
REFERENCES cursos(id);
```

---

## ⚠️ Regra importante

```text
Tipo e tamanho DEVEM ser iguais
```

---

## 🛡️ Integridade Referencial

A FK garante que o banco **não permita inconsistências**.

---

## ❌ Exemplo de erro

```text
Aluno está ligado ao curso "MySQL"
```

Se tentar apagar o curso:

```sql
DELETE FROM cursos WHERE id = 1;
```

👉 O banco NÃO deixa.

---

💡 Isso evita:

```text
- Dados órfãos
- Relacionamentos quebrados
- Bugs em sistemas
```

---

# 🔄 JOIN (Junção de Tabelas)

JOIN permite **unir dados de várias tabelas**.

---

## ❌ Problema

Temos: Dados separados em duas tabelas

```text
SELECT * FROM aluno;
-- +----+--------------+-------------------+----------+
-- | id | nome         | email             | curso_id |
-- +----+--------------+-------------------+----------+
-- | 1  | João Silva   | joao@email.com    | 1        |
-- | 2  | Maria Santos | maria@email.com   | 1        |
-- | 3  | Pedro        | pedro@email.com   | 2        |
-- +----+--------------+-------------------+----------+

SELECT * FROM curso;
-- +----+----------------+---------------+
-- | id | nome           | carga_horaria |
-- +----+----------------+---------------+
-- | 1  | MySQL Completo | 50            |
-- | 2  | Java Fundamentos| 60           |
-- +----+----------------+---------------+
```

👉 Como mostrar o nome do curso junto com o aluno?

---

## ✅ Solução: JOIN

- INNER JOIN: Mostra apenas alunos com curso válido

```sql
SELECT 
    aluno.nome AS aluno_nome,
    curso.nome AS curso_nome,
    curso.carga_horaria
FROM aluno
INNER JOIN curso ON aluno.curso_id = curso.id
ORDER BY curso.nome, aluno.nome;

```
- Resultado:

```sql
-- +--------------+------------------+---------------+
-- | aluno_nome   | curso_nome       | carga_horaria |
-- +--------------+------------------+---------------+
-- | João Silva   | MySQL Completo   | 50            |
-- | Maria Santos | MySQL Completo   | 50            |
-- | Pedro        | Java Fundamentos | 60            |
-- +--------------+------------------+---------------+
```
- Alunos sem curso NÃO aparecem
---

# ⚠️ Cláusula ON (Obrigatória)

```text
Define COMO as tabelas se conectam
```

Sem isso:

👉 vira um **produto cartesiano** (erro grave)

---

❌ Exemplo:  SEM ON

```
SELECT * FROM aluno, curso;
```
- Resultado: 3 alunos × 2 cursos = 6 linhas (combinações erradas!)
```sql
-- +----+---------+----------+----+------------------+
-- | id | nome    | curso_id | id | nome             |
-- +----+---------+----------+----+------------------+
-- | 1  | João    | 1        | 1  | MySQL Completo   | ← Correto
-- | 1  | João    | 1        | 2  | Java Fundamentos | ← Errado!
-- | 2  | Maria   | 1        | 1  | MySQL Completo   | ← Correto
-- | 2  | Maria   | 1        | 2  | Java Fundamentos | ← Errado!
```

---

# 🔗 Tipos de JOIN

---

## 🔹 INNER JOIN

Mostra apenas os dados que possuem correspondência.

```sql
SELECT a.nome, c.nome
FROM alunos a
INNER JOIN cursos c
ON a.curso_id = c.id;
```

---

💡 Retorna:

```text
Somente alunos que possuem curso válido
```

---

## 🔹 LEFT JOIN

Prioriza a tabela da esquerda.

```sql
SELECT a.nome, c.nome
FROM alunos a
LEFT JOIN cursos c
ON a.curso_id = c.id;
```

---

💡 Retorna:

```text
Todos os alunos
Mesmo os que NÃO têm curso
```

---

## 🔹 RIGHT JOIN

Prioriza a tabela da direita.

```sql
SELECT a.nome, c.nome
FROM alunos a
RIGHT JOIN cursos c
ON a.curso_id = c.id;
```

---

💡 Retorna:

```text
Todos os cursos
Mesmo sem alunos
```

---

# 🧠 Visualização Mental

```text
INNER JOIN → interseção
LEFT JOIN  → tudo da esquerda
RIGHT JOIN → tudo da direita
```

---

## 📝 Aliases (Apelidos) com AS

### Por que usar Aliases?

Para simplificar consultas:

- ❌ SEM ALIAS (código longo e repetitivo)

```sql
SELECT 
    aluno.nome, 
    curso.nome, 
    curso.carga_horaria,
    aluno.email
FROM aluno
INNER JOIN curso ON aluno.curso_id = curso.id
ORDER BY curso.nome, aluno.nome;
```
- ✅ COM ALIAS (mais limpo e profissional)

```sql
  SELECT
  a.nome AS aluno_nome,
  c.nome AS curso_nome,
  c.carga_horaria,
  a.email
  FROM aluno AS a
  INNER JOIN curso AS c 
  ON a.curso_id = c.id
  ORDER BY c.nome, a.nome;
```
---

### Sintaxe do AS

- AS é opcional (espaço também funciona)

```sql
SELECT a.nome aluno_nome, c.nome curso_nome
FROM aluno a
INNER JOIN curso c 
ON a.curso_id = c.id;
```

- Para colunas com espaço, use aspas
```sql
SELECT a.nome AS "Nome do Aluno"
FROM aluno a;
```
---

## 🧪 Verificando Estrutura

```sql
DESC alunos;
```

No MySQL Workbench, você verá:

```text
MUL → indica chave estrangeira
```
---

## 📋 Resumo Rápido

| Conceito                    | Descrição                                           |
|-----------------------------|-----------------------------------------------------|
| **InnoDB**                  | Engine padrão que suporta FK e ACID                 |
| **ACID**                    | Atomicidade, Consistência, Isolamento, Durabilidade |
| **FK (Chave Estrangeira)**  | Liga tabelas, aponta para PK de outra tabela        |
| **Integridade Referencial** | Impede exclusão de registros com dependentes        |
| **INNER JOIN**              | Mostra apenas registros com correspondência         |
| **LEFT JOIN**               | Mostra TODOS da esquerda + correspondentes          |
| **RIGHT JOIN**              | Mostra TODOS da direita + correspondentes           |
| **Alias (AS)**              | Apelidos para tabelas/colunas                       |
| **ON**                      | define a relação                                    |

---

>💡**Dica**: ON no JOIN não é opcional, sem ele, você terá um produto cartesiano catastrófico!"
