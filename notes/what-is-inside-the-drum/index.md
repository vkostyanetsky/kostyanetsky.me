---
title: Exporting Standard Data Processors
description: The platform’s built-in data processors, available through “All Functions,” can be exported as ordinary epf files!
created: 2020-07-05 11:07:20
tags:
- 1c
- work
---

We recently swapped out SSLi’s creaky old homegrown versioning for the platform’s built-in data history in our configuration. I’m now finishing data history backup and restore. Surprisingly, neither the Standard Subsystems Library, the Standard Technologies Library, nor SSLi can do this yet, though I didn’t expect it from the last one.

I’ll write more when it’s finished. For now, here’s a neat trick that came in handy: the platform’s built-in data processors available through “All Functions” can be exported as ordinary epf files! Colleagues on Infostart explained the trick in detail [here](https://infostart.ru/public/369487/) and [here](https://infostart.ru/public/538300/). In brief, the magic looks like this:

    КопироватьФайл(
        "v8res://mngbase/StandardDataChangeHistory.epf",
        "Q:/StandardDataChangeHistory.epf"
    );

The links above have the full list of forms and processors you can pull out, how to get that list, and plenty of people arguing about it all.

Why might this help you? Honestly, I don’t know. In our case, we were building interfaces for data history and were curious about how 1C had written its own. Spoiler: it’s pretty messy.
