---
title: Deleting Temporary Tables
description: 'A curious detail: closing a temporary tables manager doesn’t remove those tables from tempdb.'
created: 2019-02-07 17:35:29
tags:
- 1c
---

A curious detail: closing a temporary tables manager doesn’t remove its tables from tempdb. It doesn’t matter whether you call Close() yourself or let the manager object disappear when the method that created it finishes. In both cases, only TRUNCATE runs, deleting the table’s data. The table sticks around in case the user runs another query with the same fields. That’s cheaper than making it all over again.

The same thing, deleting data rather than the table itself, happens when you explicitly use DESTROY in query text. That command doesn’t come in handy very often anyway. Maybe if the manager hangs around for a while and you’ve got a huge temporary table you’d like to empty sooner rather than later.

The table is only deleted when the connection to the application server closes.
