# action

The **action tag** generates an [**action object**](../../../core-concepts/tasks/code/action-objects.md).

## Syntax

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

### Tag syntax, positional options

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

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% action "echo", "foo" %}
```
{% endtab %}
{% endtabs %}



