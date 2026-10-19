---
title: One Query to Rule Them All
description: For posterity, I’ve uploaded one of the inefficient queries I worked on a few months ago to GitHub Gist.
created: 2020-07-25 17:55:10
tags:
- 1c
- done
- optimization
- work
---

For posterity, I’ve [uploaded](https://gist.github.com/vkostyanetsky/ddb286da9674b05d014389bed3b022c4) one of the inefficient queries I worked on a few months ago to GitHub Gist. It powered a dynamic list on a catalog item form, and whenever the user opened that tab, the platform went off to meditate, even with a relatively small database.

At first glance, it looks straightforward: a bunch of queries against document tables, joined with UNION ALL. Each table is filtered by roughly the same conditions: reference type, date, and whether the reference appears in a subquery’s result.

But there are several potential problems. First, at least 14 tables are involved, one per combined query. That alone raises the risk that the optimizer won’t have time to choose even a reasonably decent plan. Most likely, it will just take a wild guess and hope for the best.

Second, much of that work is probably wasted. Each query has something like this:

    WHERE VALUETYPE(AdditionalExpenses.Ref) IN (&DocumentsListSelectedTypes)

The dynamic list receives a list of document types to display. But that filter kicks in **after** the data has been read: even if the user only wants invoices, the DBMS still reads all 14 tables, then discards 13 of them.

Still, none of that is especially critical. If that were the full list of problems, we might never have investigated. The main issue is the second condition in the WHERE clauses: each query checks whether the document reference appears in a subquery’s result.

Using subqueries in conditions is almost taboo unless temporary tables are involved: the DBMS often can’t work out how much data it’ll get back, or how best to handle it. And the data source makes things even worse here. A filter criterion isn’t a database table you can read, even by scanning; it’s a collection of tables. DocumentsByProject contains thirty!

That sounds bad enough, but there’s more: each of the fourteen combined queries selects documents of a particular type from those thirty tables.

![OMG](https://media.giphy.com/media/oYtVHSxngR3lC/giphy.gif)

And… scene :-)
