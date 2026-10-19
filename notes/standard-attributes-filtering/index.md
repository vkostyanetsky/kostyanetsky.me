---
title: Filtering Standard Attributes
description: A function to help skip standard attributes when iterating over a metadata object’s attributes.
created: 2020-03-09 13:52:03
tags:
- 1c
- done
---

A short [code fragment](https://gist.github.com/vkostyanetsky/ee1f913868673c2218877dfbecc9b08a) from the data history settings processor I just [wrote about](/notes/data-history-settings). It checks whether Attribute is MetadataObject’s standard attribute named StandardAttributeName. When is that useful? Suppose you’re iterating over an object’s standard attributes and want to skip one for some reason.

At first glance, the solution looks absurdly convoluted. Why not simply compare the attribute being checked with the one we want to exclude?

    If Attribute = MetadataObject.StandardAttributes.Order Then

Because that code doesn’t work: comparing two standard attributes of a metadata object in the 1C platform always returns False.

All right, you might say, why complicate things? How about this?

    If Attribute.Name = "Order" Then

That won’t work either if your code runs in a Russian configuration: the standard attribute we’re looking for is then named “Порядок.”

But you can look up the standard attribute in the collection by either its Russian or English name. Then you get its actual name and compare that with the name of the attribute you’re checking.

Of course, we could simply compare the name with both “Order” and “Порядок.” But first, that’s ugly; second, our coding standards don’t allow Russian in the code. I wanted a more general solution.

The Try/Catch is there in case the configuration has no standard attribute with that name at all. Trying to look it up would throw an error.
