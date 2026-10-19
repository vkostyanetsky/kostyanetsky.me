---
title: Date Offsets in 1C
description: MS SQL Server DATETIME fields cannot store dates before 1753. Do you know why?
created: 2020-11-10 19:06:41
tags:
- 1c
- mssql
---

MS SQL Server DATETIME fields [cannot](https://docs.microsoft.com/ru-ru/dotnet/api/system.data.sqltypes.sqldatetime.minvalue?view=dotnet-plat-ext-3.1) store dates before 1753. Try writing January 1, 0001 to the database, for example, and you’ll get an out-of-range value error. I thought this was a funny old quirk for such a well-established DBMS until I stumbled across the [reason](https://stackoverflow.com/questions/3310569/what-is-the-significance-of-1-1-1753-in-sql-server).

Briefly, Britain adopted the Gregorian calendar in 1752, losing eleven days in the process. That created a problem: suppose a user wants the difference in days between 1653 and 1753. What should happen? Account for the missing days? Ignore them? Add hints or settings?

Apparently, the developers decided not to get into all that and just cut off the date range. If you need earlier dates, DATETIME2 doesn’t have that restriction.

As for 1C, the platform originally used DATETIME and devised a special [workaround](https://its.1c.ru/db/metod8dev/content/4055/hdoc) to avoid trouble storing dates before 1753. In short, when writing dates to the database, it quietly adds two thousand years to each one, then subtracts them when reading. A user sees January 1, 2000 in 1C, while the database actually stores January 1, 4000.

I wonder why 1C hasn’t thrown this out yet. The platform uses DATETIME2 now, so it shouldn’t need the offset trick anymore. Maybe there’s a catch, or maybe they’re just being careful, but the DBMS developers themselves [seem quite confident](https://stackoverflow.com/questions/3310569/what-is-the-significance-of-1-1-1753-in-sql-server/3310627#3310627):

> Your great great great great great great great grandfather should upgrade to SQL Server 2008 and use the DateTime2 data type, which supports dates in the range: 0001-01-01 through 9999-12-31.

> *Joe Stefanelli (SQL Server developer)*

Perhaps it’s simply at the bottom of the priority list. Adding and subtracting two thousand years for every date increases hardware load, of course, but it gets lost among everything else.
