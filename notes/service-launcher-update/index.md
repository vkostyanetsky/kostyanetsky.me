---
title: Service Launcher Update
description: I’ve improved Service Launcher’s behavior when run without parameters.
created: 2020-07-22 20:23:40
tags:
- done
- powershell
- workplace
---

I’ve improved [Service Launcher](/notes/service-launcher/). If you run the script without parameters, specifying neither -start nor -stop, it now decides for itself whether to start or stop the services.

It checks the status of the first service in the list. If it’s running, the script stops everything on the list. If it’s stopped, the script tries to start them all.

Why? If, like me, you run the script from a programmable keyboard, you now need only one key. Previously, you needed two: one to start the services and another to stop them.
