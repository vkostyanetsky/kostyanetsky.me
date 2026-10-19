---
title: Unmaintainable Code
description: A good example of code that is impossible to maintain.
created: 2020-01-18 10:56:25
tags:
- 1c
---

I happened across an [example](https://helpf.pro/faq/view/1907.html) of a 1C query that calculates the difference between two dates in years, months, and days.

The code may work, I haven’t checked, but to me it’s a good example of what not to do. The problem is that its author will forget what most of those if-then-else branches mean within a couple of months, never mind other developers who decide to use it in their projects. The first time someone needs to change it, they’ll be better off throwing it out and writing something clearer.

And this is far from the most striking example. Just look at [regular expressions for validating email addresses](http://www.google.ru/search?q=email+regex). Makes you want to step outside for some air, doesn’t it? :-)
