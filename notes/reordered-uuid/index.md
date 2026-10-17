---
title: 1C's UUID Storage Trick
description: How 1C stores UUIDs in the database, and what indexes have to do with it.
created: 2026-10-17 19:24:11
tags:
- 1c
- optimization
---

I was patching up our 1cFresh setup again (more trouble with data history — I'll tell that story sometime) when I ran into the platform's little UUID storage trick once more. Figured I'd write about it.

Long story short, UUIDs for metadata objects, just like those for regular object references, are stored in a 1C database as sixteen bytes. Except when you look at those bytes, they don't look anything like the UUIDs you're used to: for example, `8e1f9db8-4be2-11ed-a101-2c4d5450919d` shows up in the database as `0xA1012C4D5450919D11ED4BE28E1F9DB8`. The first time I saw this, I thought: some kind of hash, or a bit of little-endian wizardry.

What the platform actually does is rearrange the UUID groups while keeping the byte order within each group intact:

```
UUID:   aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
SQL:    dddd eeeeeeeeeeee cccc bbbb aaaaaaaa
```

Why? My guess is that it's an attempt to make database indexes work more efficiently. The thing is, the platform uses UUID v1 when automatically generating references for documents, catalog items, and similar objects. These UUIDs are time-based, with the timestamp split into three parts. The low part comes first, followed by the middle and high parts:

```
8e1f9db8-4be2-11ed-a101-2c4d5450919d
│        │    │    │    │
│        │    │    │    └─ Node ID (48 bits)
│        │    │    └────── Clock sequence + variant (16 bits)
│        │    └─────────── Time high + version (16 bits)
│        └──────────────── Time mid (16 bits)
└───────────────────────── Time low (32 bits)
```

In a 1C database, the same UUID looks like this:

```
0xA1012C4D5450919D11ED4BE28E1F9DB8
  │   │           │   │   │
  │   │           │   │   └─ Time low (32 bits)
  │   │           │   └───── Time mid (16 bits)
  │   │           └───────── Time high + version (16 bits)
  │   └───────────────────── Node ID (48 bits)
  └───────────────────────── Clock sequence + variant (16 bits)
```

So the rearrangement puts the timestamp parts in order from most to least significant. With the same clock sequence and Node ID, a later timestamp gives you a larger binary UUID value.

References form part of index keys. When index keys arrive in a random order, new entries land all over the B-tree. If the target index page is already full, it has to be split: allocate a new page, move some entries over, and update the links in the tree. That's called a page split, and it can increase index fragmentation and hurt performance — both immediately and down the road.

With mostly increasing keys, though, new entries usually go at the end of the index. Pages fill up more sequentially, and inserts are often cheaper.

(of course, lots of concurrent inserts bring another problem: everyone wants to write to that same last page and starts elbowing each other out of the way, but that's a story for another day)

Small thing, right? But I like this kind of stuff: you're staring at sixteen odd-looking bytes in a database, and behind them, you can almost make out a perfectly sensible architectural decision somebody made all those years ago.
