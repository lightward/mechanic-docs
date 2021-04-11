---
description: An index of event topics in the Mechanic domain
---

# Mechanic

## Actions

* **mechanic/actions/perform** Occurs when an action has been performed, regardless of its success or failure. A task may subscribe to this topic to be notified when each of its actions have been performed, so that the task may then respond to the results.

## Emails

* **mechanic/emails/received** Occurs when Mechanic receives an email sent to the store's dedicated email address. \(A store at example.myshopify.com can receive email at example@mail.usemechanic.com.\)

## Scheduler

* **mechanic/scheduler/10min** Runs every 10 minutes.
* **mechanic/scheduler/15min** Runs every 15 minutes.
* **mechanic/scheduler/20min** Runs every 20 minutes. \(You see where this is going.\)
* **mechanic/scheduler/30min** Runs every 30 minutes.
* **mechanic/scheduler/hourly** Runs every hour, on the hour.
* **mechanic/scheduler/daily** Runs at midnight, in the store's local timezone.

{% hint style="info" %}
More specific times can be achieved by subscribing to an event topic with a delay \(e.g. "mechanic/scheduler/daily+5.hours"\). To learn more, see [Subscriptions](../../tasks/subscriptions.md).
{% endhint %}

## User

Not to be confused with events in [the User domain](user.md), the mechanic/user event subject describes ways that the user expresses their intent – to Mechanic – to run the task.

* **mechanic/user/trigger** When used, adds a "Run task" button to the task itself. This button generates a new event, to which only this task will respond.
* **mechanic/user/text** When used, adds a "Run task" button to the task itself. This button prompts the user with a multi-line text box. When submitted, an event is generated, to which only this task will respond. The event contains the user's input in its data, making user's input available in `{{ event.data }}`.

