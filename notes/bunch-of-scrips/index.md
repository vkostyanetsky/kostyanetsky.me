---
title: A Bunch of Scripts
description: A few more scripts for analyzing the platform’s technological log.
created: 2021-04-24 10:00:00
tags:
- 1c
- bash
- done
---

I’ve put a few more scripts on GitHub that I wrote recently to dig through the platform’s technological log:

- [Frequent Events](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/FrequentEvents.sh). Groups events by name, counts occurrences, and lists them in descending order, from most to least frequent. Not sure this one is much use in practice; I just wanted to note down which cluster process writes which events.
- [Exception Descriptions](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/ExceptionDescriptions.sh). Groups EXCP events by the Name field; for each name, it lists the Description values found in exceptions with that name. This script gives you a rough picture of which exceptions are real problems and which are just background noise you can ignore and, for example, filter out in Kibana.
- [Locks by Connection ID and Region](https://github.com/vkostyanetsky/ScriptsFor1C/blob/master/Технологический журнал/LocksByConnectIDAndRegions.sh). Useful for finding the culprit behind a managed lock timeout: it extracts all TLOCK events for a particular region and connection from the technological log, then puts them in chronological order.
