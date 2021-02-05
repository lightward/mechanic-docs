# Action objects

An action object is a plain JSON object, having the following structure:

```javascript
{
  "action": {
    "type": ACTION_TYPE,
    "options": ACTION_OPTIONS
  }
}
```

The action type is always a string, having a value that corresponds to [a supported action](../../actions/) \(e.g. `"shopify"`, or `"http"`\).

Action options vary by action type. Depending on the action type, its options may be another complete object, or an array, or a scalar value.

## The Action tag

Because so much of a task's work is generating actions, Mechanic's Liquid implementation includes an Action tag, for simpler code.

This tag has several usage styles, each style resulting in a valid action object.

### Block syntax

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

### Tag syntax, ordered arguments

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

### Tag syntax, mapped arguments

{% tabs %}
{% tab title="First Tab" %}
```javascript
{% action "echo", foo: "bar", baz: "qux" %}
```
{% endtab %}

{% tab title="Second Tab" %}
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

