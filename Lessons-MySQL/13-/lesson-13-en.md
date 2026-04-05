# 📚 Aula 13 — Relacionamentos na Prática (FK, Integridade e JOINs)

---

## 🎯 Objetivos da Aula

* Compreender o papel das **Engines** no MySQL
* Entender o conceito de **ACID**
* Implementar **Chaves Estrangeiras (FK)** na prática
* Garantir **Integridade Referencial**
* Utilizar **JOINs** para unir tabelas
* Diferenciar **INNER JOIN**, **LEFT JOIN** e **RIGHT JOIN**
* Utilizar **aliases (apelidos)** para melhorar consultas

---

# ⚙️ Engines (Motores de Armazenamento)

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

# 🧠 O Conceito de ACID

Para que um banco seja confiável, ele precisa seguir 4 regras:

---

## 🔹 Atomicidade

```text
Tudo ou nada
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

# 🔗 Implementando Chave Estrangeira (FK)

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

# 🛡️ Integridade Referencial

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

## 🎯 Problema

Temos:

```text
Tabela alunos → curso_id
Tabela cursos → nome do curso
```

👉 Como mostrar o nome do curso junto com o aluno?

---

## ✅ Solução: JOIN

```sql
SELECT alunos.nome, cursos.nome
FROM alunos
JOIN cursos
ON alunos.curso_id = cursos.id;
```

---

# ⚠️ Cláusula ON (Obrigatória)

```text
Define COMO as tabelas se conectam
```

Sem isso:

👉 vira um **produto cartesiano** (erro grave)

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

# 🏷️ Apelidos (Aliases)

Para simplificar consultas:

```sql
SELECT a.nome, c.nome
FROM alunos AS a
JOIN cursos AS c
ON a.curso_id = c.id;
```

Ou:

```sql
FROM alunos a
JOIN cursos c
```

---

💡 Benefícios:

```text
- Código menor
- Mais legível
- Menos erro
```

---

# 📈 Ordenando Resultados

```sql
SELECT a.nome, c.nome
FROM alunos a
JOIN cursos c
ON a.curso_id = c.id
ORDER BY a.nome;
```

---

# 🧪 Verificando Estrutura

```sql
DESC alunos;
```

No MySQL Workbench, você verá:

```text
MUL → indica chave estrangeira
```

---

# 📊 Resumo Rápido

* **InnoDB** → engine padrão com suporte a FK e ACID
* **ACID** → garante segurança das transações
* **FK** → conecta tabelas
* **Integridade referencial** evita dados inválidos
* **JOIN** une tabelas
* **INNER JOIN** → apenas correspondências
* **LEFT JOIN** → tudo da esquerda
* **RIGHT JOIN** → tudo da direita
* **ON** define a relação
* **Aliases** simplificam queries

---

💡 **Dica de desenvolvedor**

Se na aula anterior você aprendeu a **modelar**…

👉 Agora você aprendeu a **fazer o banco funcionar de verdade**

Porque no mundo real:

```text
Sem JOIN → seu sistema não conversa
Sem FK → seu sistema quebra
```

Dominar isso aqui é o que te permite construir:

```text
- APIs reais
- Sistemas completos
- Consultas profissionais
```

E aqui começa o nível **profissional de banco de dados** 🚀
