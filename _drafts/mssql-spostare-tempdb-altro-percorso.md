---
title: MS-SQL - spostare il tempdb in altra cartella
date: 2024-02-21
categories: blogging, tutorial
tags: appunti, microsoft, SQL
---
Ho purtroppo un rapporto di odio con la gestione di database MS SQL. Non è il mio mondo, mi piace poco e non ci metto abbastanza la testa.
Fatta questa dovuta premessa, mi capita di doverne gestire e di conseguenza sbatterci la testa.
La scorsa settimana stavo preparando un’installazione MS SQL Serger su Azure seguendo le best-practices quando mi accorgo di aver fatto la cazzata. Il tempdb installato su un disco sbagliato.

## A cosa serve il tempdb?
In MS SQL Server il tempdb è un database di sistema le cui funzioni principali sono la memorizzazione di tabelle temporanee, cursori, procedure memorizzate e altri oggetti interni creati dal motore del database. Cit.: Wikipedia

## Come spostare il tempdb
Purtroppo rispetto ai database tradizionali non è possibile spostarlo facendo la procedura di detach-attach. Bisogna seguire una procedura diversa ma non troppo complicata che riporto sotto:
1. Identificare la posizione del file dati e del file di log del TempDB
2. Modificare la loro posizione 
3. Arrestare e riavviare il servizio SQL Server
4. Verificare che tutto sia stato spostato correttamente
5. Eliminare i vecchi file tempdb.mdf e templog.ldf

### Identificare la posizione dei file del tempdb 

```sql
USE master;
GO

SELECT name AS [LogicalName]
	,physical_name AS [Location]
	,state_desc AS [Status]
FROM sys.master_files
WHERE database_id = DB_ID(N'tempdb');
GO
```

spostare il tempdb
```sql
USE master;
GO

ALTER DATABASE tempdb MODIFY FILE (
	NAME = tempdev
	,FILENAME = 'D:\MSSQL\DATA\tempdb.mdf'
	);
GO

ALTER DATABASE tempdb MODIFY FILE (
	NAME = templog
	,FILENAME = 'D:\MSSQL\DATA\templog.ldf'
	);
GO
```


## Bibliografia
- [Datamaze](https://www.datamaze.it/blogs/post/come-spostare-il-tempdb-su-una-nuova-unit%C3%A0-disco)