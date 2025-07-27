I stumbled upon a hefty Infostart [article](https://infostart.ru/1c/articles/2434171/) all about "debunking myths" around the platform.

Honestly, about half of it reads like a joke; I can't take it any other way. It's like:

> Hot off the press! Crime, intrigue, investigations! File‑based infobase is faster than client–server! DCS is slower than a plain request! Calling a server method on the server counts as a brand-new server call! <s>UFO over Red Square!</s>

Still, there are some genuinely interesting nuggets! For instance, I'd never heard the claim that cramming your code into one single line makes it run ten times faster. Too bad you'd get your ass handed to you by the team for trying that kind of desperate swag before you even get to celebrate the speed boost. And let's be real — most enterprise app slowdowns usually come from totally different places.

Or take their speed comparison for dumping data into a table: one test for object presentations and another for their descriptions. The presentation dump was literally tens of times slower! On paper, that sounds weird — both are just strings already pulled from the database at measurement time. My guess is it's down to typing: presentation strings can be unlimited length, unlike descriptions. So you get extra memory allocations, helper structures popping up, and bam — that's where the slowdown creeps in.

P.S. I've jotted down some of these points myself before. Off the top of my head — I remember [benchmarking](/notes/is-ref-empty) ValueIsFilled() and getting a nasty surprise from the built‑in [FindByNumber()](/notes/method-with-surprise).