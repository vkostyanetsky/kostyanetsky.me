> There is a path to a CSV file. You need to open it, read the header (first line), find the Salary column and display the top 10 salaries.

> *[Link](https://t.me/nikitonsky_chat/26402)*

Threw in my [two cents](https://gist.github.com/tonsky/881d5d8c4fbed818fe2905a7591a91e0#file-vkostyanetsky-1c) just to complete the picture. If you forget about stability, readability, and performance — you can cut it in half. Here it was obvious from the very beginning that it would be shorter on the bash, so I just wrote it as I was used to.

What was useful: there are a bunch of examples in other languages ​​in the comments to the post. Frankly speaking, I haven’t come across some of them; it was really curious to look at the syntax and try to understand the way to solve the task.

In general, the whole story reminded Eugene Stepanishev's [hobby](https://bolknote.ru/tags/beer99/) — to write the output of the American “beer song” in all languages ​​in a row. By the way, Tonsky's issue looks as fun for me too — too trivial to seriously compare something on its basis.

What was funny: for a couple of [colleagues](https://t.me/nikitonsky_pub/201?comment=26703), the 1C code caused such acute vision problems that they considered it necessary to report it :-) I partly understand the desire to assert itself on the stereotype “1C is bad, mkay?”, but it's the wrong case. The preference in syntax is nothing but a matter of taste, and besides it, the solution in 1C is no different from solutions in any other language without a built-in library for parsing CSV.