---
title: Data History Settings
description: A data processor for configuring the data history mechanism recently introduced in the platform.
created: 2020-03-09 12:02:26
tags:
- 1c
- done
- work
---

I’ve put a [data processor](https://github.com/vkostyanetsky/AuditLogSettings) on GitHub for setting up the data history feature that [arrived](https://its.1c.ru/db/v8312doc#bookmark:dev:TI000001938) in the platform not long ago. Here’s what it does:

1. Builds a tree of objects that support data history;
2. Shows which objects currently have data history enabled;
3. Lets you enable or disable data history for objects and their attributes.

It supports batch operations: for example, you can enable history for several documents and all catalogs at once. Or wipe all your changes and go back to the defaults the configuration developer set.

I think it turned out well. I mostly wrote it for myself, to give my brain a workout in the evenings, but we might even include it in [our configuration](https://firstbit.ae)! Right now, SSLi handles our data versioning, and it’s not exactly efficient: it’s slow and makes the database balloon. Hopefully, the platform will do better when we test it.

You can find similar tools on Infostart and ITS, but they’re either buggy, work only with Russian configurations (no English interface and dependencies on the Russian Standard Subsystems Library), or are just out of date, with no support for constants, for example.
