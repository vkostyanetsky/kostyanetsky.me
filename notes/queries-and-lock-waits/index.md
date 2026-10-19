---
title: Queries and Lock Waits
description: 'Two more technological log analysis scripts: one ranks MS SQL queries, the other ranks lock waits.'
created: 2020-10-19 06:23:06
tags:
- 1c
- bash
- done
---

I’ve put together two more technological log analysis scripts: the [first](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический%20журнал/LongestQueries.sh) ranks heavy MS SQL queries, and the [second](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический%20журнал/LongestLockWaitsByRegions.sh) ranks long lock waits.

I pick out heavy queries by how long their DBMSSQL events lasted. The longer a query takes, the more likely it is to have eaten a lot of resources. That usually holds up, though you’d need a trace to diagnose it properly.

Lock waits are also measured by duration. The script also checks that the TLOCK event has something in WaitConnections. That tells us the platform really was waiting to get a lock, rather than just taking a while to set one.
