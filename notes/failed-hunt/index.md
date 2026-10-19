---
title: An Unsuccessful Hunt
description: After upgrading the platform to 8.13.12.1790, we encountered a bug when switching a control’s current page on the server.
created: 2019-02-21 16:42:32
tags:
- 1c
- done
- work
---

After upgrading the platform to 8.13.12.1790, we encountered an intermittent bug when switching a control’s current page on the server. It works on the server itself, but as soon as control returns to the client, the current page reverts to the previous one. The current-page-change event also fires, as though the user had changed it manually.

Unpleasant, but nothing terrible: we worked around the bug, documented it, and reported it to 1C. They’re still [thinking about it](https://bugboard.v8.1c.ru/error/000050675.html). But weirdly, I’m a little disappointed. When you dig into these things, there’s always a bit of mystery. It feels like you’re about to uncover something interesting, some new quirk of the platform, maybe.

Then you finally look at the problem from the right angle, and it’s just an ordinary bug. Maybe striped or spotted, but a perfectly ordinary bug, sitting there twitching its antennae. And you think: damn, is that all I caught? :-)
