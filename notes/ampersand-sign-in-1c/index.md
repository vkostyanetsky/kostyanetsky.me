---
title: The Ampersand in 1C
description: When handling ampersands in labels, the platform follows the standard Windows convention.
created: 2018-12-27 10:11:50
tags:
- 1c
---

For ampersands in labels, the platform does the usual Windows thing: the ampersand itself disappears, and the following character becomes a keyboard shortcut for the action associated with the label, such as a command handler or a control event.

The example below shows up as “Project Costing.” The “P” is underlined and becomes the shortcut key; if this is a command title, for example, you can run it with Alt + P.

    ТекстНадписи = "&Project Costing";

The only catch is when you want an actual ampersand in the label. Then you need to double it: the example below will appear as “Project P&L.” No shortcut keys will be created.

    ТекстНадписи = "Project P&&L";
