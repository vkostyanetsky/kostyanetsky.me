---
title: Preparing for the 1C:Expert Exam
description: A few things that helped me keep my head together while preparing for the exam.
created: 2021-05-25 21:25:48
tags:
- 1c
- done
---

In mid-May, I passed the 1C Expert exam on the technical side of large implementations. I’d spent almost two years preparing, so I’m damn glad I finally got through it.

There’s tons of material online about the exam and everything around it, from Viktor Bogachev’s [course](https://uc1.1c.ru/course/podgotovka-k-1s-ekspertu-po-tehnologicheskim-voprosam-osnovnoj-kurs/) to, say, a Telegram [chat](https://t.me/OneC_Expert) on the subject. So there’s no shortage of answers to the question in the title. But there are a couple of ways this can mess with your head that people rarely talk about. It helps to know about them beforehand.

## Not Enough Dopamine

This exam is hard. Preparing can take a long time, and so can passing it: there’s a good chance you’ll need several attempts. I passed on my second try, but it varies. Some people I know passed on their third, fifth, or even eighth attempt.

You can end up putting in a huge effort again and again with nothing to show for it. Eventually, your brain decides this is something to avoid: it never works out, it feels pointless, and it makes you miserable. You can guess what that does to your chances of passing.

That’s why it’s better to treat preparation as a long, risky project: break it up with small tasks you can actually finish and feel good about. Your brain doesn’t draw much distinction between big and small successes, and getting back to the main project will be easier.

For example, as a distraction I wrote home automation scripts: the [static site generator](https://github.com/vkostyanetsky/BlogBuilder) for this website, an organizer based on text files, tools for my home NAS, and other things I’d never got around to. Any tasks will do, including ones that have nothing to do with IT. Unclog the bathroom drain? Excellent idea!

## Not Enough Memory

Unless you deal with 1C performance problems every day, this exam eats up a lot of mental RAM. You have to keep all sorts of things in your head that you’d normally just look up: Microsoft SQL Server trace flags and dynamic view names, PostgreSQL settings and recommended values, the platform’s standard indexes, and so on and so forth. A hu-u-uge pile of information! *(gestures to show how huge)*

Give your brain enough data, and it starts acting suspiciously like a DBMS with an overflowing buffer cache. In other words, anything you don’t need right now will be evicted from memory. Be prepared to forget the most unexpected things: your apartment number, the neighbor’s dog’s name, your wife’s birthday, and those aren’t even the strangest consequences. While preparing, I could really relate to Johnny Mnemonic, the guy from an ancient cyberpunk film who loaded so much data into his brain that he nearly died :-)

![Don’t do this](dont-do-it.jpg)

I see two ways out, both fairly obvious.

First, get some practice in. Complicated technical details are a hundred times easier to remember when they bring up a picture of the app where you tried them out.

Second, find something to associate the really tricky bits with. Take Bash! The utility name sed is easier to remember if you know it stands for Stream EDitor. The name cat comes from concatenate, not the animal, but if the animal helps you remember it, use that. Why not?

Or take the byte order mark: \xef\xbb\xbf, yes. I got it to stick with, don’t laugh, the completely unrelated phrase “Elf Friend BB Best Fight.” I have no idea why my brain needs <s>such ass-backwards</s> such roundabout ways of getting things in, but I’m not complaining. Whatever works!
