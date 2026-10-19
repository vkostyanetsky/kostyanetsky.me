---
title: Service Launcher
description: I wrote a PowerShell script that starts and stops selected Microsoft Windows services.
created: 2020-07-13 20:15:07
tags:
- done
- powershell
- workplace
---

Some time ago, I [wrote](/notes/manage-services-with-powershell/) about PowerShell cmdlets for starting and stopping services. I’ve now turned those sketches into a proper script: it reads the list of services to start or stop from a separate file, and checks whether it has admin rights, asking for them if it doesn’t.

You can [see the result](https://github.com/vkostyanetsky/ServiceLauncher) on GitHub. No big deal: I wanted to practice the language and simplify my daily routine. My system has about fifteen resource-hungry services needed for work but useless at other times. Stopping and starting that whole zoo by hand is a pain. Doing it with one command? Much better!
