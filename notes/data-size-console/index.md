---
title: The Size of Data in a 1C Database
description: Last week, while browsing the 8.3.15 release notes, I came across GetDatabaseDataSize().
created: 2020-07-11 14:00:51
tags:
- 1c
- done
---

Last week, while browsing the 8.3.15 release notes, I came across GetDatabaseDataSize(). I got curious about how it works and how its numbers compare with what you get from, say, Management Studio.

I ended up writing a sort of [console](https://github.com/vkostyanetsky/DataSizeConsole) to pass various metadata objects to the method, and started watching the platform’s database queries.

Basically, the platform works out the data size with something like this:

    CAST(
        SUM(
            CAST(
                DATALENGTH(T1._Fld40) AS NUMERIC(12, 0)
            )
        ) AS NUMERIC(18, 0)
    )

It does this for every field in the object, including standard attributes. If there are tabular sections, it counts those too, then sums the results.

Conclusions?

First, now I understand the method’s awkward name. It doesn’t calculate table size, as I initially thought, but **data size** specifically. The estimate excludes schema overhead, index overhead, and extent allocation. It counts only the size of the data stored directly in the object.

So a product catalog, say, will actually take up more space than this method reports. Possibly a lot more. You wouldn’t use it for precise analysis, but it’s fine for a quick look at where the data is piling up.

Second, the method doesn’t count the data history overhead for the objects being analyzed, which the docs are quite clear about. In theory, you could calculate it manually starting from _DataHistoryMetadata, but let’s wait a release or two: perhaps the developers will add it.

Third, the DBMS reads all the data in the tables it needs, then works out how big that data is. That probably means a whole bunch of scans, which can flush the buffer cache pretty quickly. In 1cFresh, the queries respect data separators, but that’s cold comfort to me.

Use it with care on a live production system.
