---
description: Suggestions for task code that's running out memory
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# How can I reduce memory usage of my task?

{% hint style="info" %}
Instant fixes are hard to come by for this kind of problem. If you help with your task code, [here's where to find assistance](../custom-help.md).
{% endhint %}

Memory efficiency is a dynamic problem. Here are some tips for task code when memory constraints become a factor.

## Look for loops.

Any time a data set is iterated upon, any memory inefficiencies within the loop have a chance to be multiplied in severity.

## Look for large strings.

Remember that Liquid variable assignments are always by value, not by reference, which can lead to surprise memory exhaustion with large values. Check on your assignments, concatenations, captures, etc.

## Look for opportunities to split up the work.

A single task run has a limited amount of memory available to it, but you can generate as many task runs as you like. If you need to "fork" into multiple task runs, use [Event actions](../core/actions/event.md) to create events with just enough data to create multiple tightly-scoped task runs.
