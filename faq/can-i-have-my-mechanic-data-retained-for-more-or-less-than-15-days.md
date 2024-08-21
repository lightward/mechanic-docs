---
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

# Can I have my Mechanic data retained for more (or less) than 15 days?

Indirectly, yes! Let's review how Mechanic's data retention works, first:

{% hint style="info" %}
* An event and its related task and action runs are deleted/expunged 15 days after the event is considered "complete".
* An event is "complete" when both of the following are true:
  * The event's task and action runs are complete (i.e. they've been run cancelled)
  * The next five generations of child events are considered "complete"

This is an abbreviated summary! See the rest of the details here: [Retention of events](../platform/policies/data.md#retention-of-events)
{% endhint %}

The 15 day period is fixed. We don't support different retention periods for specific Mechanic accounts, whether longer or shorter.

However, you can get around this by generating child events, using [Event actions](../core/actions/event.md). By scheduling a child event for the future, you can prevent the parent event from being deleted until 15 days after that child event is performed. This gives you a measure of control over data retention at the event level – something that isn't available at the shop level.

## Example

This Event action schedules an event for 5 days in the future. Once the child event runs, the parent event will be considered complete. 15 days from completion, the parent event is deleted.

Any impact from other child events aside, the result is a practical data retention period of 20 days for this event.

```liquid
{% raw %}
{% action "event" %}
  {
    "topic": "user/noop/data_retention",
    "run_at": {{ "now" | date: advance: "5 days" | json }}
  }
{% endaction %}
{% endraw %}
```
