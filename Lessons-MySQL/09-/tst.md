# 📚 Aula 9 — Consultando Dados com SELECT no MySQL

---

## 🎯 Objetivos da Aula

* Dominar o comando SELECT, a ferramenta mais importante do SQL
* Aprender a selecionar colunas específicas e todas as colunas
* Compreender a ordenação de resultados com ORDER BY
* Utilizar a cláusula WHERE para filtrar registros com precisão
* Aplicar operadores de faixa (BETWEEN) e lista (IN)
* Combinar condições com operadores lógicos AND e OR
* Entender a classificação do SELECT como DQL (Data Query Language)

---

# 🔎 O Comando SELECT

O comando **SELECT** é o **mais utilizado em SQL**.

Ele serve para **consultar dados armazenados em tabelas**.

Diferente de comandos que **alteram o banco**, o `SELECT` apenas **lê os dados**.

---
## 📊 Selecionando Todas as Colunas

O caractere `*` representa **todas as colunas da tabela**.

```sql
SELECT * FROM cursos;
```

ou seja, sssa consulta retorna **todos os registros da tabela**.

>⚠️ Em sistemas grandes, o uso de `*` pode ser **ineficiente**, pois retorna mais dados do que o necessário.

---

# 🎯 Selecionando Colunas Específicas

Podemos escolher exatamente **quais colunas queremos visualizar**.

### Estrutura fundamental do SELECT

```sql
SELECT nome, ano
FROM cursos;
```

Resultado:

```
nome | ano
----------
Java | 2023
SQL  | 2024
C    | 2022
```

Também é possível **alterar a ordem das colunas** no resultado.

```sql
SELECT ano, nome
FROM cursos;
```

---

# 📈 Ordenando Resultados com ORDER BY

Por padrão, o MySQL **não garante uma ordem específica** na consulta.

Para organizar os resultados utilizamos:

```sql
ORDER BY
```

---

### Ordenação Ascendente (ASC)

É a **ordem padrão**.

```sql
SELECT * FROM cursos
ORDER BY nome ASC; // ASC é opcional (padrão)
```

Exemplo:
```
SELECT nome, preco, carga_horaria
FROM curso
ORDER BY nome ASC;  
```

- Resultado:
```
-- +--------------------+--------+---------------+
-- | nome               | preco  | carga_horaria |
-- +--------------------+--------+---------------+
-- | Banco de Dados MySQL| 520.00 |            50 |
-- | Excel Básico       | 220.00 |            20 |
-- | HTML e CSS         | 320.00 |            30 |
```
#### Ordenar por Preço (menor para maior)
```
SELECT nome, preco, area
FROM curso
ORDER BY preco;  -- ASC implícito
```
- Resultado: Do mais barato ao mais caro
---

### Ordenação Descendente (DESC)

Mostra os dados **do maior para o menor** ou **de trás para frente**.

```sql
SELECT * FROM cursos
ORDER BY ano DESC;
```

Exemplo:
#### Ordenar por Preço
```sql
SELECT nome, preco, area
FROM curso
ORDER BY preco DESC;
```

- Resultado: Do mais caro ao mais barato
```
-- +--------------------+--------+-----------------+
-- | nome               | preco  | area            |
-- +--------------------+--------+-----------------+
-- | Machine Learning   | 890.00 | Programação     |
-- | Python para Dados  | 580.00 | Programação     |
-- | Redes de Computadores| 550.00| Infraestrutura  |
```

#### Ordenar por Ano (mais recente primeiro)
```
SELECT nome, ano_lancamento, preco
FROM curso
ORDER BY ano_lancamento DESC, preco DESC;
```
- Resultado: Ano 2023 primeiro, dentro dele, os mais caros primeiro

---
### ⚠️ CUIDADO: DESC vs DESCRIBE

🚨 CONFUSÃO COMUM:
```sql
DESC curso;  -- Comando DESCRIBE (estrutura da tabela)
-- Mostra: Field, Type, Null, Key, Default, Extra

-- ORDER BY ... DESC;  -- Parâmetro DESCending (ordenar decrescente)
SELECT * FROM curso ORDER BY preco DESC;  -- Ordenação
```
São coisas TOTALMENTE diferentes!

---

### Ordenação Múltipla (Por Várias Colunas)

Podemos organizar por **mais de um critério**.

```sql
SELECT area, nome, preco
FROM curso
ORDER BY area ASC, nome ASC;
```
- Resultado: Primeiro agrupa por área, depois ordena nomes dentro de cada área
```
-- +-----------------+--------------------+--------+
-- | area            | nome               | preco  |
-- +-----------------+--------------------+--------+
-- | Banco de Dados  | Banco de Dados MySQL|520.00 |
-- | Design          | Photoshop          |380.00 |
-- | Idiomas         | Inglês Técnico     |350.00 |
-- | Infraestrutura  | Redes de Computadores|550.00 |
-- | Produtividade   | Excel Básico       |220.00 |
-- | Programação     | Java Fundamentos   |450.00 |
-- | Programação     | JavaScript Avançado|490.00 |
-- | Programação     | Machine Learning   |890.00 |
-- | Programação     | Python para Dados  |580.00 |
-- | Web Design      | HTML e CSS         |320.00 |
-- +-----------------+--------------------+--------+
```

---


# 🔍 Filtrando Registros com WHERE

A cláusula **WHERE** permite **filtrar quais registros devem aparecer no resultado**.

Exemplo:

```sql
SELECT * FROM cursos
WHERE ano = 2023;
```

Result Set: O resultado retornado pela consulta.

```
id | nome | carga_horaria | ano
--------------------------------
1  | Java | 40             | 2023
```

---

### ⚙️ Operadores Relacionais

Para construir filtros usamos operadores comparativos.

| Operador | Significado    |
|----------| -------------- |
| =        | Igual          |
| != ou <> | Diferente      |
| '>        | Maior          |
| <        | Menor          |
| >=       | Maior ou igual |
| <=       | Menor ou igual |

### Exemplos

Maior que:

```sql
SELECT * FROM cursos
WHERE carga_horaria > 40;
```

Menor ou igual:

```sql
SELECT * FROM cursos
WHERE ano <= 2023;
```

Diferente:

```sql
SELECT * FROM cursos
WHERE ano <> 2024;
```

---

# 📦 Operadores de Faixa

### 📏 Operador BETWEEN: 
Seleciona valores **dentro de um intervalo**.

Importante: **os limites são incluídos**.

```sql
SELECT * FROM cursos
WHERE ano BETWEEN 2022 AND 2024;
```
- Resultado: Cursos lançados entre 2021 e 2022 (inclusive)

Equivalente a:

```sql
WHERE ano >= 2022 AND ano <= 2024
```

---

### 📋 Operador IN:

Permite verificar se um valor está **dentro de uma lista específica**.

Exemplo:

```sql
SELECT * FROM cursos
WHERE ano IN (2022, 2024);
```

Resultado:

```
Cursos de 2022 e 2024
```

O que estiver **fora dessa lista é ignorado**.

---

# 🧠 Operadores Lógicos

Permitem combinar múltiplas condições.


### AND (E)

Todas as condições precisam ser **verdadeiras ao mesmo tempo**.

```sql
SELECT * FROM cursos
WHERE ano = 2023 AND carga_horaria > 30;
```

Somente registros que atendem **ambas as condições** aparecem.

---

### OR (OU)

Basta **uma condição ser verdadeira**.

```sql
SELECT * FROM cursos
WHERE ano = 2023 OR ano = 2024;
```

Resultado:

```
Cursos de 2023 ou 2024
```

---

## Combinação Complexa com AND e OR

> ⚠️ Precedência: AND tem prioridade sobre OR (Assim como multiplicação tem prioridade sobre adição)

Exemplo: Queremos Cursos de Programação OU Design, mas apenas ativos
```sql
SELECT nome, area, ativo
FROM curso
WHERE (area = 'Programação' OR area = 'Design')
  AND ativo = TRUE;
```
- Os parênteses são ESSENCIAIS!
- Sem parênteses, seria interpretado como:
- WHERE area = 'Programação' OR (area = 'Design' AND ativo = TRUE)

---

# 🏷️ Classificação do SELECT

Existe uma discussão teórica sobre a classificação do comando.

Alguns autores classificam como:

```
DML — Data Manipulation Language
```

Pois ele **manipula dados consultando registros**.

Outros classificam como:

```
DQL — Data Query Language
```

Pois o comando **apenas consulta informações**, sem alterar nada no banco.


### Na Prática: A Classificação Não Muda a Sintaxe


- Independente da classificação, o SELECT funciona igual
- O importante é saber usar!
---

## 📋 Resumo Rápido

* **SELECT**: Comando para consultar dados (classificado como DQL)
* **Seleção de colunas**: `*` para todas, ou liste colunas específicas
* **ORDER BY**: Organiza resultados (`ASC` crescente/padrão, `DESC` decrescente)
* **WHERE**: Filtra registros com condições
* **Operadores relacionais**: `=`, `!=`, `<>`, `<`, `>`, `<=`, `>=`
* **BETWEEN**: Valores em um intervalo (inclusive)
* **IN**: Valores em uma lista específica
* **AND**: Todas as condições devem ser verdadeiras
* **OR**: Pelo menos uma condição deve ser verdadeira
* **Result Set**: Conjunto de resultados gerado pela consulta
* **DQL vs DML**: SELECT não altera dados, apenas consulta

---

>💡**Dica:** Grande parte do trabalho com bancos de dados **não é inserir dados**, e sim **consultar corretamente as informações**.

>Dominar o **SELECT** é essencial para trabalhar com **relatórios, APIs, dashboards e sistemas corporativos**.
