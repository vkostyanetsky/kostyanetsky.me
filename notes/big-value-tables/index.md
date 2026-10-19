---
title: Large Value Tables
description: Writing about finding rows on the client reminded me of another quirk of large collections on managed forms.
created: 2019-02-17 11:55:37
tags:
- 1c
---

While writing about [searching for rows](/notes/find-rows-at-client/) on the client, I remembered another detail about data on managed forms, also involving transfers between client and server.

Say we need to work with fairly large value tables, thousands of rows each. These might hold data that generally doesn’t need to be shown to the user: pricing parameters, sales and purchase information, or something else. Creating these tables as form attributes seems logical, if only because the designer is convenient.

But things get awkward when the platform sends one of those tables to the server. Instead of a nice compact ValueTable, it sends a much bulkier FormDataCollection. The difference is enormous: on one of our projects, this data exchange accounted for around 40% of a key operation’s total runtime.

The fix is pretty obvious: don’t use form attributes for the tables. Instead, create value tables in code, for example in the OnCreateAtServer handler, put them in temporary storage, and keep only their storage addresses in form attributes. When you need to work with a table, only the address string is sent to the server, so you spend next to nothing on serialization and data transfer. On the server, retrieve the table, read its data, change it if necessary, and save it back to temporary storage at the same address.
