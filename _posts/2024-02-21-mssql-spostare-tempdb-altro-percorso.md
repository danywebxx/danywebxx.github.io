---
title: MS-SQL - spostare il database TempDB in altra cartella
date: 2024-02-21
categories: [blogging, tutorial]
tags: [appunti, microsoft, sql, database]
description: In un'installazione MS SQL Server può divenire necessario spostare i database di sistema come il TempDB. 
image:
     path: /assets/2024-02-21/mssql-feature.png
---
Ho purtroppo un rapporto di odio con la gestione di `database MS SQL`. Non è il mio mondo, mi piace poco e non ci metto abbastanza la testa.
Fatta questa dovuta premessa, mi capita di doverne gestire e di conseguenza sbatterci la testa.
La scorsa settimana stavo preparando un’installazione `MS SQL Serger` su Azure seguendo le best-practices quando mi accorgo di aver fatto la cazzata. Il `TempDB` installato su un disco sbagliato.

## A cosa serve il tempdb?
In MS SQL Server il tempdb è un database di sistema le cui funzioni principali sono la memorizzazione di tabelle temporanee, cursori, procedure memorizzate e altri oggetti interni creati dal motore del database. Cit.: Wikipedia

## Come spostare il tempdb
Purtroppo rispetto ai database tradizionali non è possibile spostarlo facendo la procedura di detach-attach. Bisogna seguire una procedura diversa ma non troppo complicata che riporto sotto:
1. Identificare la posizione del file dati e del file di log del TempDB
2. Modificare la loro posizione 
3. Arrestare e riavviare il servizio SQL Server
4. Verificare che tutto sia stato spostato correttamente
5. Eliminare i vecchi file tempdb.mdf e templog.ldf

### Verificare la posizione dei file del tempdb 
Verificare la posizione dei file "tempdb.mdf" e "tempdb.ldf" con una query:
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

Il risultato sarà simile a quello dell'immagine

![Posizione del tempdb](/assets/2024-02-21/sql-temp-db.png)
_Posizione di partenza del TempoDB_
### Spostare il TempDB
Spostare il tempdb eseguendo la seconda query (modificando il percorso dei file "tempdb.mdf" e "tempdb.ldf".
```sql
USE master;
GO

ALTER DATABASE tempdb MODIFY FILE (
	NAME = tempdev
	,FILENAME = 'path di destizione\tempdb.mdf'
	);
GO

ALTER DATABASE tempdb MODIFY FILE (
	NAME = templog
	,FILENAME = 'path di destizione\templog.ldf'
	);
GO
```
### Riavvio dei servizi
Riavviare i servizi di MS SQL Server
```
net stop MSSQL$nome_istanza
net start MSSQL$nome_istanza
```

### Controllo ed eliminazione dei vecchi file
A questo punto meglo rieseguire la prima query e verificare che il tempdb sia stato spostato correttamente  nella cartella di destinazione.
Se tutto è corretto si possono cancellare i file "tempdb.mdf" e "tempdb.ldf" nella cartella di origine.

## Bibliografia
- [Datamaze](https://www.datamaze.it/blogs/post/come-spostare-il-tempdb-su-una-nuova-unit%C3%A0-disco)
- [Checklist: Best practices for SQL Server on Azure VMs](https://learn.microsoft.com/en-us/azure/azure-sql/virtual-machines/windows/performance-guidelines-best-practices-checklist?view=azuresql)
