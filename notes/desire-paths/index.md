Alright, Jean Fresco's riddle. You've got a table — say, ~50k rows. How do you end up reading half a billion?

Easy-peasy: Nested Loops + Clustered Index Seek:

![Half a billion](500_million.png)

Clustered Index Seek is a bit of a marketing name here, of course. In reality, on every execution the operator walks the entire table (the whole clustered index) and checks every row against the predicates. And it does that 10 730 times for 51 391 rows. Result: 551 425 430 rows read, 13 343 returned.

![Ouch](ouch.gif)

So yeah — a perfect textbook example of a horrible query plan in a vacuum. Put it in a museum. Nested Loops, if you've forgotten, works roughly like this:

<pre>
For Each Table1Row In Table1 Do
    For Each Table2Row In Table2 Do
        ...
</pre>

That's fine for small tables, but the DB can also pick it for bigger ones — for example, if it runs out of time to build a proper plan.

That's exactly what happened here. Zooming out to the platform level: we've got a dynamic list that queries a documents table, and the devs bolted on like a dozen virtual tables from accumulation registers.

Some of those registers were huge on their own, and the virtual tables poured gasoline on the fire (each one turns into 2+ nested queries). The DB honestly tried to come up with an efficient strategy, but at some point it basically decided: "a garbage plan is still better than no plan at all".

And the user? They tried to search a document by number — and the client app just straight-up froze.

So, about virtual tables in dynamic lists. There's a nice English phrase: "desire path" — the trail people naturally carve because it's the easiest way. Slapping a virtual table onto the main one really is often the simplest, fastest, most familiar way to solve the task. But it's **not efficient**.

There's an alternative, for example the OnGetDataAtServer() handler. It takes longer to implement, but it lets you properly tune the virtual table and avoid the scenario above. Scrolling the list will produce more queries, sure — but they'll be smaller, faster, and way more efficient than one single giant monster query.