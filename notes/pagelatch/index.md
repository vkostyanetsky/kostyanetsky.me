---
title: PageLatch Waits
description: PageLatch waits are hard to notice until you have several thousand users.
created: 2019-04-22 20:00:30
tags:
- 1c
- mssql
---

PageIOLatch waits, which I [wrote about](/notes/pageiolatch) today, are easy to encounter in any system, even small ones. PageLatch waits are the opposite: they’re hard to notice until you have several thousand users.

They occur when a page is already in RAM and several users try to modify it. Until one user finishes, the others must wait. The wait is usually tiny, but with many users competing for the page, it becomes noticeable.

Two things can cause this.

## An Index Hotspot

This happens when we try to write heavily to the last page of an index, primarily one with a monotonically increasing key. Examples include indexes on reference-type fields: since late 8.2 versions, the platform has generated sequential GUIDs, reducing disk fragmentation and making the index key monotonically increasing.

What is an index with a monotonically increasing key? In 1C, register indexes on the period are one example. They don’t grow perfectly monotonically, but the important point is that we generally write data at the end. Document number and catalog code indexes also belong here: a new number is assigned and written to the end of the index.

Fixing a hotspot means changing the design: you need writes to land in different parts of the index, not all at the end.

## tempdb System Pages

Creating or dropping a table updates a number of internal pages: IAM (Index Allocation Map), PFS (Page Free Space), GAM (Global Allocation Map), SGAM (Shared Global Allocation Map), and others.

Why does this matter for tempdb? The 1C platform regularly creates an **enormous** number of tables there. Those internal pages get updated so often that you start seeing PageLatch waits.

MS SQL Server can optimize system page updates for tempdb so they happen less often, but sometimes even that isn’t enough, especially when tables are created with indexes, a very common case in 1C applications.

There are a few ways to tackle it. First, for DBMS versions before 2016, disable mixed extents using trace flag 1118. That removes the need for two internal page types, GAM and SGAM, so you no longer have to wait for their updates. An extent is eight data pages, or 64 kilobytes. If it contains pages from one table, it is uniform; if from several, mixed.

Second, split tempdb into multiple files. Since 2016, the DBMS splits it into eight files by default. Each file has its own internal pages, so spreading the work across files cuts the waits.

Third, reduce the number of temporary tables to lower the load on tempdb. Rewrite the most frequent queries so they run acceptably fast without temporary tables.
