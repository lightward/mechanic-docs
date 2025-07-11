# How do I add a Shopify access scope to my task?

If you're reading this page, you might have encountered an error like this one:

{% code overflow="wrap" %}
```
{"message": "Access denied for metaobject field. Required access: `read_metaobjects` access scope."}
```
{% endcode %}

The solution is usually found in [**task previews**](../core/tasks/previews/)! Mechanic auto-applies access scopes as the need is demonstrated during task preview. If a scope is missing, it's usually because it wasn't used during task preview.

## Debugging steps

1. Create a new task, subscribing to something like mechanic/user/trigger.
2. Copy in _just_ the parts of your code that are using the scope(s) in question, without any if/else blocks.
3. Does the new task request the right scope(s)?
   * If yes: Great! This means that the problem can be solved at the level of task code, by making sure that the code you just tried is evaluated during task preview.
     * Tip: use [preview events](../core/tasks/previews/events.md) to reach event-specific parts of your task.
   * If no: Great! Export your new task (i.e. the one that's _specifically_ focused on the issue), and send it to team@usemechanic.com, so a member of the platform team can take a look.
     * Important: the platform team can't help with debugging custom code, but they _can_ help with platform issues. If you've reached this part, you might have found a platform issue!

## Getting more help

The best place to compare code-level notes is [**our community Slack workspace**](../resources/slack.md). Join up, start a thread, and share your code in the new thread.

If your issue is time-sensitive, [**get help from a professional Mechanic partner**](https://partners.mechanic.dev/).
