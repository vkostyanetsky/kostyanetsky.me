---
title: Bloated Roles
description: An interesting post about the trouble the “Clear All Rights” command can cause in a role.
created: 2019-04-18 16:57:50
tags:
- 1c
- work
---

A very interesting [post](https://start1c.blogspot.com/2019/01/blog-post.html) about the trouble the “Clear All Rights” command can cause in a role. It does what it says, sure: the rights are gone. But the role object can end up eating several times more memory than it needs, out of nowhere.

I read it, thought about it, and went to check our configuration. And what do you know? I found a couple of dozen roles stuffed to the brim with “unchecked boxes.” The rights export weighed in at around three hundred megabytes. After a cleanup, it was almost three times smaller.

It’s curious that this had gone unnoticed for so long. Most likely, it’s down to the object count: our configuration is relatively small. Those roles just weren’t eating enough resources for us to notice.
