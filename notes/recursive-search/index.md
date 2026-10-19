---
title: Searching Files Recursively
description: Some time ago, I tried finding code vulnerabilities with Bash scripts and made the solution too complicated. There’s an easier way!
created: 2021-01-14 10:15:16
tags:
- 1c
- bash
---

Some time ago, I [tried](/notes/vulnerabilities/) finding code vulnerabilities with Bash scripts. Sounds intimidating, but it’s just recursive text searching with regular expressions. I wrote the scripts, but as I realized today, I did it in a rather… ass-backwards way, ahem. All you need is egrep! Out of find, xargs, and egrep, we can throw away two of the three.

For example, today our configuration stopped building in the latest EDT release. We suspected broken GUIDs: references to metadata objects removed from the configuration. The platform doesn’t always clean these up after deleting the objects themselves. I’ve written about this a couple of times, for example [here](/notes/designer-error-investigation/) and [here](/notes/unresolved-gopher/).

We decided to search the configuration export for GUIDs and check whether they all belonged where they were. All it takes is one command:

    egrep -rn '.{8}-.{4}-.{4}-.{4}-.{12}' dump

The r flag enables recursive search; n adds not only the filename but also the line number to each match. The final parameter, dump, is the directory to search.

You could make the regex more precise, but this is plenty. False positives, GUIDs that aren’t broken references, are easy to filter through a pipe. For example, the following script excludes GUID-containing lines that include “uuid”:

    egrep -rn '.{8}-.{4}-.{4}-.{4}-.{12}' dump | grep -v 'uuid'
