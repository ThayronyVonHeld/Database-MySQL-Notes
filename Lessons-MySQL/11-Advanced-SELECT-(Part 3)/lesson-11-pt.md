# 📚 Aula 11 — SELECT Avançado (Parte 3)

---

## 🎯 Objetivos da Aula

* Diferenciar **DISTINCT** e **GROUP BY**
* Agrupar dados com **GROUP BY**
* Utilizar funções de agregação em grupos
* Filtrar grupos com **HAVING**
* Compreender a diferença entre **WHERE** e **HAVING**
* Criar **subconsultas (SELECT dentro de SELECT)**
* Entender a **ordem de execução** de uma query SQL

---

# 🔍 DISTINCT vs GROUP BY

Esses dois conceitos parecem parecidos, mas têm objetivos diferentes.

---

## 🧹 DISTINCT (Eliminar Repetições)

Remove valores duplicados.

```sql
SELECT DISTINCT nacionalidade FROM alunos;
```

Resultado:

```text
Brasil
Argentina
Chile
```

👉 Aqui você só vê **valores únicos**, sem saber quantas vezes aparecem.

---

## 📦 GROUP BY (Agrupar Dados)

Agrupa registros semelhantes para permitir **análises**.

```sql
SELECT nacionalidade FROM alunos
GROUP BY nacionalidade;
```

Resultado:

```text
Brasil
Argentina
Chile
```

👉 Visualmente pode parecer igual ao `DISTINCT`, mas o **GROUP BY permite muito mais**.

---

# 📊 GROUP BY + Funções de Agregação

Agora vem o verdadeiro poder.

```sql
SELECT nacionalidade, COUNT(*) 
FROM alunos
GROUP BY nacionalidade;
```

Resultado:

```text
Brasil     | 3
Argentina  | 1
Chile      | 1
```

---

💡 Aqui você não só vê os dados — você **analisa os dados**.

---

## 📌 Outro Exemplo

```sql
SELECT carga_horaria, COUNT(*)
FROM cursos
GROUP BY carga_horaria;
```

Resultado:

```text
40 | 6 cursos
20 | 1 curso
30 | 2 cursos
```

---

# 🔎 Filtrando Grupos com HAVING

O `HAVING` funciona como um:

```text
WHERE para grupos
```

---

## Exemplo

```sql
SELECT nacionalidade, COUNT(*) 
FROM alunos
GROUP BY nacionalidade
HAVING COUNT(*) > 2;
```

Resultado:

```text
Brasil | 3
```

---

# ⚠️ WHERE vs HAVING

Essa diferença é **CRÍTICA**:

| WHERE                        | HAVING           |
| ---------------------------- | ---------------- |
| Filtra registros individuais | Filtra grupos    |
| Executado antes do GROUP BY  | Executado depois |
| Não usa agregação            | Usa agregação    |

---

## Comparação Prática

### WHERE (antes do agrupamento)

```sql
SELECT * FROM alunos
WHERE idade > 18;
```

---

### HAVING (depois do agrupamento)

```sql
SELECT idade, COUNT(*)
FROM alunos
GROUP BY idade
HAVING COUNT(*) > 2;
```

---

# 🧠 Subconsultas (Subqueries)

Uma **subconsulta** é um `SELECT` dentro de outro `SELECT`.

---

## 🎯 Exemplo Clássico

Cursos com carga horária acima da média:

```sql
SELECT * FROM cursos
WHERE carga_horaria > (
    SELECT AVG(carga_horaria) FROM cursos
);
```

---

💡 O que acontece aqui:

1. A subconsulta calcula a média
2. O SELECT principal usa esse valor como filtro

---

## 🧩 Outro Exemplo com HAVING

```sql
SELECT carga_horaria, COUNT(*)
FROM cursos
GROUP BY carga_horaria
HAVING carga_horaria > (
    SELECT AVG(carga_horaria) FROM cursos
);
```

---

# ⚙️ Ordem de Execução do SELECT

Mesmo que escrevamos assim:

```sql
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ...
```

O MySQL executa nessa ordem:

```text
1. FROM       → define a origem dos dados
2. WHERE      → filtra registros
3. GROUP BY   → agrupa os dados
4. HAVING     → filtra os grupos
5. SELECT     → escolhe as colunas
6. ORDER BY   → ordena o resultado final
```


💡 Entender isso evita MUITOS erros.

---

# 📋 Resumo Rápido

| Conceito | O que faz | Quando usar |
|----------|-----------|-------------|
| **DISTINCT** | Elimina valores repetidos | Apenas listar valores únicos |
| **GROUP BY** | Organiza dados em grupos | Calcular estatísticas por grupo |
| **COUNT()** | Conta registros por grupo | Saber quantos em cada grupo |
| **SUM()** | Soma valores numéricos | Totalizar valores por grupo |
| **AVG()** | Calcula média por grupo | Médias estatísticas |
| **MAX()/MIN()** | Maior/menor valor por grupo | Extremos por grupo |
| **WHERE** | Filtra LINHAS (antes do GROUP BY) | Filtrar registros individuais |
| **HAVING** | Filtra GRUPOS (depois do GROUP BY) | Filtrar resultados de agregação |
| **Subconsulta** | SELECT dentro de SELECT | Consultas dinâmicas e complexas |

---

>💡 **Dica:** Dominando o comando SELECT você de destaca MUITO no mercado e é capaz de realizar:
> - Relatórios complexos
> - Dashboards profissionais 
> - APIs inteligentes 
> - Análise de dados real

