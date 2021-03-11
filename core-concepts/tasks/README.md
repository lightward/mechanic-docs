# Tasks

In Mechanic, a **task** is a bundle of logic and configuration, that responds to and interprets [**events**](../events/). The result of a task can define [**actions**](../actions/), which are the task's opportunities to have an effect on the world.

A task responds to events based on its [**subscriptions**](subscriptions.md). When an event is received that matches a subscription, the task processes the event using its [**code**](code/). The code has access to the event data; it also has access to the user's task configuration, through [**options**](options/). Task code is written in Liquid, and is responsible for rendering a series of JSON objects \(including [**action**](code/action-objects.md), [**error**](code/error-objects.md), and [**log**](code/logging.md) objects\), defining work to be performed once task rendering is complete.

A task uses its [**preview**](previews/) to communicate ahead of time the work it intends to do. Previews are important for users, and are also important for Mechanic itself – Mechanic looks to the task preview to understand what permissions a task requires.

Tasks may be written from scratch, or installed from the Mechanic library \(available in-app and [on GitHub](https://github.com/lightward/mechanic-tasks)\). Once installed, a task's code may be modified at any time.

## Example

This task consists of:

* A subscription to shopify/customers/create
* Task code that defines an email action
* Usages of the `{{ options }}` variable that automatically generate a user-configurable form

{% tabs %}
{% tab title="Subscriptions" %}
```text
shopify/customers/create
```
{% endtab %}

{% tab title="Code" %}
```javascript
{% action "email" %}
  {
    "to": {{ options.email_recipient__email_required | json }},
    "subject": {{ options.email_subject__required | json }},
    "body": {{ options.email_body__multiline_required | newline_to_br | json }},
    "from_display_name": {{ shop.name | json }}
  }
{% endaction %}
```
{% endtab %}
{% endtabs %}

![](../../.gitbook/assets/screen-shot-2021-02-05-at-4.00.45-pm.png)

