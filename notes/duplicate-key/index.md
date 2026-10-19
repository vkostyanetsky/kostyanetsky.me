---
title: A Duplicate Key
description: Infostart published an interesting article about the DBMS error “cannot insert duplicate key.”
created: 2019-03-01 17:17:35
tags:
- 1c
- done
- work
---

Infostart published an interesting [article](https://infostart.ru/public/1010017/) about the DBMS error “cannot insert duplicate key.” One possible cause is using an object’s unique identifier to match it during exchanges with other infobases, without a dedicated register mapping local objects to identifiers in the external database.

I haven’t yet encountered anything as wild as unique identifiers colliding across different infobases for the same table. But I abandoned the “single identifier” approach for good after building an exchange between [FirstBIT ERP](https://firstbit.ae/products/erp-dubai/financial_management/) and [GROTEM / Agent](http://superagent.ru/products/grotem_agent).

The first version worked like this: data from FirstBIT ERP was first exported to an intermediary 1C infobase written by GROTEM’s developers, and then to the mobile application server’s main database.

Matching was very simple: object identifiers. The same document in both databases had the same identifier, for example. But some mappings were more complicated: a FirstBIT ERP counterparty became three objects on GROTEM’s side: a counterparty, a retail outlet, and a settlement document.

![WAIT WHAT](https://media.giphy.com/media/zkSFsZpQMZuG4/giphy.gif)

Yes, a settlement document. All three objects had the same identifier, that of the FirstBIT ERP counterparty. It wasn’t particularly elegant, but the platform handles this arrangement perfectly well, and we decided there shouldn’t be any problems.

The exchange refused to work, though.

A quick look showed that the data made it to the intermediate infobase just fine and passed every integrity and completeness check. Only the last step was failing: sending it to the mobile application server’s database.

Eventually, it turned out that:

1. This database has a table storing all the objects we care about: documents, catalog items, and so forth.
2. That table has a unique index on the object GUID.

Well, mystery solved. Of course, the very first exported counterparty created three entities with the same GUID. They lived happily in their separate tables on the 1C side, but couldn’t be written into one shared table in the application database.

![I will not use GUIDs to map objects!](i-will-not-use-guids.gif)
