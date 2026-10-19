---
title: Debugging over HTTP
description: How do you enable HTTP debugging on a 1C server?
created: 2020-01-26 12:32:49
tags:
- 1c
---

At the end of the week, I ran into a rather silly situation: TCP/IP debugging on a 1C server stopped working. Debug mode itself was enabled, but Designer couldn’t “see” the server’s debug contexts.

The silly part is how hard it is to find the cause: the platform gives you no clear error to work with. After checking the obvious possibilities, such as the firewall and ports, all you can do is shrug.

I solved it by switching to HTTP debugging. The overall [setup](https://its.1c.ru/db/v8310doc#bookmark:dev:TI000001599) is more complicated, but configuring it is quite simple. Briefly, you need to:

- Start the server agent service with the “-debug -http” flags.
- In Designer’s debugging settings (Tools → Options → Debug), select HTTP as the debugging protocol and the cluster debug server as the debug server.

There’s a more detailed [walkthrough](https://its.1c.ru/db/v8310doc#bookmark:dev:TI000001604) on ITS.
