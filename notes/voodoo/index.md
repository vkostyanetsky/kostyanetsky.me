Looks like we finally caught our first lab-reproducible case of platform cache corruption. Short synopsis:

- Spin up a fresh app from the v35 template of [our ERP](https://firstbit.ae).
- Run it and wait for initialization to finish.
- Swap the app configuration to the v36 release (specifically build 28537 from the dev repo) and run it again.

After this super simple sequence, about half the team sees the platform suddenly lose one of the enums. And it's annoyingly selective: hit an enum value on the client — all good; do the same on the server — boom, exception.

Same story with the manager module of one catalog: its methods exist and are exported, but after the update the platform pretends they don't and throw an exception when you call them.

[![Welcome To Dipshit Central](welcome.jpg)](https://x.com/EffinBirds/status/1970264357427704080)

As always, when the magic blooms — look at the cache. Clear it and symptoms vanish. There are indirect hints too; in v35/v36:

- The enum did exist in v35, but it was renamed in v36.
- Those manager-module methods didn't exist in v35 (they were added in v36).

So the v35 cache didn't contain either of these in their v36 identities, yet for some reason the platform insists on digging into that old cache.

We still don't have a clean root cause. We traced it to a specific commit after which the bug started showing up, but the only weird thing there its [its name](the-commit.png) (and honestly, there are nearby commits that look even [more suspicious](the-other-commit.png) from this point of view). The rest were [minor changes](5ba6ac0956e0cc7bc6b520e5110420e6950478fe.diff) that regenerate internal metadata IDs in the manifest. Yes, those IDs are tied to cache keys, but they get bumped on any metadata change and have never caused issues before.

If you landed here from Google because you hit the same mess — make a no-op change to the problematic object so the platform refreshes the metadata IDs in the manifest. For example, add an empty method or even just a space in the module. That fixes the configuration so the recipe at the top stops corrupting the cache.

Voodoo, sure — but hey, it works ¯\_(ツ)_/¯