# Responding to action results

In writing a Mechanic task, it may be necessary to do more than a single round of generating actions. The task may need to act based on the results of an action, possibly performing additional actions depending on those results.

To achieve this flow, subscribe to the **mechanic/actions/perform** event topic. When a task includes this subscription, Mechanic will generate an event with that topic for every action that the task completes.

This strategy is commonly used with the [HTTP action](../core/actions/http.md), for reading data from third-party APIs \(see [Working with external APIs](working-with-external-apis.md)\).

{% hint style="info" %}
The [Echo action](../core/actions/echo.md) does not generate mechanic/actions/perform events.
{% endhint %}

## Inspecting the context

Events with the topic mechanic/actions/perform are, by nature, always child events \(see [Parent and child events](../core/events/parent-and-child-events.md)\). As such, their `event` variable contains a reference to the event's parent. This means a task may use `event.parent.data` to access all the data that was used to create the action with which the current child event is associated. \(Note that only the previous 5 generations of parents are available.\)

Task runs also receive the `action` variable, named after "actions" in "mechanic/actions/perform", and mirroring the contents of `event.data` \(a standard pattern for events that are associated with a singular subject\). This variable contains the original definition of the action \(including its type, options, and meta package\), and data from the run in which the action was performed.

Common sources of information for followup task runs:

* `action.options` — the original options defined for the action
* `action.run.ok` — `true` or `false`, indicating the action's success
* `action.meta` — the [meta package](../core/tasks/code/action-objects.md#meta) given in the action's definition

{% hint style="warning" %}
It's important to specifically account for the mechanic/actions/perform topic when writing a task script, minding the fact that improper composition could result in an infinite loop.

Mechanic will step in and forcibly fail subsequent task runs that contain results identical to their predecessors.
{% endhint %}

## Examples

### Switching on action success

This example prompts the Mechanic user to enter a chunk of JSON, which will be used to create a customer record via a [Shopify action](../core/actions/shopify.md). If Shopify reports back that the customer creation was successful, the task will log the success. If not, an email is sent, reporting the error.

Note: This script is written to specifically support [previews](../core/tasks/previews/), using stub data during event preview to ensure that appropriate preview actions are generated.

{% tabs %}
{% tab title="Subscriptions" %}
```text
mechanic/user/text
mechanic/actions/perform
```
{% endtab %}
{% endtabs %}

{% tabs %}
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
    {% log "Success!" %}
  {% else %}
    {% action "email" %}
      {
        "to": "admin@example.com",
        "subject": "Failed to create a customer",
        "body": {{ "Alert!" | append: newline | append: newline | append: action.run.error | json }}
      }
    {% endaction %}
  {% endif %}
{% endif %}
```
{% endtab %}
{% endtabs %}

### Switching on action meta

This example cycles between three different modes of operation, depending on the meta information recorded in the action definition. And, by checking on `action.run.ok` at the very beginning, the task is also able to "break" into failure-handling mode.

{% tabs %}
{% tab title="Subscriptions" %}
```text
mechanic/user/trigger
mechanic/actions/perform
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Code" %}
```javascript
{% if action.run.ok == false %}
  {% log "Failure!" %}
{% elsif event.topic contains "trigger" %}
  {% action %}
    {
      "type": "cache",
      "options": ["set", "foo", "bar"],
      "meta": {
        "mode": "first"
      }
    }
  {% endaction %}
{% elsif action.meta.mode == "first" %}
  {% capture mutation %}
    mutation {
      tagsAdd(id: "gid://shopify/Customer/739331866685", tags: ["processed"]) {
        userErrors { field, message }
      }
    }
  {% endcapture %}

  {% action %}
    {
      "type": "shopify",
      "options": {{ mutation | json }},
      "meta": {
        "mode": "second"
      }
    }
  {% endaction %}
{% elsif action.meta.mode == "second" %}
  {% action "echo", "done" %}
{% endif %}
```
{% endtab %}
{% endtabs %}

