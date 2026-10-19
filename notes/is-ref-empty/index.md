---
title: IsEmpty() or ValueIsFilled()?
description: A few days ago, a colleague and I discussed the best way to check whether a reference is filled.
created: 2020-04-11 12:24:12
tags:
- 1c
- done
---

A few days ago, a colleague and I discussed the best way to check whether a reference is filled. I remembered an instructor telling us, very confidently, to be careful with ValueIsFilled(). Supposedly, it compares the variable with every possible empty value in the configuration, and the whole thing is a huge, slow mess. As an alternative, the instructor recommended the reference’s IsEmpty() method and checking composite attributes roughly like this:

    СсылкаЗаполнена = Ссылка <> Неопределено И Не Ссылка.Пустая();

My colleague argued that ValueIsFilled() is faster because it’s a built-in function and doesn’t spend time looking up a method on an object, a relatively slow operation in scripting languages.

I can’t recall either method ever being a bottleneck, but I got curious and wrote a [test](https://github.com/vkostyanetsky/IsEmptyOrValueIsFilled) that checks a reference variable for an empty value both ways and measures the time taken.

The average result looks [roughly like this](is-ref-empty.png). There are four steps: the first two assume the reference is defined, while the third and fourth add a check for that. The table’s last column shows the execution context, client or server.

The result is pretty clear: ValueIsFilled() doesn’t slow things down at all. It’s actually faster than IsEmpty(), especially once we start checking the variable for Undefined. The difference isn’t enormous, of course, but it could save a couple of seconds in some lengthy scheduled operations.

Besides, one check that works for everything is easier than trying to guess every value the variable might have.
