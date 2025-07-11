# Action objects

An action object defines work to be performed by an [**action**](../../actions/), after the task is fully finished rendering. Action objects are most easily generated using the [**action tag**](../../../platform/liquid/tags/action.md).

An action object is a plain JSON object, having the following structure:

```javascript
{
  "action": {
    "type": ACTION_TYPE,
    "options": ACTION_OPTIONS,
    "meta": ACTION_META
  }
}
```

{% hint style="info" %}
Use the [action tag](../../../platform/liquid/tags/action.md) to skip the boilerplate while writing actions. All tasks in the Mechanic task library use the action tag, rather than writing out the action object in raw JSON.
{% endhint %}

{% hint style="info" %}
In Mechanic, actions are performed after their originating task run concludes. Actions are not performed inline during the task's Liquid rendering.

To inspect and respond to the results of an HTTP action, add a task subscription to mechanic/actions/perform, allowing the action to re-invoke the task with the action result data.

Learn more: [Responding to action results](../../../techniques/responding-to-action-results.md)
{% endhint %}

## Defining an action

### Type

The action **type** is always a string, having a value that corresponds to [a supported action](../../actions/) (e.g. `"shopify"`, or `"http"`).

### Options

Action **options** vary by action type. Depending on the action type, its options may be another complete object, or an array, or a scalar value.

### Meta

Actions may optionally include **meta** information, annotating the action with any JSON value.

This information could be purely for record-keeping, making it easy to determine why an action was rendered, or to add helpful context:

```javascript
{
  "action": {
    "type": "shopify",
    "options": [
      "post",
      "/admin/customers/1234567890/send_invite.json",
      {}
    ],
    "meta": {
      "invite_reason": "alpha",
      "customer_email": "customer@example.com"
    }
  }
}
```

Or, this information could be used to facilitate complex task flows, in concert with a subscription to mechanic/actions/perform (see [Responding to action results](../../../techniques/responding-to-action-results.md)). An action's meta information can supply followup task runs with information about state, allowing the task to cycle between different phases of operation.

```liquid
{% if event.topic contains "trigger" %}
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
  {% action %}
    {
      "type": "cache",
      "options": ["set", "foo", "bar"],
      "meta": {
        "mode": "second"
      }
    }
  {% endaction %}
{% elsif action.meta.mode == "second" %}
  {% action "echo", "done" %}
{% endif %}
```
