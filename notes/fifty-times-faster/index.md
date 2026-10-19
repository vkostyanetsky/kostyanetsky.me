---
title: Fifty Times Faster
description: A client’s month-end closing document took almost an hour to post, most of it spent on one procedure.
created: 2019-12-05 19:56:49
tags:
- 1c
- done
- optimization
- work
---

Here’s the brief: a client’s month-end closing document takes just under an hour to post. The database isn’t especially big, but an hour is still way too long. Sure, you don’t close the month very often, and you could do it at night. Still.

I dig into the code, profile it, and see that the platform spends almost all that time in a single procedure, executing a query batch. I look at the [first query](https://gist.github.com/vkostyanetsky/cf34c6cb088515e2bab6485b2f35eab8) and think: classic, accessing data through a dot on a composite type. The DBMS must be attaching a wagonload of joins to huge document tables, and the optimizer can’t come up with a decent plan in time.

I test the theory. But SalesDocument includes only eight document types. That should be manageable: the usual rule of thumb is that the optimizer can find a decent plan with up to eight joins.

I check the document tables: they aren’t particularly big. I run the query separately from the batch. It isn’t instantaneous, reading around 350,000 records and selecting about 200,000, but it certainly doesn’t take an hour.

All right, the batch’s first query is heavy, but it’s not the problem. I look at the [second](https://gist.github.com/vkostyanetsky/f4423dcfb28d4a27039c7310c13c3a73) and realize that we’ve just read the entire Inventory register and selected most of its records, and now we’re reading it again and joining both result sets on a pile of conditions. The register’s record table has no suitable index: only the standard recorder index, which naturally doesn’t help.

I check: this is exactly where the platform spends most of its time waiting for the DBMS.

I couldn’t find a way around reading the whole register table twice. Every alternative I tried meant changing how the data was stored, and that wasn’t an option. So I went with a compromise: in the second query, I replaced the join to the actual register record table with a join to a temporary table created in advance and indexed on the join fields.

This was a case where 1C’s general recommendation worked perfectly: posting time fell from 50 minutes to three, and after further code optimization, to one minute. Fifty times faster than where I started.

A minute for month-end closing is usually fine, so I called it a day.
