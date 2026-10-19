---
title: A Large Table on the Client
description: Colleagues suggested an interesting way to optimize relatively large data collections on the client.
created: 2019-05-19 16:54:12
tags:
- 1c
---

I’ve already written a couple of times about the quirks of working with huge tables on the client ([one](/notes/find-rows-at-client), [two](/notes/big-value-tables)), but late last week my colleagues suggested another interesting way to optimize relatively large data collections on the client. The short version: the [standard](https://its.1c.ru/db/v8std#content:628:hdoc) recommends working with large collections only on the server. However, if you iterate through the entire collection on the client at the start, for example in the form’s opening handler, it gets cached. After that, you can loop through it, search for rows, and do other work without extra server calls. Overall, you spend less time shuffling data around.

I’ve just run some simple tests, and yes, it seems to work. The method feels a bit like a workaround, and I haven’t tested it under serious load yet, but it’s probably worth keeping in mind.
