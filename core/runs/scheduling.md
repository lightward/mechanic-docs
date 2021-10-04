# Scheduling

[Event](../actions/event.md) and [task](../tasks/) runs may be **scheduled** to perform in the future. They will not have any effect until they are performed. This means that their eventual performance may be impacted by changes to a store's Mechanic account, prior to the scheduled performance time.

## Event runs

Event runs may be scheduled using the [Event](../actions/event.md) action, using its `run_at` option to define the time at which the run should be performed.

The task runs that arise from a scheduled event run will not be established until the event run is performed. \(This does not apply if the `task_ids` option is used, which determines ahead of time which tasks may be run in response to the new event.\) This means that changes to the set of enabled tasks can have an impact on what tasks are actually run, in response to a scheduled event run.

Event runs generated in response to [Mechanic's scheduler events](../../platform/events/topics.md#scheduler) are always adjusted for the store's local time.

## Task runs

### Subscription offsets

Task runs may be scheduled using [subscription offsets](../tasks/subscriptions.md#offsets), in which a task states that it wishes to run later \(by some amount of time\) than the event that triggers it.

Subscription offsets are a property of the task, and are applied by the task run – _not_ the event run. This means that the subscribed-to event must be created and run _before_ the subscription offset is calculated and applied.

{% hint style="info" %}
The first task run on a new mechanic/scheduler/daily task may not be performed when expected, for the very first run.

To illustrate: if a user creates a task at 9am Monday, subscribing to mechanic/scheduler/daily+10.hours, they will have to wait until _the following midnight_ before the mechanic/scheduler/daily event is created. When that event's run is performed, the task's subscription offset will be calculated and applied, and the task run will be enqueued for 10 hours later. This means that the task will run for the first time on 10am Tuesday, _not_ 10am Monday.
{% endhint %}

### The Event action

Subscription offsets only allow for relative scheduling \(e.g. "run 5 days after event x"\); by contrast, absolute scheduling \(e.g. "run on December 16th at 2:30pm"\) can be indirectly achieved using the [Event](../actions/event.md) action, scheduling an event run which may then give rise to a task run at the scheduled time.

Task runs that are scheduled for the future will always use a task's latest configuration, including the task's [options](../tasks/options/), [code](../tasks/code/), and [Shopify API version](../tasks/shopify-api-version.md).

If a task is disabled or deleted at the time a task run comes due, the task run will still perform at the scheduled time, but will fail instantly.

