---
title: A New Object’s Reference
description: A unique identifier is a convenient way to match objects across configurations.
created: 2018-11-29 19:22:56
tags:
- 1c
---

A unique identifier is a convenient way to match objects across configurations, for example when transferring invoices from one infobase to another. How can the receiving infobase tell whether an invoice has already been loaded? The first idea is simply to find the object by UUID:

    ИнвойсЗагружен = Документы.Инвойс.ПолучитьСсылку(UUID).Пустая();

Sometimes, though, unique identifiers are used to link objects within the same configuration. Maybe the task won’t let us add a regular reference attribute, so document A can’t point straight to document B. Or perhaps we create a catalog item based on a document and want a direct link between them, assigning the item a reference obtained from the document’s unique identifier. The platform guarantees identifier uniqueness within a table, so such tricks are perfectly safe.

My point is: use SetNewObjectRef() carefully! Check whether a reference has already been assigned before your code runs:

    СсылкаЗадана = ПолучитьСсылкуНового().Пустая();

If there’s already a reference, deal with it: leave it alone if that works for your task, or throw an exception. Otherwise, you can break the links between your data, and good luck tracking that down. Or even noticing it.

This works both ways, of course: some other code, such as an object event handler or subscription, might overwrite the reference you assigned. Testing is the only reliable way to catch that.
