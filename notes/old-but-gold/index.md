---
title: Good Old DATETIME
description: After reading about MS SQL Server’s date types, I no longer wonder why the 1C platform still uses DATETIME.
created: 2020-11-11 17:00:50
tags:
- 1c
- mssql
---

I did some reading online about date types in MS SQL Server, and I no longer wonder why 1C still clings to its date offset. People describe a whole bundle of DATETIME2 problems:

1. You can’t do basic date arithmetic. Want to subtract two dates or add a day? You’ll need a workaround.
2. Standard functions still return good old DATETIME, DATEADD for example. If you store your data as DATETIME2, you have to convert it.
3. Fields of this type [don’t index well](https://sqlperformance.com/2016/04/sql-performance/surprises-dateadd), since each DATETIME2 value is stored backwards, time first, date second. So the DBMS gets its row count estimate wrong and comes up with a bad execution plan.

You can read more on [Towards Data Science](https://towardsdatascience.com/datetime2-why-you-should-not-use-it-70e50ae2bab9) or, for example, [SQL Server Central](https://www.sqlservercentral.com/forums/topic/why-do-some-dbas-avoid-datetime2).
