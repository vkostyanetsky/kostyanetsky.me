---
title: Auto-Refresh for Timesheet
description: Made a few improvements to my Obsidian plugin that generates a list of tasks with the time spent on each one.
created: 2026-07-26 12:37:00
tags:
- obsidian
- javascript
---

I spontaneously returned to my old [Obsidian plugin](https://github.com/vkostyanetsky/ObsidianTimesheet), which takes the daily note for a given day and generates a list of tasks along with the time spent on each one.

The plugin had an original sin of a bug: whenever the note text changed, the plugin block would not refresh on its own. So you would add a task, change the time spent, or fix a description, while the report kept showing the old data. You had to either reopen the note itself or start editing the block and then cancel, just to trigger a manual render. It was annoying — and smelled suspiciously like reinventing the wheel.

Now the plugin block automatically re-renders after changes — just the block, not the entire note. There is also a short delay after typing and an additional check: if the report has not actually changed, no render happens at all.

I also wanted to update the dependencies while I was at it — it has been a year, after all — but... You know, NPM remains its usual shining example of order and discipline. Every tiny library for adding two numbers inevitably depends on ten others, every package is somehow a second cousin twice removed of the fence next door, and you cannot replace a single one without consequences: `npm audit` complains whenever someone sneezes. So, at first glance, I could not figure out how to update the dependencies reliably without breaking the plugin itself.

Oh well. That is a problem for tomorrow me.
