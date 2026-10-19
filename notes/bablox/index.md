---
title: What Is Bablox?
description: I watched Evgeny Filippov’s webinar about Bablox.
created: 2019-10-20 12:22:43
tags:
- 1c
---

I watched Evgeny Filippov’s [webinar](https://www.youtube.com/watch?v=p6D7SdhO4LI) about Bablox. Basically, it’s another way to measure performance. Like Apdex, it relies on timing operations, but uses a different calculation.

To calculate Bablox, multiply an operation’s duration by the salary rate of the user performing it. The salary can be:

- An average value, if you want a rough performance comparison of the same application across several companies.
- The user’s actual salary, if you need to convince management to spend some money on making things faster.

You end up with a figure for how much money the business has lost to a slow application. That’s the big selling point: it’s a number the average CFO can get their head around. Explaining what an Apdex of 0.5 means takes some effort; explaining a loss of 300,000 rubles doesn’t.

It also takes the guesswork out of questions that come up in every implementation. Take deciding which operations matter most: which matters more, selling goods faster or receiving them into the warehouse? Once you know how much money the company loses on each operation, the answer is much easier.

Of course, it’s not a perfect model. Suppose the application is slow for warehouse workers but runs fine for accountants. Warehouse workers earn less, so their Bablox will be low. But accountants depend on what the warehouse workers enter. Until that’s done, they have nothing to work with. The company is losing money because the application is slow for warehouse workers, but Bablox won’t show it.

I doubt Bablox will knock Apdex off its perch in the IT world, but it’s an interesting idea, and it could make talking to clients a whole lot easier.
