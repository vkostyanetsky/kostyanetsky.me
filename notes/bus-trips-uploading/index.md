---
title: Exporting Bus Schedules
description: Since my colleagues and I rebuilt KPAT’s automation, it has been running on Bus Terminal Management.
created: 2017-11-17 16:49:35
tags:
- 1c
- done
- work
---

Ever since my colleagues and I redid the automation at [KPAT](http://kpat.ru/), it has been running on [Bus Terminal Management](http://avibus.pro/standalone/). It’s a 1C:Enterprise configuration, so hooking it up to pretty much anything is fairly quick.

First up, we wanted to send the bus schedules to E-Traffic and Yandex Schedules.

## What Are These Services?

The first, [E-Traffic](http://e-traffic.ru), is Russia’s largest aggregator of bus terminal data. It collects information about scheduled departures and acts as an electronic ticket agent. Passengers don’t have to fight their way through individual bus terminal websites: E-Traffic lets them buy a ticket from anywhere to anywhere.

As for [Yandex Schedules](http://rasp.yandex.ru), it’s one of the Russian search engine’s side projects. Like E-Traffic, it’s mainly for passengers, and it can quickly put together fairly complicated routes with transfers. It doesn’t sell tickets, though.

## How Did We Build the Export?

At first, we built a fairly elaborate setup to send schedules to both systems. We worked our asses off, until a conversation revealed that E-Traffic already knew how to pass schedules on to Yandex. Just like that, half the job disappeared.

On the technical side, we made a web service for Bus Terminal Management. E-Traffic sent a SOAP request, the service gathered the bus schedule data from the 1C:Enterprise infobase, and back came some XML.

We didn’t even have to write code to fetch schedules from every terminal in the KPAT network: all schedules are created and stored in one place, the main node of the distributed infobase, and that’s where we deployed the web service. In the end, coordination took longer than programming.

## Where Can You See It?

The inner workings aren’t visible from outside, and there’s nothing unusual there anyway: robots talking to robots, big deal :-) The result of their chatter is much more interesting! For example, here’s today’s [schedule](https://rasp.yandex.ru/search/bus/?fromName=Кемерово&toName=Новокузнецк&fromId=c64&toId=c237&when=сегодня) from Kemerovo to Novokuznetsk.
