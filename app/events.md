---
description: Monitor and investigate the events flowing through your Mechanic account.
---

# Events

The Events screen shows a live stream of [events](../core/events/) — the triggers that cause your tasks to run. Use it to monitor your automations and investigate issues.

## Event list

<figure><img src="../.gitbook/assets/events_mechanic.jpg" alt="The Events list showing recent events with their topics and task run results"><figcaption></figcaption></figure>

Events are displayed in reverse chronological order. Each row shows:

* **Time** — when the event occurred
* **Topic** — the type of event (e.g., `shopify/orders/create`)
* **Task runs** — which tasks responded, and their status

Click any event to see its full details.

### Live updates

The event list streams new events in real time. A **pulse indicator** shows your connection status:

* **Green** — connected and receiving live updates
* **No pulse** — paused

Use the **Pause/Resume** button to control the live stream manually. Updates also auto-pause after 2 minutes of inactivity and resume when you interact with the page.

### Filtering

Use the **Filters** button to narrow results by:

* **Search** — full-text search across event data
* **After / Before** — date and time range
* **Actions** — events with or without actions
* **Errors** — filter by error type (event, task, or action errors)
* **Topic** — filter by event type (supports prefix matching)
* **Task** — show only events processed by a specific task

The **Errors** tab provides quick access to all events where something went wrong.

You can save filter combinations as **custom views** for quick access.

## Event detail

<figure><img src="../.gitbook/assets/event_detail_mechanic.jpg" alt="An event detail view showing event runs, task runs, action runs, logs, and event data"><figcaption></figcaption></figure>

Clicking an event opens the detail view, which shows:

### Event runs

The processing record for the event. Each event run shows:

* **Status** — succeeded, failed, or in progress
* **Duration** — how long processing took
* **Task runs** — the tasks that executed, with their results

You can **retry** a failed event run or **cancel** a pending one.

### Event data

A sidebar shows the full event payload (JSON) — the data sent by Shopify or your system when the event was triggered.

### Related events

If this event was triggered by another event, or if it triggered child events, those relationships are shown in the sidebar.

### Export

Enter an email address to receive a full JSON export of the event and all related runs.
