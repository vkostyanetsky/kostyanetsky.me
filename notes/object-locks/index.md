---
title: Object Locks
description: 'There are two types: pessimistic and optimistic object locks.'
created: 2019-04-22 18:42:54
tags:
- 1c
---

First, to avoid confusion: the platform’s object locks have nothing to do with managed locks, let alone DBMS locks. Second, there are two types: pessimistic and optimistic object locks.

Both are well [documented](https://its.1c.ru/db/metod8dev#content:5839:hdoc) on ITS; here’s the short version.

## Pessimistic Locks

A form extension applies one when the user starts editing an object, for example by changing a field. If someone else tries to edit the same object in a form, that form tries to get a pessimistic lock too. It can’t, so the user gets “Failed to lock record.”

The platform is being pessimistic here: the first user has started editing, so they’ll probably save. Better not let the second user change it in the meantime.

You can also apply a pessimistic lock using the object’s Lock() method and release it with Unlock(). Other options are the global context’s LockDataForEdit() and a managed form’s LockFormDataForEdit().

## Optimistic Locks

First, some theory: the platform stores versions for reference-type objects, such as catalogs and documents. Basically, a version tells you when the object last changed. When a form extension or code reads an object, its version is read with it.

When writing the object, the platform compares the version obtained when reading it with the version currently stored in the database. If they differ, it reports: “The operation cannot be performed because the versions do not match or the database record is missing.”

That’s an optimistic lock. It’s called that because the platform leaves the check until the last possible moment, when someone actually tries to save.

You cannot apply an optimistic object lock through code: the version is stored in the object table’s _Version field, populated by the DBMS. The platform provides no way to change it directly, though writing the object will change its version.
