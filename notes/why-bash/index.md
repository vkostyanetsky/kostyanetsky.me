---
title: Why Bash?
description: A few words on why Bash is convenient for analyzing the 1C technological log.
created: 2021-06-23 22:24:48
tags:
- 1c
- bash
- python
---

I occasionally see colleagues asking: is 1C still messing around with Bash? Why not Python? Or PowerShell, at least? Weirdos!

Yes, Bash isn’t native to Windows, which is native territory for 1C, and getting it onto Windows is a job in itself. Yes, as a task grows more complicated, script readability drops exponentially. Yes, Bash simply can’t handle some tasks.

But its main advantage is that, for many 1C technological log analysis tasks, it is the simplest and fastest solution. Like an old screwdriver your grandfather used: always at hand, scratched, nicked, and a little awkward, but still working perfectly.

An example? Suppose we need to extract exceptions from the log. Let’s keep it simple and say we only need the first line of each event. We get [something like this](https://gist.github.com/vkostyanetsky/f95b30e2f09edd64d9e7a39575bb289a) in Python.

You’ll say it can easily be cut in half, or even to a third. After all, it’s just a one-off job. Why fuss about readability and predictability?

Agreed! But let’s first solve the same task in Bash:

    grep -r --include "*.log" ',EXCP,' > result.txt
That’s why Bash.
