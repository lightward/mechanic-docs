# Runs

[Events](../events/), [tasks](../tasks/), and [actions](../actions/) are all processed using queues, in which a piece of work is enqueued, and performed in its turn. Each piece of work is called a **run**. Thus, Mechanic performs work using event runs, task runs, and action runs.

When performed, a run has a **result**. Depending on the type of run, this result may define additional work to be performed:

* **Event runs**, when performed, result in a set of task runs.
* **Task runs**, when performed, result in a set of action runs.
* **Action runs**, when performed, have results that vary by [action type](../actions/#action-types).

Most runs are scheduled to be performed immediately. Some runs may be [scheduled](scheduling.md) for the future. Some runs may be [paused](pauses.md); some runs may be [retried](retries.md), once performed.

At the moment a run is performed, it loads in all related data (which may include the related store, or the related event, or the related task).&#x20;

## Run flow

A normal flow in Mechanic looks like this:

1. An event is created – possibly by a [Shopify webhook](../../platform/webhooks.md), or by a [user webhook](../../platform/webhooks.md), by the [Mechanic scheduler](../../platform/events/topics.md#scheduler), or by an [Event action](../actions/event.md).
2. An event run is created, and performed. During this phase, Mechanic scans the store's tasks to see which ones are relevant for the current event, by checking the subscriptions on file for each task. For each task that Mechanic discovers for the event, a task run is created. (If the task subscription involved an [offset](../tasks/subscriptions.md#offsets), as in "mechanic/scheduler/daily+2.hours", the task run will be set to wait for that amount of time.) The result of the event run is this set of task runs.
3. Each task run is performed. During this phase, Mechanic takes each task's [Liquid code](../tasks/code/), and renders it using the associated event. The result of the task run is the set of JSON [action objects](../tasks/code/action-objects.md) rendered by the task's Liquid code. Each action object is used to create an action run.
4. Each action run is performed. During this phase, Mechanic executes each action, given the options that were provided for it by the task run's result.

Understanding this sequence of events is important. Task runs do not come into existence until the event run has been performed, and action runs are only performed after their task run has fully concluded.

Importantly, this means that tasks do not have direct access to the effects of the actions they generate. Actions are performed later in the sequence, and their effects will only be seen by subsequent task runs.

## Run priorities

In general, given a mix of event, task, and action runs that are all due, Mechanic will perform due action runs first, then due task runs, and finally due event runs.

If Shopify's rate limit for _either_ the GraphQL or REST Admin API has been reached, Mechanic will skip over task runs and over [Shopify action](../actions/integrations/shopify.md) runs, until _both_ rate limits have been recovered. In these cases, Mechanic may choose to perform due runs of a lower priority, while it waits for the Shopify API rate limits to recover sufficiently to perform the higher priority runs.

## Run states

| Run state | Description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| Pending   | Scheduled to be performed, but the scheduled time has not yet arrived         |
| Due       | The scheduled time has arrived, and the run is waiting to be enqueued         |
| Enqueued  | The run has been placed in a queue, and is waiting to be performed            |
| Running   | The run is being performed                                                    |
| Cancelled | The run has been cancelled by a user                                          |
| Paused    | The run has been [paused](pauses.md) indefinitely, by a user or by the system |
| Failed    | The run has been performed, and an error has been recorded                    |
| Succeeded | The run has been performed, without errors                                    |
