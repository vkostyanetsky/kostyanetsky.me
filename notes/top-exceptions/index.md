---
title: Top Exceptions in the Technological Log
description: A script that identifies the most frequent exceptions in a collected 1C technological log.
created: 2020-02-16 16:32:02
tags:
- 1c
- bash
- done
---

I’ve published a Bash [script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический%20журнал/FrequentExceptions.sh) on GitHub that identifies the most frequent exceptions in a collected 1C technological log.

It analyzes EXCP events. These are the main source of error information, though certainly not the only one: EXCPCNTX, for example, describes an event that had started but hadn’t finished when an error occurred. Also, if an exception occurs during a server call, the CALL event’s RetExcp field contains the error text returned to the client.

Most of the time, though, EXCP and the occasional EXCPCNTX will do. People often collect them by default: it doesn’t put any real load on the system and lets you run the logs through an analysis tool, such as the script above, to keep a finger on the pulse.
