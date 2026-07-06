# Background tasks

Mechanic's [run system](../../runs/) gives every shop a queue of work, performing runs as quickly as the shop's capacity allows. Normally, all of a shop's tasks share that capacity on a first-come, first-served basis — which means a single task generating tens of thousands of runs (say, a backfill tagging every historical order) can leave your time-sensitive automations waiting in line behind it.

Two advanced task settings, found in the "Runtime" tab of the advanced task editor, let you decide how a task shares your queue:

* **Run this task in the background** – Background runs wait until your other tasks are caught up. A background task's runs only ever receive spare capacity: whenever any of your other work is waiting, that work goes first, and incoming events are always processed ahead of background runs. Use this for bulk jobs and backfills that aren't time-sensitive.
* **Maximum concurrent runs** – Caps how many of this task's runs may be running at the same time, leaving room in your queue for your other tasks. Leave this blank for no limit.

## How background tasks behave

* The background setting applies to the task's runs — both its task runs and the action runs they generate. Event processing is unaffected: events are always received and interpreted at normal speed, so your run history fills in promptly even when the resulting work waits.
* Changing the setting takes effect right away, in both directions, including for runs that are already waiting in the queue (a very large backlog may take a few moments to finish moving over). If a bulk task is slowing down your store right now, marking it as a background task moves its queued work to the back of the line right away — and un-checking the setting brings a background task's waiting runs back to normal scheduling just as quickly.
* A busy shop may pause background work entirely for a while — that's the setting working as intended, not a stuck task. Mechanic still guarantees slow progress: a background run that has been waiting for several hours is periodically given a slot even while your queue is busy, so background work can be paused, but never stopped for good.
* Nothing is ever dropped. A background task's runs queue up without limit, exactly like any other task's — the background setting only changes *when* they run, never *whether* they run.
* Background waiting is always visible. The Mechanic home page shows how much background work is waiting and for how long, and each waiting run shows how many runs from your other tasks are ahead of it. Background work doesn't count toward your queue's lag — a deliberately-waiting backfill isn't your queue running behind.

{% hint style="warning" %}
Background tasks are not recommended for tasks that use [bulk operations](../../../platform/graphql/bulk-operations.md). Shopify's bulk operation results are only available for a limited time, and a background task may wait long enough for its results to expire.
{% endhint %}

## Maximum concurrent runs

A concurrency limit is useful when a task's work should keep moving, but shouldn't be allowed to fill your whole queue. A task with a limit of 2 will never have more than 2 of its runs (task runs plus action runs) executing at once, no matter how much of its work is waiting — the rest of your queue stays available for your other tasks.

Compared to the background setting, a concurrency limit is the gentler tool: a background task yields *completely* to your other work, while a concurrency-limited task keeps a steady, bounded pace regardless of what else is happening. The two settings can be combined.

A manually retried run re-enters the queue like any other run, using its task's current settings — retrying a run that failed while its task was a background task, after the task has left the background, runs it at normal priority.
