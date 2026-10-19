---
title: An Updated Script for Finding Long Queries
description: The first version had a serious flaw, now fixed.
created: 2021-04-26 19:24:00
tags:
- 1c
- bash
- done
---

I’ve updated the [script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/LongestQueries.sh) for finding long queries in the platform’s technological log. The old version had a pretty big bug: it happily grouped DBMSSQL events by Sql and Content, but ignored cases where Sql was missing. That isn’t exactly normal for DBMSSQL, but it happens.

While I was at it, I tidied the code: added formatting and comments, and moved some gawk logic out of the main body into functions. Yes, the script now looks longer than it actually is, but it’s much easier to read.

Honestly, I don’t understand why Bash veterans cram all their logic into two or three lines. The language isn’t exactly a shining example of readability to begin with; Nikita Prokopov’s Telegram channel has an [excellent piece](https://t.me/nikitonsky_pub/55) about that. A page of pipelines without a single line break makes me want to quietly climb out of the window.
