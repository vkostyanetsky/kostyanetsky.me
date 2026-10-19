---
title: A Slow Notification
description: An optimization case where the cause of slowness turned up somewhere unexpected.
created: 2021-08-18 20:03:30
tags:
- 1c
- work
- optimization
---

[![Tweet](snap-tweet-iamhellcat-1427626118635470866.png)](https://twitter.com/iamhellcat/status/1427626118635470866)

This tweet reminded me of a job I once worked on. A client gets in touch: 1C is slow, do something. We ask a few questions. Okay, not all of 1C, just one operation.

They’re running a heavily rewritten Trade Management configuration, and the slow bit is custom code. Inside, it’s queries nested all over the place, a lot of messing about with value tables, some dubious calculations, and a bunch of server procedures seven hundred lines long each. We can’t copy the infobase, the technological log is unavailable, and DBMS and performance counter access are blocked. Debugging on the 1C server is off, and enabling it isn’t an option.

A mess. How can we find where it bogs down? Well, we can change the configuration; that’s something. I time the operation: yes, it thinks about something for a couple of minutes. Excellent! Rubbing my hands, I narrow the search by adding a whole pile of measurements to the most suspicious sections.

And here’s the funny part: nothing’s slow! The code doesn’t exactly fly, but we’re nowhere near minutes. For something this complicated, the timings look fine. Could it be serialization between client and server? I go through the server procedure carefully. Nope, nothing’s being passed back. Everything on the server stays there.

I move to the client code. What could possibly be slow here? Simplified, it looked something like this:

https://gist.github.com/vkostyanetsky/df0b17526991bbac3b3df1c75dfa154f

I stared at that code like an idiot for about ten minutes. Eventually, I asked a colleague, who told me something surprising: Notify() doesn’t just send notifications to every open form, it **waits** for their notification handlers to finish.

That blew my mind. I went digging and eventually found a form that caught the operation-completed event and immediately refreshed its dynamic lists. A query in one of those lists caused the delay: a subquery inside a subquery inside a subquery, plus a pile of joins “through a dot.” Nothing interesting after that.

Well, there you have it :-) Notify with care!
