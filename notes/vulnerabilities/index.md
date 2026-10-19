---
title: Vulnerabilities in 1C Configurations
description: I put together a simple Bash script that searches a configuration export for common vulnerabilities.
created: 2020-10-13 17:35:51
tags:
- 1c
- bash
- done
---

Last month, I attended an Infostart [meetup](https://infostart.ru/events/1269292/) about security in 1C-based solutions. I learned a lot! Among other things, Oleg Tymko talked about things we do in code that can leave a product vulnerable: hard-coded IP addresses, links, email addresses, passwords, that sort of thing.

Yesterday, I put together a simple Bash [script](https://github.com/vkostyanetsky/ScriptsForLogAnalysis/blob/master/Файлы%20конфигурации/Vulnerabilities.sh) that searches a configuration export for such issues. Essentially, it just runs regular expressions over the configuration. I didn’t put too much thought into it, just tried a couple of approaches for the fun of it.

My takeaway? If you’re going to keep developing this seriously, start with something like SonarQube. Brute-force approaches produce too many false positives; you need filters, exceptions, history, and supporting infrastructure.

For a quick look, though, this does the job: no infrastructure, no fussing with configuration or hardware. Export the configuration, kick Bash into action, and that’s it: results in a few seconds.
