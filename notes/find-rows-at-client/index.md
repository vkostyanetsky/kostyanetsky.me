---
title: Finding Rows on the Client
description: Calling FindRows() doesn’t necessarily cause a server call.
created: 2019-02-12 17:14:18
tags:
- 1c
---

I learned quite a while ago that FindRows() on form data collections can quietly generate server calls, when I ran into a form that took forever to load. The method was being called right in the opening handler, inside a loop, no less. You shouldn’t do that, of course, as the help gently suggests: calling the method executes a server call.

Today, though, while writing code for a similar task, I noticed there were no actual server calls. Surprised, I checked ITS. Here’s the explanation: when you call FindRows() on the client, the search does happen there, but the platform doesn’t keep all the data of large collections on the client. It transfers data from the server, and requesting that data is the server call mentioned in the help.

The platform developers are vague about the collection size below which the client doesn’t need the server; ITS [says](https://its.1c.ru/db/v8std/content/628/hdoc/_top/) it’s about twenty rows. So I’d still do the same thing: make an explicit server call and don’t worry about the platform adding its own. Still, it’s always nice to understand a little better what’s going on under the hood :-)
