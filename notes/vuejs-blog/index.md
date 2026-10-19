---
title: A Blog in Vue.js
description: In the middle of last year, I got excited about rewriting this website with something more modern than PHP.
created: 2020-07-21 20:01:10
tags:
- blog
- done
- javascript
---

In the middle of last year, I [got excited](/notes/so-much) about rewriting this website with something more modern than PHP, chose Vue.js as the framework, and got to work. I barely knew JavaScript at the time, so I stumbled over plenty of things like “the date formatting method crashes in Safari.” Eventually, though, I had a working app.

Well, sort of. On one hand, I achieved my main goal: a client-side web application that only asks the server for data and builds the entire interface itself. On the other, I had to bolt an army of <s>workarounds</s> hooks onto the framework even for basic things, like highlighting menu items or returning the correct HTTP codes. Some things I couldn’t get working at all on the first go, such as serving HTML versions of pages to Yandex, whose crawlers in 2020 can’t index JavaScript websites.

I learned a lot, but the end result was such a mixed bag that I eventually gave up on the idea and [published](https://github.com/vkostyanetsky/VuejsBlog) the resulting Frankenstein on GitHub. Perhaps it will save someone time on tasks like “display arbitrary HTML through Vue.js” or “make VueI18n work with Russian.”

There’s no documentation, though. I initially wanted to explain how everything worked, but quickly realized it would take a lot of time for little obvious benefit. I’ll probably return to the subject when the mood strikes. For now, let’s call it an MVP :-)
