# 📚 Aula 5 — Inserindo Dados com INSERT INTO (MySQL)

---

## 🎯 Objetivos da Aula

* Compreender a diferença entre DDL e DML na prática
* Aprender a inserir registros em tabelas usando SQL
* Entender a correspondência entre campos e valores
* Inserir múltiplos registros em um único comando
* Utilizar AUTO_INCREMENT, DEFAULT e NOT NULL corretamente
* Aplicar boas práticas de modelagem ao inserir dados

---

## 🧠 DDL vs DML na Prática

Antes de inserir dados, é importante lembrar a diferença entre dois grupos de comandos SQL:

```text
DDL → Define a estrutura
DML → Manipula os dados
```

### Exemplos de DDL

```sql
CREATE DATABASE escola;
CREATE TABLE aluno (...);
ALTER TABLE aluno ...;
```

### Exemplos de DML

```sql
INSERT INTO aluno ...;
UPDATE aluno ...;
DELETE FROM aluno ...;
```

Nesta aula, começamos a trabalhar com **DML**, alimentando as tabelas com informações reais.

---

## ✍️ O Comando INSERT INTO

O comando `INSERT INTO` é usado para inserir registros em uma tabela.

A lógica é simples:

```text
Campos → Valores correspondentes
```

---

### Sintaxe Completa (Forma Recomendada)

```sql
INSERT INTO aluno (nome, nascimento, sexo, peso, altura, nacionalidade)
VALUES ('Carlos', '2004-03-15', 'M', 78.5, 1.82, 'Brasil');
```

Regras importantes:

```text
- A ordem dos valores deve corresponder à ordem dos campos
- Textos devem estar entre aspas simples
- Datas usam o formato YYYY-MM-DD
- O comando termina com ponto e vírgula
```

---

## 🤖 AUTO_INCREMENT e DEFAULT

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

## 🧩 Integridade dos Dados

As regras definidas na tabela continuam valendo durante a inserção:

```text
NOT NULL → impede campos obrigatórios vazios
DEFAULT → define valores automáticos
PRIMARY KEY → impede duplicação de identificadores
ENUM → restringe valores possíveis
```

Exemplo inválido:

```sql
INSERT INTO aluno (sexo) VALUES ('X');
```

Resultado:

```text
Erro — valor não permitido pelo ENUM
```

---

## 🎂 Boa Prática: Idade vs Data de Nascimento

Nunca armazene idade diretamente:

```text
Idade muda com o tempo
Data de nascimento não
```

Exemplo correto:

```sql
nascimento DATE
```

A idade pode ser calculada futuramente via SQL ou aplicação.

---

## 🛠️ Ambiente de Prática

Você pode executar os comandos usando:

```text
MySQL Workbench
Terminal MySQL
Aplicações Java via JDBC
```

O importante é **praticar manualmente os comandos SQL**.

---

## 📊 Resumo Rápido

* INSERT INTO insere registros em tabelas
* DML manipula dados; DDL define estruturas
* AUTO_INCREMENT gera IDs automaticamente
* DEFAULT preenche valores não informados
* É possível inserir múltiplos registros
* Sempre prefira informar os campos no INSERT
* Armazene data de nascimento, não idade

---

> 💡 Dica: "Criar tabelas é modelagem. Inserir dados é testar se a modelagem realmente funciona."
