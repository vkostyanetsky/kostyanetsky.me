---
title: A Miku Pet for Codex
description: Made a custom pet for the Codex desktop app, based on Miku from the old platformer Miku'n POP.
created: 2026-08-18 09:17:10
tags:
- ai
- done
---

The desktop version of [Codex](https://openai.com/codex) has pets — little pixel characters rendered on top of the UI that show what the agent is currently doing. When it's working, the pet walks around; when it's waiting for input, it calls for you; when something goes wrong, it squints, and so on. The general idea is that you kick off some workflow, go do something else, and can still tell at a glance how things are going.

Not exactly a killer feature, but it's fun. I wasn’t particularly into the built-in pets, though, so I started browsing community-made ones and somehow ended up making my own — based on Hatsune Miku's design from [Miku'n POP](https://chaim-videogames.itch.io/mikun-pop), an ancient little platformer.

Turns out it's not especially complicated. I took the sprites from [MikuPet](https://github.com/CharlesWiiFlowers/MikuPet), whose author uses the same Miku'n POP design. The pet format itself is pretty straightforward, and with Codex helping me poke around, there wasn't much reverse-engineering involved. I actually spent more time [figuring out](https://github.com/vkostyanetsky/MikuCodexPet/blob/main/ATTRIBUTION.md) who owns which bits of artwork, what the reuse terms are, and all that fun licensing stuff.

You can [grab the pet](https://github.com/vkostyanetsky/MikuCodexPet/releases) from GitHub. There’s also a screenshot and a [detailed rundown](https://github.com/vkostyanetsky/MikuCodexPet) of what's under the hood.

Free bonus: the [release workflow](https://github.com/vkostyanetsky/MikuCodexPet/actions/workflows/release.yml) for a project like this can, with complete justification, be called "Release pet" :)