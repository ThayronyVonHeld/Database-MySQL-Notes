# 📚 Aula 12 — Modelo Relacional (Fundamentos)

---

## 🎯 Objetivos da Aula

* Compreender o que é o **Modelo Relacional**
* Identificar **Entidades** e **Atributos**
* Entender o papel da **Chave Primária (PK)**
* Compreender o **Diagrama Entidade-Relacionamento (DER)**
* Aprender os tipos de **cardinalidade**
* Entender o uso da **Chave Estrangeira (FK)**
* Aplicar as regras de relacionamento entre tabelas

---

# 🧠 O que é o Modelo Relacional?

O **Modelo Relacional** foi criado por **Edgar Codd** na década de 1970.

Ele trouxe uma ideia revolucionária:

```text
Os dados não existem isolados — eles possuem RELAÇÕES entre si
```

---

💡 Exemplo simples:

```text
Um aluno → está matriculado em um curso
Um curso → possui vários alunos
```

👉 Isso é uma **relação**

---



# 🔢 Cardinalidade (Tipos de Relação)

Define **quantos registros se relacionam com outros**.

---

## 1️⃣ Um para Um (1:1)

```text
Uma entidade se relaciona com apenas uma da outra
```

Exemplo:

```text
Pessoa ↔ CPF
```

---

## 2️⃣ Um para Muitos (1:N)

```text
Um lado → vários registros
Outro lado → apenas um
```

Exemplo:

```text
Curso → vários alunos
Aluno → apenas um curso
```

---

## 3️⃣ Muitos para Muitos (N:N)

```text
Ambos os lados possuem múltiplos relacionamentos
```

Exemplo:

```text
Aluno → vários cursos
Curso → vários alunos
```

---

# 🔗 Chave Estrangeira (Foreign Key — FK)

A **FK** é o que conecta tabelas.

Ela é:

```text
Uma PK de outra tabela usada como referência
```

---

## Exemplo

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
    curso_id INT,
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);
```

---

💡 Aqui:

```text
curso_id → FK
cursos.id → PK original
```

---

# ⚙️ Regras de Implementação

Agora vem o mais importante na prática 👇

---

## 🔹 1:1 (Um para Um)

Escolhe-se um lado para receber a FK.

```text
Tabela A ← recebe FK da Tabela B
```

---

## 🔹 1:N (Um para Muitos)

Regra fixa:

```text
A chave do lado 1 vai para o lado N
```

Exemplo:

```text
Curso (1) → Aluno (N)

FK fica em: Aluno
```

---

## 🔹 N:N (Muitos para Muitos)

Aqui muda tudo:

👉 Criamos uma **tabela intermediária**

---

### Exemplo

```text
Aluno ↔ Curso
```

Tabela intermediária:

```sql
CREATE TABLE aluno_curso (
    aluno_id INT,
    curso_id INT,
    FOREIGN KEY (aluno_id) REFERENCES alunos(id),
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);
```

---

💡 Essa tabela representa o **relacionamento**

---

# 🧠 Resumo Visual

```text
1:1 → FK em um dos lados
1:N → FK vai para o lado N
N:N → cria tabela intermediária
```

---

# 📊 Resumo Rápido

* **Modelo Relacional** organiza dados com relações
* **Entidade** → tabela
* **Atributo** → coluna
* **Registro** → linha
* **PK** → identifica registros únicos
* **FK** → conecta tabelas
* **DER** → representação visual
* **1:1** → relação única
* **1:N** → um para vários
* **N:N** → muitos para muitos (tabela intermediária)

---

💡 **Dica de desenvolvedor**

Se você entendeu isso aqui…

👉 Você saiu de **“escrever SQL”**
👉 E entrou em **“modelar sistemas de verdade”**

Porque no mundo real:

```text
O problema não é escrever query…
É estruturar o banco corretamente
```

E isso é o que separa um dev comum de um dev mais avançado 🚀
