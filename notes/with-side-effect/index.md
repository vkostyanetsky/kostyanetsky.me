---
title: With Side Effect
description: About the dark side of the OnGetDataAtServer() handler.
created: 2026-03-15 12:24:12
tags:
- work
- 1c
---

In the latest release of [our ERP](https://firstbit.ae), we optimized several heavy dynamic lists — orders, invoices, proforma invoices, and so on. Over time they had accumulated a pretty solid pile of technical debt, mostly in the form of mountains of helper tables bolted onto the main queries: contact info, technical attributes, balances, turnovers, you name it.

At some point even in fairly small deployments the DB optimizer started producing complete nonsense instead of query plans, and the resource cost of letting that happen was getting less and less funny.

So, we patched things up using several different approaches. One of them was loading extra row data inside the `OnGetDataAtServer()` handler (I actually [mentioned](/notes/desire-paths/) this mechanism not that long ago). We built a nice little framework around it, rolled it out, tested it, and... Well, now we're all sitting here with those deeply unimpressed engineer faces.

To be fair, performance really did get a lot better. Inside a compact handler, you can tune queries against heavy virtual tables almost perfectly. The problem is somewhere else: field values populated by this handler are not passed into the standard dynamic list mechanisms. Which means search, sorting, and grouping simply do not work for those fields.

So you type a value that is clearly visible in the column — and the row is not found. Or not all matching rows are found. Or rows show up that visibly should not match at all. From the user's point of view, this looks absolutely terrible. A bug is a bug. Those fields don't look any different in the UI, so how exactly are you supposed to explain that this is "just how the platform works™"?

And if you explicitly exclude such a field from the mechanisms that can't handle it, that's somehow even worse. For example, try to sort by it — and boom, giant error message. Explaining why sorting blows up on this "Amount" column while the one right next to it works just fine is... Not exactly a beginner-friendly support conversation.

Honestly, how do you come up with such a great handler concept and then completely fumble the platform-level implementation?

This randomly reminded me of Reddit. Some communities love threads like "invent a superpower, but with a side effect". Like, one person comments: "I can run at the speed of the wind!" and someone replies: "yeah, but you can't stop". That kind of thing. Sometimes it gets pretty funny.

![Superpower](reddit.png)

That's basically the kind of conversation we're having with the platform developers. You can massively speed up dynamic lists, but the UI will make your users furious. You can automatically send binary data to an S3 bucket, but lose it all with one careless click. You can teleport anywhere, but it takes exactly as long as walking. You can shapeshift, but only into an elderly pug. You have thick, silky, luxurious hair, but on your ass.