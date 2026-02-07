# 📚 Aula 5 — Inserindo Dados com INSERT INTO (MySQL)

---

## 🎯 Objetivos da Aula

* Entender a diferença entre comandos **DDL** e **DML**
* Aprender a inserir dados em tabelas com **INSERT INTO**
* Inserir registros individuais e múltiplos
* Trabalhar corretamente com **datas, textos e números**
* Aplicar boas práticas de inserção de dados

---

## 1. DDL vs DML (Revisão Importante)

Antes de inserir dados, precisamos lembrar da divisão dos comandos SQL:

**DDL — Data Definition Language**
Define a estrutura do banco.

Exemplos:

```sql
CREATE DATABASE cadastro;
CREATE TABLE pessoas (...);
```

**DML — Data Manipulation Language**
Manipula os dados dentro das tabelas.

Exemplos:

```sql
INSERT
UPDATE
DELETE
```

Nesta aula, começamos oficialmente a trabalhar com **DML**.

---

## 2. O Comando INSERT INTO

O comando `INSERT INTO` é usado para **adicionar registros em uma tabela**.

Sintaxe completa:

```sql
INSERT INTO nome_da_tabela (campo1, campo2, campo3)
VALUES (valor1, valor2, valor3);
```

Exemplo:

```sql
INSERT INTO pessoas (nome, nascimento, sexo, peso, altura, nacionalidade)
VALUES ('Ana', '2004-05-10', 'F', '55.5', '1.65', 'Brasil');
```

Observe que:

* A ordem dos valores deve corresponder à ordem dos campos
* Textos devem estar entre **aspas simples**
* Datas usam o padrão **YYYY-MM-DD**

---

## 3. Trabalhando com AUTO_INCREMENT

Quando a tabela possui um campo ID com `AUTO_INCREMENT`, não precisamos informá-lo.

Exemplo:

```sql
INSERT INTO pessoas (nome, nascimento, sexo, peso, altura, nacionalidade)
VALUES ('Carlos', '2001-02-15', 'M', '78.2', '1.80', 'Brasil');
```

O MySQL gera automaticamente o ID.

Também é possível usar:

```sql
DEFAULT
```

Exemplo:

```sql
INSERT INTO pessoas VALUES
(DEFAULT, 'Maria', '1999-03-20', 'F', '60.0', '1.70', 'Brasil');
```

---

## 4. Inserção Simplificada

Se os valores forem informados **na mesma ordem da criação da tabela**, os campos podem ser omitidos:

```sql
INSERT INTO pessoas VALUES
(DEFAULT, 'João', '2000-01-01', 'M', '80.0', '1.75', 'Brasil');
```

Embora funcione, essa prática não é recomendada em projetos reais, pois depende da ordem exata das colunas.

---

## 5. Inserindo Múltiplos Registros

Podemos inserir vários registros em um único comando:

```sql
INSERT INTO pessoas (nome, nascimento, sexo, peso, altura, nacionalidade)
VALUES
('Lucas', '2003-07-12', 'M', '70.0', '1.72', 'Brasil'),
('Julia', '2005-11-30', 'F', '58.0', '1.60', 'Brasil'),
('Pedro', '1998-09-09', 'M', '90.0', '1.85', 'Brasil');
```

Isso melhora a eficiência e reduz o número de comandos executados.

---

## 6. Boas Práticas

### Não armazenar idade

Errado:

```sql
idade INT
```

Correto:

```sql
nascimento DATE
```

A idade deve ser calculada dinamicamente pelo sistema.

---

### Integridade de dados

Constraints ajudam a manter o banco consistente:

* `NOT NULL`
* `DEFAULT`
* `PRIMARY KEY`
* `AUTO_INCREMENT`

Essas regras evitam registros incompletos ou inválidos.

---

## 7. Ambiente de Prática

Ferramentas utilizadas:

* MySQL Server
* MySQL Workbench

A prática constante é essencial para aprender SQL de verdade.

Banco de dados não se aprende apenas lendo — é necessário executar comandos e testar cenários.

---

## 🧠 Resumo da Aula

Nesta aula você aprendeu:

* O que são comandos **DML**
* Como usar **INSERT INTO**
* Inserir registros únicos e múltiplos
* Trabalhar com **AUTO_INCREMENT**
* Utilizar boas práticas de inserção

A partir daqui, o banco começa a ficar **vivo**, pois já conseguimos cadastrar dados nas tabelas.
