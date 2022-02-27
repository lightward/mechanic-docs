# log

The **log tag** generates a [**log object**](../../../core/tasks/code/log-objects.md).

## Syntax

This tag has several usage styles, each style resulting in a valid action object.

### Block syntax

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% log %}
  {
    "foo": "bar"
  }
{% endlog %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "log": {
    "foo": "bar"
  }
}
```
{% endtab %}
{% endtabs %}

### Tag syntax, single argument

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% log "foobar" %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "log": "foobar"
}
```
{% endtab %}
{% endtabs %}

### Tag syntax, mapped arguments

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% assign details = hash %}
{% assign details["foo"] = "bar" %}

{% log message: "Something to remember", details: details %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "log": {
    "message": "Something to remember",
    "details": {
      "foo": "bar"
    }
  }
}
```
{% endtab %}
{% endtabs %}
