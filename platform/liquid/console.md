# Liquid console

Mechanic's **Liquid console** is available in the footer of every page of the Mechanic Shopify app. It's a useful scratchpad for testing [**Mechanic Liquid**](./), without having to think about the output restrictions of [task previews](../../core/tasks/previews/). The console includes information about the final rendering **context** for each Liquid submission, which can be useful in understanding how Mechanic Liquid works.

{% hint style="info" %}
Unlike task previews, the Liquid console _does_ permit access to the Shopify Admin API, with a limit of 10 requests per Liquid submission. For REST requests, a page size of 10 is used, instead of the usual 250.
{% endhint %}

{% hint style="warning" %}
The Liquid console has much tighter restrictions than actual task runs. If you run into a memory limit error, or an error relating to the number of Shopify Admin API requests made, try testing your code in a new task subscribing to mechanic/user/trigger. This will allow you to experiment with your code in Mechanic's full runtime environment.
{% endhint %}

![](<../../.gitbook/assets/Screen Shot 2022-05-17 at 3.52.18 PM.png>)

## Context

Liquid code is always rendered in a certain **context**, which includes some information going in, and which may contain additional information afterward.

Mechanic's Liquid console includes information about the final rendering context, as inspected after the render. This includes...

* [Environment variables](../../core/tasks/code/environment-variables.md), as set by the Mechanic platform ahead of time
* Scope variables, as set by the user in their submitted Liquid code during rendering
* Information about the way the user's Liquid code interacted with the Shopify API during rendering

![](<../../.gitbook/assets/Screen Shot 2022-05-18 at 5.38.16 PM.png>)

### Events

When viewing a specific event in the Mechanic app, the Liquid rendering context bears the current event in mind. This means that variables available are similar to those given to any tasks that responded to that event: both the `event` variable and the [event's subject variables](../../core/tasks/code/environment-variables.md#event-subject-variables) are available to the console.

![](<../../.gitbook/assets/2022-05-18 17.48.01.gif>)

Because of this, the Liquid console can be used for examining what a task had access to at the moment of any given event. For a shopify/orders/create event, for example, one might use `{% log order.line_items[0] %}` to see the contents of an order's first line item.

{% hint style="info" %}
Looking at a child event? Try using `{% log event.parent %}` in the console to inspect data available from the parent. Or, to examine data from an ancestor event further back, try `{% log event.parent.parent %}`.
{% endhint %}

