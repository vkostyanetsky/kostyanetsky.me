---
title: String Concatenation
description: 'How should you build a string from fragments: concatenation, StrConcat(), or StrTemplate()?'
created: 2019-01-27 11:52:24
tags:
- 1c
- work
---

Like many companies, we give applicants a test assignment. It’s intentionally straightforward, mainly to see how well the candidate understands the platform and how carefully they work with configuration objects and code.

One little thing we look at is how they put together a long message with lots of parameters in it. A developer can use StrTemplate() or StrConcat(), or simply concatenate strings. We mark that last option down a little: the user sees the same result, but the code makes it much harder to picture roughly what the message will look like.

But there’s something else to it. In the first half of last year, Rarus published a [report](http://techlab.rarus.ru/upload/iblock/2/185/Beeline-obespechenie-bespereboynoy-raboti.pdf) about its work for Beeline. There’s lots of interesting stuff in there, but one bit floored me: dropping string concatenation cut CPU load by 40%! They didn’t even need those two extra servers anymore.

The problem was that worker processes spent too much time allocating memory to concatenate strings. Joining two strings once isn’t going to hurt, but building a large query in a loop and doing it repeatedly can make the load snowball. Don’t use concatenation in those cases. The right tools are the same StrTemplate() or StrConcat(). On older platform versions without those methods, you can use a text document object: add lines to it, then retrieve the text.
