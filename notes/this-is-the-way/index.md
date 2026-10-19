---
title: This Is the Way
description: A little existential despair from the Syntax Assistant, in case you were short of it.
created: 2023-07-17 00:46:01
tags:
- 1c
---

A little existential despair from the Syntax Assistant, in case you were somehow short of it:

![An exception is generated](delete-messages.png)

This is the help for the integration service channel manager’s DeleteMessages() method. Seriously, I get what they meant: all integration service methods throw exceptions if called in a session with data separators enabled. The documentation carefully mentions this for every method, so apparently part of the sentence went missing here. The English documentation for this method, for example, is fine.

It still looks funny. Reminded me of an old meme:

![An exception is generated](bees.jpeg)
