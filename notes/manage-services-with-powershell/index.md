---
title: Managing Services with PowerShell
description: Examples of PowerShell scripts for starting and stopping services.
created: 2020-04-28 17:18:10
tags:
- powershell
- workplace
---

The other day, I sketched out a few PowerShell scripts for starting and stopping services on my work PC. I didn’t end up needing them, so I’ll leave a couple of examples here before I lose them:

    get-service -Name 1C:Enterprise* | Where-Object {$_.status -eq 'running'} | stop-service -pass
    get-service -Name *SQL* | Where-Object {$_.status -eq 'running'} | stop-service -pass

Both lines above do the same thing: find services with a particular name, check their status, and stop them if they’re running. Patterns come in handy for the service names. Microsoft SQL Server, for example, has a whole brood of services with different purposes, and it’s handy to kill them with one line. Or take the 1C:Enterprise server service: there’s only one, but its name is so long that I can’t be bothered to type it in full :-)

You can do pretty much the same thing with regular processes. This line looks for obs64 and stops it if it’s there.

    get-process -Name obs64 | stop-process -pass

If you want the details, there’s plenty of [documentation](https://docs.microsoft.com/ru-ru/powershell/scripting/samples/managing-services?view=powershell-7), packed with examples and discussions along the lines of “how to start a service while meditating on one leg in artificial zero gravity.”
