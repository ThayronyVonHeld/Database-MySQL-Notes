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

✅ SOLUÇÃO PARA GRANDES VOLUMES:
- mysqldump com compressão
- Ou use backups físicos (arquivos binários)
```

---

