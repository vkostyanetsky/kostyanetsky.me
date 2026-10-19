---
title: Counting Exceptions in the Technological Log
description: 'Another script: it counts exceptions per minute and ranks the periods with the most exceptions.'
created: 2020-10-24 12:00:12
tags:
- 1c
- bash
- done
---

Another [script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический%20журнал/ExceptionsNumberByMinutes.sh). It counts exceptions per minute and ranks the results, so you can quickly see when the applications were having a particularly bad time.

Along the way, two odd little problems threw me off. First, for some reason I was convinced that uniq -c groups lines regardless of where they occur in the data stream. Take this:

    банан
    банан
    груша
    банан

I thought that feeding this data to uniq -c would group identical lines, count repetitions, and produce something like this:

    3 банан
    1 груша

But what I actually got was:

    2 банан
    1 груша
    1 банан

Turns out uniq expects repeated lines to sit next to each other. If a line differs from the previous one, it starts a new count for that line. To get the result I expected, you have to sort the data first, then pass it to uniq.

The second problem involved sed. I was trying to use it to remove everything from the data stream except hours and minutes; the attempt is on line 12 of the script. But some events stubbornly failed to match the regular expression despite looking exactly the same. I wasted a lot of time and got thoroughly annoyed before remembering [BOM](https://ru.wikipedia.org/wiki/Маркер_последовательности_байтов). Once I removed the markers, everything went smoothly.

Every technological log file uses a BOM, which you can confirm with this [script](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический%20журнал/LogFilesWithoutBOM.sh), for example. Each log begins with special characters invisible to the naked eye, and these can prevent the first line from being processed because they don’t match the regular expression.

My takeaway is to strip BOMs out by default, rather than wonder every time whether they’ll cause trouble. Sometimes this will be unnecessary; a BOM doesn’t interfere with solving the task above using grep, for instance. But I don’t like surprises. Besides, a few processor cycles spent on a simple replacement are a much better deal than the equivalent in fried nerves and wasted time.
