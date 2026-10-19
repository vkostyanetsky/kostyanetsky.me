---
title: MS SQL Server Waits
description: Whenever a SQL query could run but is waiting for another resource, information about the reason for the wait is recorded.
created: 2019-03-08 06:40:05
tags:
- mssql
---

Whenever a SQL query could run but is waiting for another resource, information about the reason for the wait is recorded. You can access it through [sys.dm_os_wait_stats](https://docs.microsoft.com/ru-ru/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

Here are a couple of ready-made scripts to make sense of it:

- A [script](causes_of_wait_times.txt) from Ian Stirk’s article “Uncover Hidden Data to Optimize Application Performance,” published in MSDN Magazine back in [2008](https://msdn.microsoft.com/ru-ru/magazine/ee310108.aspx). It lists wait types ordered by time, from the most prominent to the least.
- A [script](sql_server_wait_statistics.txt) from Paul Randal’s [article](https://www.sqlskills.com/blogs/paul/wait-statistics-or-please-tell-me-where-it-hurts/) about analyzing MS SQL Server waits; there’s also a Russian [translation](https://habr.com/ru/post/216309/) on Habr. It filters out waits that always occur and can usually be ignored. It also adds a link to a detailed explanation of each wait, such as this [description](https://www.sqlskills.com/help/waits/cxpacket) of CXPACKET.
