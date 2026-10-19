---
title: MS SQL Server Startup Time
description: Useful, for example, for determining how long server wait statistics have been accumulating.
created: 2019-03-08 07:47:23
tags:
- mssql
---

Why would you need to know when MS SQL Server started? Say we’re looking into DBMS waits and want to know how long the [sys.dm_os_wait_stats](https://docs.microsoft.com/ru-ru/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) DMV has been collecting data. It keeps its data in RAM from the moment the DBMS starts, just like the other DMVs.

You can get the startup time from [sys.dm_os_sys_info](https://docs.microsoft.com/ru-ru/sql/relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql):

    SELECT sqlserver_start_time FROM sys.dm_os_sys_info

There’s another way, using tempdb. It’s created when the server starts, so its creation time gives us the same answer. You can get it from [sys.databases](https://docs.microsoft.com/ru-ru/sql/relational-databases/system-catalog-views/sys-databases-transact-sql):

    SELECT create_date FROM sys.databases WHERE name = 'tempdb'

One catch: the server’s startup time doesn’t necessarily tell you when DMV data collection began. Someone might have cleared the stats by hand, for example like this:

    DBCC SQLPERF('sys.dm_os_wait_stats', CLEAR)
