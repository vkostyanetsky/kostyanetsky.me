---
title: Viewing a Value Table
description: I’ve uploaded a short function to GitHub that converts a value table into a spreadsheet document.
created: 2020-05-16 21:19:05
tags:
- 1c
- done
---

I’ve uploaded a [short function](https://gist.github.com/vkostyanetsky/ef44bbb18f36a1c51826a5d0b4f3960e) to GitHub that converts any value table into a spreadsheet document.

Why? So you can see what’s in it. You can’t pass the value table itself to the client, but you can pass a spreadsheet document based on it. A spreadsheet document also has a client-side Show() method that displays it in a separate window.

That way, you can poke around in utility tables right in Enterprise mode, without building an interface for each one. Feed a table to the function above, get a spreadsheet document back, send it to the client, and show it.
