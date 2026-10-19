---
title: Multiple Languages in Vue.js
description: I write this website in Russian, but it also has an English mirror where I gradually translate my posts.
created: 2019-06-25 14:29:28
tags:
- blog
- done
---

I write this website in Russian, but it also has an English [mirror](https://kostyanetsky.me) where I gradually translate my posts. I don’t want to maintain two sets of scripts, so both domains point to the same server, which checks which domain received the request and replies in the right language.

Sounds simple, but it creates a bunch of other headaches. Getting the interface into the right language is one of them. At first, <s>out of habit</s>, I created two client-side arrays of phrases in different languages. I soon ditched that homegrown solution and plugged in [Vue I18n](https://www.npmjs.com/package/vue-i18n).

Underneath, it’s still just [two arrays](https://kazupon.github.io/vue-i18n/guide/formatting.html#named-formatting) of phrases that you pass in at startup. But that’s not the useful part. The plugin handles two other bits of the interface for you:

1. [Pluralization](https://kazupon.github.io/vue-i18n/guide/pluralization.html). Needed to display “page” correctly in [tag statistics](/notes/): in Russian, one post, two posts, and five posts require different noun forms.
2. [Date formatting](https://kazupon.github.io/vue-i18n/guide/datetime.html), such as the date below this note.

Sure, I could have done both on the server in PHP, but that felt messy. The server should return data; the client should build the interface. No need to mix them.
