---
description: Your Mechanic dashboard — tasks, activity, and recent events at a glance.
---

# Home

The Mechanic home screen is your dashboard. It shows your installed tasks, current processing activity, and a live feed of recent events.

## Tasks

The main area of the home screen lists all the tasks installed on your store.

Each task row shows:

* **Task name** — click to open the [task editor](task-editor.md)
* **Status** — whether the task is enabled or disabled
* **Subscriptions** — the [event topics](../core/events/topics.md) that trigger the task
* **Tags** — custom labels for organizing tasks
* **Last modified date**
* **Update available** — shown when a newer version exists in the [task library](../resources/task-library/)

Use the **Add task** button to create a new blank task, browse the task library, or import tasks from JSON.

### Filtering and search

The task list supports full-text search across task names, code, and options. You can also filter by:

* Enabled/disabled status
* Event topic
* Shopify API version
* Tags

You can save filter combinations as **custom views** for quick access.

### Bulk actions

Select multiple tasks to add or remove tags in bulk, or export tasks as JSON.

## Current activity

The right sidebar shows real-time processing status for your Mechanic queue.

A **pulse indicator** shows your connection status:

* **Green** — connected and receiving live updates
* **Orange** — updates paused
* **Gray** — disconnected

The summary line shows how many runs are in progress and waiting, and whether the queue is on schedule or behind.

Expand **Show details** to see a breakdown of:

* **Running** — event runs, task runs, and action runs currently being processed
* **Waiting to run** — queued runs grouped by type, with counts

{% hint style="info" %}
Live updates automatically pause after 2 minutes of inactivity to save resources. Move your mouse or type to resume.
{% endhint %}

## Recent events

Below current activity, a live feed shows the 5 most recent [events](events.md). Each entry shows the event topic, timestamp, and a link to the full event detail.

Click **View more** to go to the [Events](events.md) page for the full event history.
