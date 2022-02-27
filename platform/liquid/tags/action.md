# action

The **action tag** renders an [**action object**](../../../core/tasks/code/action-objects.md) in JSON, which in turn defines work to be performed by an [**action**](../../../core/actions/).

## Syntax

This tag has several usage styles, each style resulting in valid JSON for an action object.

{% hint style="info" %}
As with nearly all Liquid tags, the action tag supports Liquid variables. This means that the action type and options may be given using variables (instead of scalar values, like strings).
{% endhint %}

### Block syntax

This usage style offers the lightest form of abstraction, in that it only abstracts away `{ "action": ... }` layer of the resulting JSON object. Use this style when it's necessary to also provide [meta information for an action](../../../core/tasks/code/action-objects.md#meta), in addition to the action's type and options.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
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
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "http",
    "options": {
      "method": "post",
      "url": "https://postman-echo.com/post",
      "body": null
    },
    "meta": {
      "mode": "initial_request"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Block typed syntax

🏆 This is the most common style of usage. The action type is given as an argument to the Liquid tag itself, and the action options are given in JSON in the tag's body.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "http" %}
  {
    "method": "post",
    "url": "https://postman-echo.com/post",
    "body": {{ event.data | json }}
  }
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "http",
    "options": {
      "method": "post",
      "url": "https://postman-echo.com/post",
      "body": null
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Tag syntax, positional options

Useful for action types that accept an array of options, this usage style accepts the action type as the first tag argument, and the following two or more positional arguments are used as options. This syntax does not have a closing `{% endaction %}` tag.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "cache", "set", "foo", "bar" %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "cache",
    "options": [
      "set",
      "foo",
      "bar"
    ]
  }
}
```
{% endtab %}
{% endtabs %}

### Tag syntax, mapped options

This usage style is appropriate for action types that accept a single-level, un-nested object of options. The action type is given as the first argument, and the mapping of values that follow is used as the action's options. This syntax does not have a closing `{% endaction %}` tag.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "echo", foo: "bar", baz: "qux" %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "echo",
    "options": {
      "foo": "bar",
      "baz": "qux"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Tag syntax, single option

This usage style accepts two arguments: the action type, and the action options. The action options may be given as a simple scalar value, or may be given as a variable reference to any Liquid variable. This syntax does not have a closing `{% endaction %}` tag.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "echo", "foo" %}

{% assign action_options = hash %}
{% assign action_options["foo"] = "bar" %}

{% action "echo", action_options %}
{% endraw %}
```
{% endtab %}

{% tab title="" %}
```javascript
{
  "action": {
    "type": "echo",
    "options": "foo"
  }
}

{
  "action": {
    "type": "echo",
    "options": {
      "foo": "bar"
    }
  }
}
```
{% endtab %}
{% endtabs %}
