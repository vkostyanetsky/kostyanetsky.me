---
title: What Is the Performance Management Center For?
description: The main thing this tool provides is a set of analytical indicators.
created: 2019-11-24 15:46:53
tags:
- 1c
---

You use the Performance Management Center to:

1. Find a system bottleneck, such as slow code or a heavy query;
2. Investigate specific locks or deadlocks.

For the first one, turn on query analysis and, say, server call analysis. The results show which parts of the system load the application server and which load the DBMS server. This is often done without a specific problem, simply to remove bottlenecks and give the system more headroom for the future.

For the second, turn on lock or deadlock analysis when you know they occur and want a report: who blocked whom, when, on which line of code, and so forth.

In both cases, monitoring runs only during peak load, for ten to fifteen minutes. That keeps the pile of raw data manageable. PMC has to chew through it before it can show you anything, and the more you collect, the longer that takes. You also avoid picking up unrelated stuff, like load from overnight maintenance jobs.

It also keeps the collection itself from slowing things down too much. PMC can collect a monster of a technological log, and your hardware will feel it. Thirty percent slower? Hold my beer. Be careful with that.

PMC’s analytical indicators are sufficient for all of the above: query analysis, lock waits, and 1C and MS SQL deadlock analysis. Operational indicators, such as the number of queries running and their total execution time, are less popular. They usually aren’t worth the time it takes to set them up, and PMC was never really designed to collect them continuously.
