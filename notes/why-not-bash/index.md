---
title: Why Not Bash?
description: A few words on why Bash can be inconvenient.
created: 2021-07-03 21:31:48
tags:
- 1c
- bash
- python
---

In my previous note, I discussed tasks where Bash is the perfect tool: minimal constraints and simple requirements. I also mentioned that sometimes it can’t cope at all.

Here’s one I actually ran into. Suppose we have around seventy gigabytes of 1C technological logs and need to rank exception descriptions from most to least frequent.

Easy, right? Extract EXCP, get Descr, count repetitions. I’d even written the necessary [script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/FrequentExceptions.sh) before. I run it and patiently wait…

Know how long it took? Neither do I: after a day, I stopped it and investigated. Something was getting stuck inside gawk: it was hammering the CPU, with cat occasionally chipping in to read more data.

![Processes](procexp64.png)

Well, crap. I thought about it and [rewrote](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/FrequentExceptions.py) the script in Python. The new version finished in fifteen minutes and gave me:

1. A ranking of exception descriptions;
2. A script I can read six months later without Google;
3. Confidence that I can add two or three conditions without accidentally summoning Satan through a combination of operators and flags.

Of course, I could have optimized the Bash version. The array lookups are probably what’s slow: the more descriptions you’ve collected, the longer each search takes, growing linearly with the array size. I could change the collection approach, using gawk only to extract descriptions and passing the results to sort and uniq.

But that starts to resemble the [meme](but-why.jpg) about a loaf of bread: you can build a trolleybus out of it, sure, but why? Come on, I just need to solve the problem. Python already did that with flying colors, saving me a lot of time and nerves.

That’s my point. We know Bash has its quirks, and often we can live with them. We know what it’s good at too. But sometimes the question becomes: is getting a solution specifically in Bash worth spending two hours wrestling with utilities, parameters, manuals, and Stack Overflow posts?

[![Tweet](snap-tweet-EffinBirds-1408560771877965825.png)](https://twitter.com/EffinBirds/status/1408560771877965825)

That’s why not Bash.

UPDATE: A good [post](https://grishaev.me/no-bash/) by Ivan Grishaev on the same subject.
