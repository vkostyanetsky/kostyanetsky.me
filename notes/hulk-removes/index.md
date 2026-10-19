---
title: Hulk Delete!
description: 'A funny detail: PostgreSQL 10 renamed pg_xlog to pg_wal and pg_clog to pg_xact.'
created: 2020-11-09 16:03:07
tags:
- pgsql
---

On a PostgreSQL [course](https://postgrespro.ru/education/courses/DBA1), I learned a funny detail: in version 10, the DBMS developers renamed pg_xlog, containing write-ahead logs, to pg_wal, and pg_clog, containing transaction statuses, to pg_xact.

Know why? Because of admins with more confidence than experience, who saw “log” in a folder name and went for it. I need to free some disk space, they’d think, and this DBMS has filled it all with stupid logs. No time to investigate: rm -rf the lot, problem solved!

So out went both the protection for data operations in the buffer cache and multiversion concurrency control, in one fell swoop. The cluster then died in agony. There was plenty of free disk space, but I’m afraid the joy didn’t last long :-)
