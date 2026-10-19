---
title: A Method with a Surprise
description: 'A couple of weeks ago, I encountered an annoying bug in the simplest possible task: finding a document by number.'
created: 2020-07-27 07:38:49
tags:
- 1c
---

A couple of weeks ago, I encountered an annoying bug. The task couldn’t be simpler: find a document by number. Do it this way, and the document is found:

    SELECT Ref FROM Document.Invoice WHERE Number = &Number

But this way? No luck:

    Documents.Invoice.FindByNumber(Number)

At first I froze for a moment, then checked the documentation and, of course, found the answer. FindByNumber() has a second parameter, IntervalDate, for finding documents with periodic numbering. It narrows the search to a particular period. If numbering resets annually and you pass May 1, 2020, for example, the search covers January 1 through December 31, 2020. And Invoice does indeed use annual numbering.

So what’s the problem? The vague phrase “the parameter is used for documents with periodic numbering” actually hides an iron rule: you **cannot** omit the parameter for such documents.

To check, let’s look at the SQL the DBMS actually runs. An ordinary query brings no surprises. P1 is the separator, P2 the document number:

    SELECT
        T1._IDRRef 
    FROM
        dbo._Document283 T1
    WHERE
        ((T1._Fld704 = @P1))
        AND ((T1._Number = @P2))

But calling FindByNumber() adds a third parameter with the value 2001-12-31 23:59:59. Yes, “the end of the first year since the beginning of time”:

    SELECT
        T1._IDRRef
    FROM
        dbo._Document283 T1
    WHERE
        ((T1._Fld704 = @P1))
        AND
        (
            T1._Number = @P2
            AND T1._Date_Time <= @P3
        )

Naturally, this query doesn’t find the required document. It won’t find anything in a real infobase, ever, under any circumstances.

The solution is clear: pass IntervalDate or, if we don’t have a date, use the Standard Subsystems Library’s ObjectAttributeValue() or an equivalent. But honestly, the platform should throw an exception when FindByNumber() is called without IntervalDate on a document with periodic numbering, rather than quietly execute a query guaranteed to be pointless.
