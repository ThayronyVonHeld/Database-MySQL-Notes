# 📚 Aula 8 — Backup e Restauração no MySQL (Dump)

---

## 🎯 Objetivos da Aula

* Entender a importância do backup em bancos de dados
* Compreender o conceito de **Dump**
* Aprender a exportar bancos de dados no MySQL Workbench
* Aprender a importar arquivos `.sql`
* Diferenciar servidor de banco de dados e ferramenta de interface
* Desenvolver responsabilidade ao manipular dados

---

## 💾 Por Que Backup é Essencial?

Diferente de editores de texto, o MySQL **não possui Control + Z**.

Comandos como:

```sql
UPDATE
DELETE
DROP TABLE
DROP DATABASE
```

podem apagar dados ou estruturas **permanentemente**.

Sem backup:

```text
Não há recuperação.
```

Por isso, todo profissional precisa dominar o processo de cópia de segurança.

---

## 🧠 O Que é um Dump?

No contexto de banco de dados, backup é chamado de:

```text
DUMP
```

Um dump é um **arquivo de texto (.sql)** contendo uma sequência de comandos SQL que recriam completamente o banco.

Ele pode conter:

```sql
CREATE DATABASE escola;
USE escola;

DROP TABLE IF EXISTS aluno;

CREATE TABLE aluno (...);

INSERT INTO aluno VALUES (...);
```

Ou seja:

```text
Um dump não é uma imagem do banco.
É um roteiro completo de reconstrução.
```

---

## 📤 Exportação (Gerando o Backup)

No MySQL Workbench:

```text
Server → Data Export
```

---

### 🔎 Seleção de Objetos

Você pode escolher:

```text
- O banco inteiro (Schema)
- Apenas tabelas específicas
```

---

### ⚙️ Opções de Exportação

#### Dump Structure and Data

```text
Exporta estrutura + registros
```

#### Dump Structure Only

```text
Exporta apenas as tabelas (sem dados)
```

#### Dump Data Only

```text
Exporta apenas os registros
```

---

### 📄 Self-contained File

Opção recomendada:

```text
Export to Self-contained File
```

Gera um único arquivo `.sql`, facilitando transporte e armazenamento.

---

### 🏗️ Include Create Schema

Essa opção é fundamental.

Se marcada, o dump incluirá:

```sql
CREATE DATABASE nome_do_banco;
```

Isso permite restaurar o banco automaticamente em outro servidor.

---

## 📥 Importação (Restaurando o Banco)

No MySQL Workbench:

```text
Server → Data Import
```

Passos:

1. Selecionar o arquivo `.sql`
2. Escolher o destino
3. Clicar em **Start Import**

Após a importação:

```text
Atualizar (Refresh) a lista de Schemas
```

O banco reaparecerá no servidor.

---

## 🖥️ Ferramenta vs Servidor

É fundamental entender:

```text
MySQL Workbench ≠ Banco de Dados
```

O Workbench é apenas uma interface gráfica.

O banco real está no:

```text
Servidor MySQL
```

Em ambiente de estudo, ele pode estar sendo executado via:

```text
WAMP Server
```

O dump permite mover dados entre servidores diferentes.

---

## 🔄 Fluxo Real de Uso Profissional

```text
1. Desenvolvedor cria banco localmente
2. Gera dump (.sql)
3. Envia para servidor de produção
4. Importa no servidor final
```

Ou ainda:

```text
Baixar banco de testes da internet
Importar para estudo
Explorar consultas
```

---

## ⚠️ Boas Práticas

Antes de:

```text
- Alterações grandes
- Atualizações em massa
- Exclusões estruturais
```

Sempre:

```text
✔ Fazer backup
✔ Testar em ambiente de desenvolvimento
✔ Confirmar servidor correto
```

---

## 📊 Resumo Rápido

* Dump é um arquivo `.sql` com comandos de reconstrução
* MySQL não possui desfazer (undo)
* Exportação é feita em **Server → Data Export**
* Importação é feita em **Server → Data Import**
* Workbench é ferramenta, não servidor
* Backup é obrigação profissional

---

> 💡 **Dica Profissional:**
> “Desenvolvedor iniciante faz código. Desenvolvedor profissional faz backup.”

---
