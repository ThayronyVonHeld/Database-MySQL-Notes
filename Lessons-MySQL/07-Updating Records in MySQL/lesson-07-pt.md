# 📚 Aula 7 — Atualizando Registros no MySQL (UPDATE)

---

## 🎯 Objetivos da Aula

* Compreender a terminologia de bancos de dados: linhas, registros, tuplas
* Aprender a estrutura completa do comando **UPDATE**
* Entender a importância da cláusula **WHERE**
* Implementar medidas de segurança para evitar alterações acidentais
* Praticar atualizações em múltiplas colunas simultaneamente
* Entender os riscos e aprender técnicas de prevenção de erros

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
SET coluna = valor1
    coluna = valor2
    ...
WHERE condicao;
```

### Exemplo Prático Passo a Passo


- 1. Criar tabela de exemplo
```sql
    CREATE TABLE IF NOT EXISTS funcionario (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    cargo VARCHAR(50),
    salario DECIMAL(10,2),
    departamento VARCHAR(50),
    data_admissao DATE,
    ativo BOOLEAN DEFAULT TRUE
);
```

- 2. Inserir dados de teste
```sql
INSERT INTO funcionario (nome, cargo, salario, departamento, data_admissao) 
VALUES 
    ('João Silva', 'Analista', 3500.00, 'TI', '2023-01-15'),
    ('Maria Santos', 'Gerente', 5500.00, 'RH', '2022-03-10'),
    ('Pedro Oliveira', 'Desenvolvedor', 4200.00, 'TI', '2023-06-20'),
    ('Ana Costa', 'Analista', 3700.00, 'Financeiro', '2023-08-05');
```

- 3. Atualização Simples (um campo)
```sql
-- João foi promovido a Coordenador
UPDATE funcionario
SET cargo = 'Coordenador'
WHERE nome = 'João Silva';  -- Atualiza apenas João
```

- 4. Atualização Múltipla (vários campos)
```sql
-- Maria mudou de departamento e teve aumento
UPDATE funcionario
SET departamento = 'Administrativo',
    salario = 6000.00,
    cargo = 'Gerente Sênior'
WHERE nome = 'Maria Santos';
```

- 5. Atualização com Expressões
```sql
-- Aumento de 10% para todos do departamento TI
UPDATE funcionario
SET salario = salario * 1.10  -- Expressão matemática
WHERE departamento = 'TI';
```

- 6. Atualização com Funções do MySQL
```sql
-- Corrigir formato do nome (primeira letra maiúscula)
UPDATE funcionario
SET nome = CONCAT(
    UPPER(SUBSTRING(nome, 1, 1)),
    LOWER(SUBSTRING(nome, 2))
);
```

- 7. Atualização Condicional com Case
```sql
-- Aumento diferenciado por cargo
UPDATE funcionario
SET salario = CASE
    WHEN cargo LIKE '%Gerente%' THEN salario * 1.15
    WHEN cargo LIKE '%Coordenador%' THEN salario * 1.12
    WHEN cargo LIKE '%Analista%' THEN salario * 1.10
    ELSE salario * 1.05
END
WHERE ativo = TRUE;
```

- 8. Verificar Resultados
```sql
SELECT * FROM funcionario ORDER BY departamento, nome;
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

## 🔐 Segurança e o Uso da Chave Primária

A forma mais segura de atualizar registros é utilizando a **PRIMARY KEY**.

### Por que a Chave Primária é Essencial?


❌ CENÁRIO PERIGOSO: Atualizar sem chave primária
```sql
UPDATE funcionario
SET cargo = 'Estagiário'
WHERE nome = 'João Silva';
```

- Problema: E se existir outro "João Silva"?
- Problema: E se "João Silva" mudar o nome depois?

---

✅ CENÁRIO SEGURO: Sempre usar chave primária
```sql
UPDATE funcionario
SET cargo = 'Coordenador'
WHERE id = 1;  -- ID é ÚNICO e IMUTÁVEL
```

✅ BOA PRÁTICA: Selecionar antes de atualizar
```sql
-- Primeiro, veja o que vai atualizar
SELECT * FROM funcionario WHERE nome LIKE 'João%';
```

- Depois, atualize usando o ID correto
```sql
UPDATE funcionario
SET cargo = 'Coordenador'
WHERE id = 1;
```
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

> 💡 Dica: Antes de atualizar dados, sempre pergunte: *tenho certeza de quais registros serão afetados?* Se houver dúvida, faça um SELECT primeiro.
