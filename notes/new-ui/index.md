Over the New Year holidays I randomly got obsessed and rewrote my blog's UI. I only wanted to add search for my notes — there are a lot of them now, and every so often I need to quickly fish something out of the pile (like "here's that link" to a coworker).

The blog runs on [GitHub Pages](https://pages.github.com), so the options aren’t exactly infinite: either outsource search to Google, or build a static index and ship it to the user's browser so it can grep through it locally. I went with the second route: faster, more controllable, <s>and, yes, an excuse to write code</s>. The first time you search it has to download [the index file](/notes.json), but... It's 200 KB. That's basically one medium-sized sigh in 2026 internet terms.

And then, you know how it goes... Scope creep grabbed me by the hoodie. First I couldn't get [Tachyons](https://tachyons.io) to play nice with the search input — got annoyed and migrated everything to [Tailwind](https://tailwindcss.com) (I'd been meaning to try it anyway, just never had a "good reason", so I invented one). While I was writing the search code, I figured it'd be dumb not to add tags too, because why do the same work twice. Next thing I know I "wake up" and there's a whole tag cloud sitting above my notes. At that point it felt only logical to do the same for the projects page — except there it's not tags, it's tech stacks...

So yeah, it turned into that "Fear and Loathing in Las Vegas" meme. The tendency was to push it as far as I can, kinda.

Now the only thing left is to make myself actually write into the new project log. There's always a ton of work, and it's genuinely interesting — but if I don't write things down… Welp. Everything sinks into coffee.
