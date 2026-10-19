---
title: Uncompiled Code
description: 'A non-obvious detail: an external data processor runs slower than an identical one built into a configuration or extension.'
created: 2019-01-27 10:09:59
tags:
- 1c
---

Here’s something you might not expect: an external data processor runs noticeably slower than the exact same processor built into a configuration or extension. Its code has to be compiled again for every user, every time they run it. Built-in processors get compiled once for everyone. According to an [article](https://kb.1c.ru/articleView.jsp?id=111) in 1C’s knowledge base, this was outright catastrophic in platform 8.3.10. But a fifty-percent increase in CPU load in 8.3.13 isn’t exactly good news either.

Storing code inside the infobase does the same thing, for example if you keep it in utility catalog attributes. Every time you run it, the platform fetches it from the database and compiles it. In [our configuration](https://firstbit.ae/products/erp-dubai/financial_management/), for example, scheduled jobs can be created without changing metadata: add an item to a special catalog, load the code into it, and set its schedule.

That’s more or less fine: scheduled jobs don’t usually run all that often, so compiling them probably won’t make a noticeable dent in the CPU. But the same catalog can also be used to run code automatically after pretty much any event, say saving a certain kind of document. If lots of users will be running that code all the time, move the logic into a common module method in an extension and leave only a call to that logic in the stored code.

Or you could ditch stored code altogether and put the whole thing in an extension. It might be less flexible, but that’s still better than staring at a low APDEX later.
