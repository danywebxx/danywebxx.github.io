---
title: MS-SQL - spostare il tempdb in altra cartella
date: 2024-02-21
categories: 
tags: 
description:
---

blablavla

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

testo
```sql
test
```


## riferimenti
https://www.datamaze.it/blogs/post/come-spostare-il-tempdb-su-una-nuova-unit%C3%A0-disco