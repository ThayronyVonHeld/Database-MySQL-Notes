# 📚 Aula 8 — Backup e Restauração no MySQL (Dump)

---

## 🎯 Objetivos da Aula

* Entender a importância do backup em bancos de dados
* Compreender o conceito de **Dump**
* Aprender a exportar bancos de dados no MySQL Workbench
* Praticar a importação de backups para recuperação de dados
* Diferenciar servidor de banco de dados e ferramenta de interface
* Desenvolver responsabilidade ao manipular dados

---

## 💾 Por Que Backup é Essencial?

Diferente de editores de texto, o MySQL **não possui Control + Z**.

Exemplos Reais:

```sql
-- CENÁRIO 1: DELETE sem WHERE (pesadelo de todo DBA)
DELETE FROM clientes;  
-- Resultado: TODOS os clientes sumiram!

-- CENÁRIO 2: DROP DATABASE acidental
DROP DATABASE empresa;
-- Resultado: A empresa inteira sumiu do servidor!

-- CENÁRIO 3: UPDATE com condição errada
UPDATE produtos SET preco = 0 WHERE categoria = 'eletronicos';
-- Resultado: Todos os produtos eletrônicos de graça!

-- CENÁRIO 4: Erro de ambiente
-- Tentou rodar script de produção no ambiente de desenvolvimento
-- Mas rodou no ambiente errado... e apagou dados reais!
```


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

### Anatomia de um Arquivo Dump

```sql
-- Exemplo real de como é um arquivo dump.sql
-- (Isso é TEXTO, não imagem binária!)

-- MariaDB dump 10.19  Distrib 10.4.28-MariaDB
-- Host: localhost    Database: escola
-- ------------------------------------------------------

-- Cria o banco se não existir
CREATE DATABASE IF NOT EXISTS `escola`;
USE `escola`;

-- Remove tabelas existentes para recriar limpas
DROP TABLE IF EXISTS `aluno`;

-- Recria a estrutura da tabela
CREATE TABLE `aluno` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `nome` varchar(100) NOT NULL,
    `email` varchar(100) DEFAULT NULL,
    `data_nascimento` date DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4;

-- Insere os dados novamente
INSERT INTO `aluno` VALUES 
(1, 'João Silva', 'joao@email.com', '2005-03-15'),
(2, 'Maria Santos', 'maria@email.com', '2004-07-22'),
(3, 'Pedro Oliveira', 'pedro@email.com', '2006-01-30');

-- Dump completed on 2024-01-31 15:30:00
```

### Por Que Dump é um Arquivo de Texto?

```text
✅ VANTAGENS DO FORMATO TEXTO (.sql):
- Legível por humanos (pode abrir no bloco de notas!)
- Editável (corrigir dados antes de restaurar)
- Versionável (git diff mostra mudanças)
- Portável (funciona em qualquer MySQL/MariaDB)
- Comprimível (pode virar .zip, .gz)
- Pesquisável (grep para encontrar algo)

❌ DESVANTAGENS:
- Arquivo grande pode ser pesado
- Importação mais lenta que formato binário
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

## 📋 Resumo Rápido

* Dump é um arquivo `.sql` com comandos de reconstrução
* MySQL não possui desfazer (undo)
* Exportação é feita em **Server → Data Export**
* Importação é feita em **Server → Data Import**
* Workbench é ferramenta, não servidor
* Backup é obrigação profissional

---

> 💡 Dica: Existem dois tipos de administradores de banco de dados: os que já perderam dados e os que ainda vão perder. A diferença está em quem tem backup e quem não tem.