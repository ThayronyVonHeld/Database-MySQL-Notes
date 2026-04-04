# 📚 Aula 12 — Modelo Relacional (Fundamentos)

---

* Compreender o que é o **Modelo Relacional**
* Identificar **Entidades** e **Atributos**
* Entender a importância da Chave Primária (PK)
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

# 🧱 Entidades e Atributos

## 📦 Entidade

Uma entidade é como um:

```text
"container de informações"
```

Exemplos:

```text
- Aluno
- Curso
- Produto
```

No banco de dados, isso vira uma **tabela**.

---

## 🏷️ Atributos

São as **características da entidade**.

Exemplo:

```text
Aluno:
- nome
- idade
- altura
```

No banco, isso vira **colunas**.

---

## 📌 Registro (Tupla)

Cada linha da tabela representa um:

```text
Registro (ou Tupla)
```

Exemplo:

```text
João | 21 | 1.80
```

---

# 🔑 Chave Primária (Primary Key — PK)

A **PK** é o identificador único de cada registro.

```text
Não pode repetir
Não pode ser nula
```

---

## Exemplo

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100)
);
```

---

💡 Analogia:

```text
CPF → identifica uma pessoa
ID → identifica um registro
```

---

# 🔗 Relacionamentos

Relacionamento é a **ligação entre entidades**.

Exemplo:

```text
Aluno → assiste → Curso
```

---

# 📊 DER (Diagrama Entidade-Relacionamento)

É a forma **visual** de representar o banco.

Elementos:

```text
Retângulo → Entidade
Losango   → Relacionamento
Oval      → Atributo (conceitual)
```

---

💡 Exemplo:

```text
Aluno ─── assiste ─── Curso
```

---

### Representação visual: 
![](/Lessons-MySQL/img/IMG%20DER%20GNB.png)

---
