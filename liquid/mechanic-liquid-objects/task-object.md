# Task object

Only available within a task, the `task` object contains attributes describing the current task itself.

## How to access it

* Use `{{ task.id }}` in a task [script](https://help.usemechanic.com/tasks/scripts)

## What it contains

This object is always a hash, containing the following keys:

* `"id"` – string
* `"created_at"` – string

This object is most useful for scheduling follow-up work for itself, using the "task\_ids" option in [the "event" action](https://help.usemechanic.com/en/articles/1967813-the-event-action).

