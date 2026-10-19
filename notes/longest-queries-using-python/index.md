---
title: Finding Long Queries with Python
description: I’ve published a Python script for finding long queries in the 1C technological log.
created: 2021-07-18 17:23:55
tags:
- 1c
- bash
- python
---

I’ve published a [Python script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/LongestQueries.py) for finding long-running queries in the 1C technological log. I wrote it in a fit of despair: I couldn’t understand why my [trusty Bash script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/LongestQueries.sh) reported an average execution time higher than the maximum for one query.

The problem turned out to be gawk. With some log events, it couldn’t work out the duration: it tried to convert a string to a number, failed, and… Oh, no! Of course it didn’t throw an exception! It simply treated those strings as zero and calmly carried on.

The fix was even sillier than the problem: I simply [added](https://github.com/vkostyanetsky/ScriptsFor1C/commit/946f271d6ea0c1e618b180cee11ae91ce4632c18) an explicit string-to-number conversion, and everything worked. How the hell is an explicit conversion different from an implicit one in gawk-land? And, more importantly, why?

Well, that was a circus. At least I got a new script out of it, and it goes easier on memory and CPU too. In the Bash version, I used three collections to calculate execution count, total execution time, and maximum execution time, each keyed by query text and context. All three had to be updated and kept in memory.

That works until you get into hundreds of thousands of entries. Then the collections start eating gigabytes of RAM, and searching them burns through CPU time. The new script is more straightforward: one collection stores all the data for each query.
