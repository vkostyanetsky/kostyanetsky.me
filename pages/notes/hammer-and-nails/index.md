I was developing payment documents in our configuration last week and came across an incredibly redundant solution to a primitive problem. Sorry, I can't keep to myself.

Just imagine: you have a document, which contains several tabular sections. Each of them has a comment field. You make a print form for this document; if at least one row of any TS contains a comment — you need to use one template 1, if there are no comments — template 2.

The task is pretty primitive, isn't it? All of us have done something like this a million times. Just read the selection, use IsBlankString() on the comment field and load the appropriate template. Coffee time!

However, instead of a short cycle, I saw this:

https://gist.github.com/vkostyanetsky/e870d5bb3d2f23d93f3d17001eaef59b

There is a chain of queries, in the lowest of which we scan all the TS (which, I remind you, we just raked out for printing). We are looking for comments in them, then we group the result several times and return it to the script.

Well, I'm not even talking about the load on the DBMS (I would venture to guess that this trick doesn't give a noticeable effect — after all, the selection is going to be quite small). It's just… Well… Checking a selection of rows is, like, five lines of code. Clear, simple, short, Sonar has no room to swear. How could you give birth to this? Because of great love for queries?

You know what? I bet that it's the correct answer. I can almost see this programmer, who has just mastered the query language more or less tolerably. He is in the absolute delight of new opportunities, so… If all you have is a hammer, everything around looks like nails.
