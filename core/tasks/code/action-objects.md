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

#### Control parameters

Mechanic reserves a small set of double-underscore parameters for controlling how an action is handled. These control parameters include:

- `__perform_event`, which you can set to `false` to skip emitting the follow-up `mechanic/actions/perform` event for that specific action (the default is to emit it).
- `__meta`, which moves the provided value into the action's `meta` field so you can attach meta data while using any action tag syntax.

Examples:

```liquid
{%- action "http", method: "get", url: "https://postman-echo.com/get", __perform_event: false -%}
```

```liquid
{%- assign meta = hash -%}
{%- assign meta["source"] = "cache" -%}
{%- action "cache", "set", "foo", "bar", __meta: meta -%}
```

### Meta

Actions may optionally include **meta** information, annotating the action with any JSON value.

When you're using the action tag, you can attach meta in a few ways:

- Provide a `meta` object alongside `options` when you're supplying an options hash.
- Supply meta as the second positional argument when the options are a hash.
- Use the `__meta` control parameter with any action tag syntax to move the value into the action's meta.

```liquid
{%- assign options = hash -%}
{%- assign options["method"] = "post" -%}
{%- assign options["url"] = "https://postman-echo.com/post" -%}
{%- assign meta = hash -%}
{%- assign meta["mode"] = "initial_request" -%}
{%- action "http", options: options, meta: meta -%}
```

```liquid
{%- assign options = hash -%}
{%- assign options["method"] = "post" -%}
{%- assign options["url"] = "https://postman-echo.com/post" -%}
{%- assign meta = hash -%}
{%- assign meta["mode"] = "initial_request" -%}
{%- action "http", options, meta -%}
```

```liquid
{%- assign meta = hash -%}
{%- assign meta["note"] = "graphql" -%}
{%- action "shopify", __meta: meta -%}
  mutation {
    tagsAdd(id: "gid://shopify/Customer/1234567890", tags: ["vip"]) {
      node { id }
      userErrors { field message }
    }
  }
{%- endaction -%}
```

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
