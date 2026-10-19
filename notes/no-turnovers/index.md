---
title: A Turnover Register without Turnovers
description: What happens if you disable “Use in totals” for every dimension of a turnover register?
created: 2019-10-15 17:10:04
tags:
- 1c
---

If you disable “Use in totals” for every dimension of a turnover register, its turnovers will no longer be calculated. In fact, the turnover table disappears from the database altogether. Try to query it in 1C, and you’ll get an error.

That can come in handy when all we do is collect data, then filter it by recorder or other fields. We only need the register’s record table. There’s no point spending server resources calculating and storing a turnover table.

An information register naturally comes to mind for this task. But its efficiency depends on how many dimensions you plan to create: every index on an information register includes all its dimensions, plus the period for periodic registers. The platform creates a base dimension index, an index for each dimension marked “Master,” and an index for each dimension, resource, or attribute you choose to index separately. And we haven’t even reached the slice tables, which use the same approach!

So if your information register isn’t thought through all that well and has quite a few dimensions, say more than two or three, keeping all those indexes up to date can get expensive. A “flat” turnover register without turnovers starts to look pretty good.
