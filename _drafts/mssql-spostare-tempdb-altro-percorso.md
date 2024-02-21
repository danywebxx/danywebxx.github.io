---
title: MS-SQL - spostare il tempdb in altra cartella
date: 2024-02-21
categories: blogging, tutorial
tags: appunti, microsoft, SQL
description:
---

identificare la posizione del tempdb

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