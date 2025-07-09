# Can Mechanic events or tasks be prioritized?

Mechanic processes events in the order they're received, and — as of this writing — there are no affordances for prioritizing the event queue.

Here's a future describing something different:

{% embed url="https://mechanic.canny.io/futures/p/task-prioritization" %}

For right now (i.e. as of this writing), the path forward usually involves redesigning the specific operational flow of your specific Mechanic account and its subscriptions.

For example: if your event queue is being clogged up with shopify/products/update events (we see this a lot), consider refactoring away that subscription. Instead of responding to update events, you might use a scheduler event (e.g. mechanic/scheduler/hourly, mechanic/scheduler/10min, etc) to trigger a task that does specific GraphQL-based scans of products, acting selectively as needed.

The Mechanic platform team can't help with specific designs for this kind of thing (we tried! not sustainable!), but there are folks you can talk to who can help:

* [partners.mechanic.dev](https://partners.mechanic.dev/) — Mechanic-fluent professionals, available for hire
* [slack.mechanic.dev](https://slack.mechanic.dev/) — Our community Slack workspace, where Mechanic implementers of all skill levels help each other out
