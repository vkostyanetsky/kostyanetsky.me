---
title: Unresolved References
description: Unresolved references to metadata objects usually appear when objects are deleted, and sometimes when configurations are merged.
created: 2020-03-10 20:08:36
tags:
- 1c
---

Unresolved references to metadata objects usually appear when objects are deleted, and sometimes when configurations are merged. ITS has an [article](https://its.1c.ru/db/metod8dev#content:2290:hdoc) with a good explanation of the details.

Broken references don’t always affect the application’s behavior. A field without a data binding is easy to notice, of course. But suppose you delete a functional option: what does it matter if a reference remains somewhere in a form command? It simply sits there like a cut wire, inert and harmless. Nothing passes through it.

Still, these things gradually clutter the configuration and should be removed. The ITS advice isn’t all that helpful: go through every place a broken reference might be hiding. Imagine a complicated document form with lots of controls. How long would that search take? And success isn’t guaranteed.

There’s a rougher way that works: delete groups of controls from the form, one after another. Start with large groups, such as pages. After each removal, check the configuration and see whether the broken reference has disappeared. Before starting, it’s best to delete all form code so the platform doesn’t complain about references to nonexistent controls.

The problem might also be in the form itself rather than its controls, but that’s much easier to locate: settings storage, conditional appearance, commands. There aren’t many candidates.

You can also export the configuration to files and dig through the XML. That’s also worth doing because what you see in Designer can be misleading. If a form command references a missing functional option, for example, you might see nothing wrong in the command’s properties. But the XML will contain a suspicious GUID instead of an option name: practically a signed confession :-)
