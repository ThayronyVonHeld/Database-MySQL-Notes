Perfeito — vamos manter a consistência do seu material didático.

Aqui está a **Aula 7 estruturada no mesmo padrão das anteriores**.

---

# 📚 Aula 7 — Atualizando Registros no MySQL (UPDATE)

---

## 🎯 Objetivos da Aula

* Compreender a manipulação de registros em tabelas
* Aprender a utilizar o comando **UPDATE**
* Entender a importância da cláusula **WHERE**
* Atualizar múltiplos campos simultaneamente
* Utilizar a **chave primária** como referência segura
* Conhecer mecanismos de proteção como **LIMIT** e **Safe Updates**
* Entender os riscos envolvidos na manipulação de dados

---

## 🧾 Linhas, Registros e Tuplas

No contexto de bancos de dados relacionais, alguns termos são equivalentes:

```text
Linha = Registro = Tupla
Coluna = Campo = Atributo
```

Enquanto o comando:

```sql
ALTER TABLE
```

modifica a **estrutura das colunas**, o comando:

```sql
UPDATE
```

modifica o **conteúdo dos registros**.

---

## ✏️ O Comando UPDATE

O comando `UPDATE` permite alterar dados já existentes em uma tabela.

Estrutura básica:

```sql
UPDATE tabela
SET campo = valor
WHERE condicao;
```

---

### Exemplo simples

```sql
UPDATE cursos
SET nome = 'Java Básico'
WHERE idcurso = 1;
```

Aqui estamos:

```text
Tabela → cursos
Campo → nome
Novo valor → 'Java Básico'
Registro → idcurso = 1
```

---

## 🔄 Atualizando múltiplos campos

É possível modificar vários campos ao mesmo tempo:

```sql
UPDATE cursos
SET nome = 'Java Avançado',
    carga = 40,
    ano = 2025
WHERE idcurso = 1;
```

---

## 🔑 A Importância da Cláusula WHERE

A cláusula **WHERE** é a parte mais importante do comando `UPDATE`.

Sem ela:

```sql
UPDATE cursos
SET ano = 2025;
```

Resultado:

```text
TODOS os registros da tabela serão alterados.
```

Isso pode causar perda de dados irreversível.

---

## ✅ Usando a Chave Primária com Segurança

A forma mais segura de atualizar registros é utilizando a **PRIMARY KEY**.

```sql
UPDATE cursos
SET carga = 30
WHERE idcurso = 3;
```

Como a chave primária é única, apenas **um registro será alterado**.

---

## 🛡️ LIMIT como Proteção Extra

Podemos limitar a quantidade de registros afetados:

```sql
UPDATE cursos
SET carga = 25
WHERE ano = 2024
LIMIT 1;
```

Mesmo que o `WHERE` esteja incorreto, apenas uma linha será alterada.

---

## 🔒 Safe Updates (MySQL Workbench)

O MySQL Workbench possui um modo de segurança chamado:

```text
Safe Updates
```

Ele impede comandos `UPDATE` e `DELETE` que:

```text
- Não utilizem WHERE
- Não utilizem chave primária
```

Isso evita acidentes durante o desenvolvimento.

---

## 💾 A Importância do Backup

Manipular dados exige responsabilidade.

Boas práticas:

```text
- Fazer backup antes de alterações grandes
- Testar comandos com SELECT antes
- Usar WHERE com chave primária
- Usar LIMIT quando possível
```

Exemplo de teste antes do UPDATE:

```sql
SELECT * FROM cursos
WHERE idcurso = 3;
```

Depois disso:

```sql
UPDATE cursos
SET carga = 30
WHERE idcurso = 3;
```

---

## 📊 Resumo Rápido

* `UPDATE` modifica registros existentes
* `SET` define os novos valores
* `WHERE` define quais registros serão alterados
* A **chave primária** é a forma mais segura de atualização
* `LIMIT` adiciona proteção extra
* Safe Updates evita comandos perigosos
* Backup é essencial antes de alterações grandes

---

> 💡 **Dica:**
> "Antes de atualizar dados, sempre pergunte: *tenho certeza de quais registros serão afetados?* Se houver dúvida, faça um SELECT primeiro."

---
