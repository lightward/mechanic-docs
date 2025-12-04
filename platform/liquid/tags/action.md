# action

The **action tag** renders an [**action object**](../../../core/tasks/code/action-objects.md) in JSON, which in turn defines work to be performed by an [**action**](../../../core/actions/).

Mechanic also recognizes certain double-underscore control parameters on actions. For example, `__perform_event: false` tells Mechanic _not_ to emit the follow-up `mechanic/actions/perform` event for that action, and `__meta` lets you attach meta data without writing an explicit `meta` block. Control parameters work with every syntax shown below.

## Syntax

This tag has several usage styles, each style resulting in valid JSON for an action object.

{% hint style="info" %}
As with nearly all Liquid tags, the action tag supports Liquid variables. This means that the action type and options may be given using variables (instead of scalar values, like strings).
{% endhint %}

### Block syntax

This usage style offers the lightest form of abstraction, in that it only abstracts away `{ "action": ... }` layer of the resulting JSON object. It's one option for specifying [meta information for an action](../../../core/tasks/code/action-objects.md#meta) alongside the action's type and options, but you can just as easily add meta to the other syntaxes using `meta:` or the `__meta` control parameter (see below).

```liquid
{% action %}
  {
    "type": "http",
    "options": {
      "method": "post",
      "url": "https://postman-echo.com/post",
      "body": {{ event.data | json }}
    },
    "meta": {
      "mode": "initial_request"
    }
  }
{% endaction %}
```

### Block typed syntax

🏆 This is the most common style of usage. The action type is given as an argument to the Liquid tag itself, and the action options are given in JSON in the tag's body.

```liquid
{% action "http" %}
  {
    "method": "post",
    "url": "https://postman-echo.com/post",
    "body": {{ event.data | json }}
  }
{% endaction %}
```

### Tag syntax, positional options

Useful for action types that accept an array of options, this usage style accepts the action type as the first tag argument, and the following two or more positional arguments are used as options. This syntax does not have a closing `{% endaction %}`

tag.

```liquid
{% action "cache", "set", "foo", "bar" %}
```

### Tag syntax, mapped options

This usage style is appropriate for action types that accept a single-level, un-nested object of options. The action type is given as the first argument, and the mapping of values that follow is used as the action's options. This syntax does not have a closing `{% endaction %}`

tag.

```liquid
{% action "echo", foo: "bar", baz: "qux" %}
```

### Tag syntax, single option

This usage style accepts two arguments: the action type, and the action options. The action options may be given as a simple scalar value, or may be given as a variable reference to any Liquid variable. This syntax does not have a closing `{% endaction %}`

tag.

```liquid
{% action "echo", "foo" %}

{% assign action_options = hash %}
{% assign action_options["foo"] = "bar" %}

{% action "echo", action_options %}
```

## Control parameters

Control parameters adjust how Mechanic handles the resulting action. Add them inline with a double-underscore key. Two are available today:

- `__perform_event: false` skips emitting the follow-up `mechanic/actions/perform` event for that action while still running the action itself.
- `__meta` attaches meta to the action, even when you're using the tag syntaxes instead of an explicit `meta` object.

**Positional options**

```liquid
{% assign meta = hash %}
{% assign meta["source"] = "cache" %}
{% action "cache", "set", "foo", "bar", __meta: meta, __perform_event: false %}
```

**Mapped options**

```liquid
{% assign meta = hash %}
{% assign meta["note"] = "http" %}
{% action "http", method: "get", url: "https://postman-echo.com/get", __meta: meta %}
```

**Shopify GraphQL**

```liquid
{% assign meta = hash %}
{% assign meta["level"] = "info" %}
{% action "shopify", __meta: meta %}
  mutation {
    tagsAdd(id: "gid://shopify/Customer/1234567890", tags: ["vip"]) {
      node { id }
      userErrors { field message }
    }
  }
{% endaction %}
```
