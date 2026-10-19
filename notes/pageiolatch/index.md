---
title: PageIOLatch Waits
description: A wait caused by a page being latched while it moves into the buffer cache.
created: 2019-04-22 19:40:08
tags:
- mssql
---

When a data page is read from disk into the buffer cache, it is latched for a very short time: it cannot be written, and in some cases cannot be read either. Waiting for that action to finish is a PageIOLatch wait.

Maybe you don’t have enough RAM. Or maybe some lousy queries are churning through the buffer cache, reading millions of rows to return three. The DBMS repeatedly fails to find the required pages in cache and has to read them from disk again and again.

So if you’re seeing a lot of PageIOLatch waits, faster disks, more RAM, and better queries will probably help.
