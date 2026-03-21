# 📚 Aula 10 — SELECT Avançado (Parte 2)

---

## 🎯 Objetivos da Aula

* Utilizar o operador **LIKE** para buscas por padrão
* Trabalhar com caracteres coringa (**%** e **_**)
* Aplicar **NOT LIKE** para exclusões
* Utilizar **DISTINCT** para eliminar duplicidades
* Compreender e aplicar **funções de agregação**
* Realizar consultas mais inteligentes e eficientes

---

# 🔎 Buscas por Padrão com LIKE

Até agora usamos comparações exatas com `=`.

Mas em muitos casos queremos buscar **padrões de texto**, como:

```text
- Nomes que começam com "A"
- Emails que terminam com ".com"
- Palavras que contenham determinada letra
```

Para isso usamos o operador:

```sql
LIKE
```

---

# 🔤 Caracteres Coringa (Wildcards)

## % (Porcentagem)

Representa:

```text
0, 1 ou vários caracteres
```

---

### Exemplos

Começa com "A":

```sql
SELECT * FROM alunos
WHERE nome LIKE 'A%';
```

---

Termina com "A":

```sql
SELECT * FROM alunos
WHERE nome LIKE '%A';
```

---

Contém "A" em qualquer posição:

```sql
SELECT * FROM alunos
WHERE nome LIKE '%A%';
```

---

## _ (Sublinhado)

Representa:

```text
exatamente 1 caractere
```

---

### Exemplo

```sql
SELECT * FROM alunos
WHERE nome LIKE '_a%';
```

Isso retorna nomes onde:

```text
- Primeiro caractere: qualquer
- Segundo caractere: "a"
```

Exemplo de resultado:

```text
Maria
Carlos
```

---

# 🚫 NOT LIKE

Usado para **excluir padrões**.

```sql
SELECT * FROM alunos
WHERE nome NOT LIKE 'A%';
```

Resultado:

```text
Todos os nomes que NÃO começam com "A"
```

---

# ⚠️ Case Sensitivity (Importante)

No MySQL, por padrão:

```text
LIKE → NÃO diferencia maiúsculas de minúsculas
```

Ou seja:

```sql
WHERE nome LIKE 'a%'
```

é equivalente a:

```sql
WHERE nome LIKE 'A%'
```

Além disso:

```text
- Suporte a acentos depende da collation (UTF-8 geralmente funciona bem)
```

---

# 🧹 Eliminando Duplicados com DISTINCT

Às vezes uma consulta retorna valores repetidos.

Exemplo:

```sql
SELECT nacionalidade FROM alunos;
```

Resultado:

```text
Brasil
Brasil
Argentina
Brasil
Chile
```

---

## Usando DISTINCT

```sql
SELECT DISTINCT nacionalidade FROM alunos;
```

Resultado:

```text
Brasil
Argentina
Chile
```

---

💡 O `DISTINCT` funciona como um **"filtro de duplicidade"**.

---

# 📊 Funções de Agregação

Permitem realizar **cálculos diretamente no banco de dados**.

---

## 🔢 COUNT() — Contagem

Conta quantos registros existem.

```sql
SELECT COUNT(*) FROM alunos;
```

Resultado:

```text
Quantidade total de alunos
```

---

Com filtro:

```sql
SELECT COUNT(*) FROM alunos
WHERE nacionalidade = 'Brasil';
```

---

## 🔝 MAX() — Maior Valor

```sql
SELECT MAX(carga_horaria) FROM cursos;
```

Resultado:

```text
Maior carga horária
```

---

## 🔻 MIN() — Menor Valor

```sql
SELECT MIN(carga_horaria) FROM cursos;
```

---

## ➕ SUM() — Soma

```sql
SELECT SUM(carga_horaria) FROM cursos;
```

Resultado:

```text
Soma total das cargas horárias
```

---

## 📉 AVG() — Média

```sql
SELECT AVG(carga_horaria) FROM cursos;
```

Resultado:

```text
Média das cargas horárias
```

---

# 🧠 Observações Importantes

✔ Funções de agregação retornam **um único valor**

✔ Podem ser combinadas com `WHERE`

✔ Ignoram valores `NULL` automaticamente

---

# 🔄 Exemplo Completo

```sql
SELECT 
    COUNT(*) AS total_cursos,
    AVG(carga_horaria) AS media,
    MAX(carga_horaria) AS maior,
    MIN(carga_horaria) AS menor
FROM cursos
WHERE ano >= 2022;
```

---

# 📊 Resumo Rápido

* **LIKE** busca padrões em textos
* `%` representa qualquer quantidade de caracteres
* `_` representa um único caractere
* **NOT LIKE** exclui padrões
* **DISTINCT** remove duplicidades
* **COUNT()** conta registros
* **MAX()** maior valor
* **MIN()** menor valor
* **SUM()** soma valores
* **AVG()** média dos valores

---

💡 **Dica de desenvolvedor**

Com `LIKE`, `DISTINCT` e funções de agregação, você começa a sair do básico e entrar no nível onde o banco **realmente trabalha por você**.

Esses recursos são muito usados em:

```text
- Relatórios
- Dashboards
- APIs
- Análise de dados
```

Dominar isso é um grande passo para se tornar um dev mais completo 🚀
