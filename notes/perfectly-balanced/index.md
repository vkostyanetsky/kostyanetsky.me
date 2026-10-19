---
title: On Perfect Balance
description: I want to share a couple of great pieces about modern software. They aren’t especially new, but someone has surely missed them.
created: 2020-05-20 06:48:19
tags:
- 1c
- work
---

I want to share a couple of great pieces about modern software. Neither is exactly new, especially the first, but I’m sure some of you haven’t seen them:

- [Software Disenchantment](https://tonsky.me/blog/disenchantment/ru/) by Nikita Tonsky
- [Everything in Software Is Amazing, but Nobody Is Happy](https://habr.com/ru/company/jugru/blog/493178/) by Evgeny Trifonov

What brought this to mind? Last week, I took part in a hackathon for developers at our company. I learned a lot of cool things and even shared first place with two other people who came up with very similar solutions.

(If you also work at First BIT, the [results](https://newportal.1cbit.ru/news/3066979/) are on the portal, along with a [review](https://newportal.1cbit.ru/news/3066980/) by another winner, Dima Leshchenko.)

Along the way, we had to deploy and configure a mountain of software: EDT, RabbitMQ, Docker, GitLab, JIRA, SonarQube, and a wagonload of smaller, simpler tools. All right, no complaints about RabbitMQ: it’s light and fast. But the rest… I already knew EDT had an appetite, but GitLab and JIRA’s hunger really surprised me.

Yes, everything ran in Docker; yes, the configuration wasn’t optimal, with several PostgreSQL servers instead of one, for example; yes, it was Docker for Windows, which every sysadmin I know loves to make jokes about. But twelve gigabytes of RAM straight out of the gate?! I won’t even mention the CPU: the load looked as if the computer were simulating a nuclear explosion in real time.

It’s hard to find a better illustration of Nikita’s points: this software devoured a sea of resources and calmly asked for more before it had even started doing anything useful. But Evgeny has a point too: with one single command, I got several operating systems up and running, complete with some pretty complicated servers. And it took a tiny fraction of the time it would have a few years ago.

I don’t have an answer. I don’t think anyone does. We’ve been having this argument forever. Pick either side, and you still end up trying to find a balance. You need to write good, fast code, while also avoiding premature optimization. Getting the code right feels great: you remember that, damn it, you’re pretty good! The second simply keeps you from losing your mind in search of the perfect bubble sort algorithm :-)
