# action

The **action tag** renders an [**action object**](../../../core/tasks/code/action-objects.md) in JSON, which in turn defines work to be performed by an [**action**](../../../core/actions/).

## Syntax

This tag has several usage styles, each style resulting in valid JSON for an action object.

{% hint style="info" %}
As with nearly all Liquid tags, the action tag supports Liquid variables. This means that the action type and options may be given using variables, instead of scalar values, like strings.
{% endhint %}

### Block syntax

This usage style offers the lightest form of abstraction, in that it only abstracts away `{ "action": ... }` layer of the resulting JSON object. Use this style when it's necessary to also provide an action's optional meta payload \(in addition to the required values of an action definition, i.e. action type and options\). This syntax requires a closing `{% endaction %}` tag.

{% page-ref page="../../../techniques/responding-to-action-results.md" %}

{% tabs %}
{% tab title="Liquid" %}
```javascript
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

🏆 This is the most common style of usage. The action type is given as an argument to the Liquid tag itself, and the action options are given in JSON in the tag's body. This syntax requires a closing `{% endaction %}` tag.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% action "http" %}
  {
    "method": "post",
    "url": "https://postman-echo.com/post",
    "body": {{ event.data | json }}
  }
{% endaction %}
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
{% action "cache", "set", "foo", "bar" %}
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
{% action "echo", foo: "bar", baz: "qux" %}
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
{% action "echo", "foo" %}

{% assign action_options = hash %}
{% assign action_options["foo"] = "bar" %}

{% action "echo", action_options %}
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

