# Task object

Only available within a task, the `task` object contains attributes describing the current task itself.

## How to access it

* Use `{{ task.id }}` in a task [code](../../../core/tasks/code/)

## What it contains

This object is always a hash, containing the following keys:

* `"id"` – string
* `"created_at"` – string

This object is most useful for scheduling follow-up work for itself, using the "task\_ids" option in [the "event" action](../../../core/actions/event.md).

