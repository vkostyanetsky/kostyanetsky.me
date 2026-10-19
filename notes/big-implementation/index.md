---
title: A Large 1C Implementation
description: What counts as a large 1C implementation?
created: 2019-10-15 17:56:15
tags:
- 1c
---

What counts as a large 1C implementation? In Russia, the platform is used almost everywhere, even in companies that use a lot of competing software, like SAP. Of course, some implementations are small, with 1C handling purely practical tasks like generating reports. But the Corporate Technology Support Center’s work includes truly enormous projects, such as optimizing the [Delovye Linii](http://v8.1c.ru/expert/cts/cts-218-001.htm) system for five thousand concurrent users.

So in Russia these days, hundreds of people working in the same infobase at once is a medium-sized implementation. A thousand or more is a large one.

The size of the database doesn’t tell you much on its own: many configurations don’t involve large numbers of concurrent users. A classic example is 1C:Document Management; a less classic one is Avibus’s Bus Terminal Management. Bus Terminal Management can collect huge amounts of data in its central database from remote nodes in its distributed network of bus terminals and stations, without having many users working simultaneously.
