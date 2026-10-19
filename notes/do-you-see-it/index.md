---
title: Checking an Object’s Visibility
description: A function for checking whether an object is visible to a user.
created: 2021-01-17 12:34:45
tags:
- 1c
---

The platform has a few different ways to decide whether a user can see a metadata object. One is functional options. If you haven’t used them before, here’s the short version: an object can be included in an option that is either active in a particular infobase, making it visible to users, or inactive, hiding it. An object can belong to several options, in which case it appears if at least one is active, or to none, in which case users always see it.

I remember looking, about five years ago, for a way to tell from code whether a user could see an object, just as far as functional options are concerned. For some reason, I didn’t find a solution then, although it’s ridiculously simple. Here’s a [function](https://gist.github.com/vkostyanetsky/a6c0525c38a8231f37a676ec240d657a) for exactly that. The logic:

1. Iterate through the options and look for the object in each one.
2. If the object belongs to an option, check whether that option is active. If so, the user can see the object.
3. If the object belongs to no options, the user can see it too.

You’d want to cache the result, of course, if only because there’s a query inside a loop. Apart from that, it does the job. I still can’t understand why I didn’t write something similar back then. Man, I hope that five years from now, the things driving me nuts today will look just as easy to fix.
