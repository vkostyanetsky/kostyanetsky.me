---
title: An Updated Top Exceptions Script
description: I rewrote the Bash script that ranks exceptions in a collected technological log, looking for a simpler solution.
created: 2020-10-17 13:25:02
tags:
- 1c
- bash
- done
---

I rewrote the Bash [script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический%20журнал/FrequentExceptions.sh) that ranks exceptions in a collected technological log: I wanted a simpler way to do it.

Out went all the fiddling with replacing event beginnings with a gawk record separator: you can just give gawk a regex for that. I also moved more of the logic into gawk itself. It’s easier to follow, especially if I want to add to it later.

It’s clearly better than [before](https://gist.github.com/vkostyanetsky/47e02e199bb32082c4d5019ca88233c0), or at least the logic is easier to understand. Originally, though, I wanted to make the script smaller and get something like this:

    grep -hoP ",EXCP,.*\KDescr=.*" */*.log | uniq -c | sort -rn

Filter only lines with EXCP events, cut everything before the error description, and group with uniq. Very elegant, I think.

Except EXCP descriptions can run over several lines. So sometimes we’d lose information we need to figure out what happened: everything after the first line break. I haven’t yet worked out how to solve that without tripling the script’s size :-)
