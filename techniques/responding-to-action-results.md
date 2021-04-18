# Responding to action results

In writing a Mechanic task, it may be necessary to do more than a single round of generating actions. The task may need to act based on the results of an action, possibly performing additional actions depending on those results.

To achieve this flow, subscribe to the mechanic/actions/perform event topic. When a task includes this subscription, Mechanic will generate an event with that topic for every action that the task completes.

This strategy is commonly used with the [HTTP action](../core-concepts/actions/types/http.md), for reading data from third-party APIs \(see [Working with external APIs](working-with-external-apis.md)\).

{% hint style="info" %}
The [Echo action](../core-concepts/actions/types/echo.md) does not generate mechanic/actions/perform events.
{% endhint %}

## Inspecting the action

Events with the topic mechanic/actions/perform are, by nature, always child events \(see [Parent and child events](../core-concepts/events/parent-and-child-events.md)\). As such, their `event` variable contains a "parent" property. This means a task may use `event.parent.data` to access all the data that was used to create the action with which the current child event is associated.

Task runs also receive the `action` variable \(named after "actions" in "mechanic/actions/perform", and mirroring the contents of `event.data` – a standard pattern for events that are associated with a singular subject\). This variable contains a definition of the action, and data from the run that was performed for it.

{% hint style="warning" %}
It's important to specifically account for the mechanic/actions/perform topic when writing a task script, minding the fact that improper composition could result in an infinite loop.

Mechanic will step in and forcibly fail subsequent task runs that contain results identical to their predecessors.
{% endhint %}

## Example task

This example prompts the Mechanic user to enter a chunk of JSON, which will be used to create a customer record via a [Shopify action](../core-concepts/actions/types/shopify.md). If Shopify reports back that the customer creation was successful, the task will render an [Echo action](../core-concepts/actions/types/echo.md), reporting the success. If not, another Echo action will be rendered, reporting the specific error message from Shopify.

Note: This script is written to specifically support [previews](../core-concepts/tasks/previews/), using stub data during event preview to ensure that appropriate preview actions are generated.

{% tabs %}
{% tab title="Subscriptions" %}
```text
mechanic/user/text
mechanic/actions/perform
```
{% endtab %}

{% tab title="Code" %}
```javascript
{% if event.topic == "mechanic/user/text" %}
  {% if event.preview %}
    {% assign event = hash %}
    {% assign event["topic"] = "mechanic/user/text" %}
    {% assign event["data"] = '{"email":"customer@example.com"}' %}
  {% endif %}

  {% assign data = event.data | default: "{}" | parse_json %}
  {% action "shopify", "create", "customer", data %}
{% elsif event.topic == "mechanic/actions/perform" %}
  {% if event.preview %}
    {% assign action = hash %}
    {% assign action["type"] = "shopify" %}
    {% assign action["run"] = hash %}
    {% assign action["run"]["ok"] = true %}
  {% endif %}

  {% if action.run.ok %}
    {% action "echo", "Success!" %}
  {% else %}
    {% action "echo", "Invalid data!", action.run.error %}
  {% endif %}
{% endif %}
```
{% endtab %}
{% endtabs %}

