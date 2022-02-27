# error

The **error tag** generates an [**error object**](../../../core/tasks/code/error-objects.md).

## Syntax

This tag has several usage styles, each style resulting in a valid action object.

### Block syntax

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% error %}
  {
    "foo": "bar"
  }
{% enderror %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "error": {
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
{% error "foobar" %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "error": "foobar"
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

{% error message: "Something went wrong!", details: details %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "error": {
    "message": "Something went wrong!",
    "details": {
      "foo": "bar"
    }
  }
}
```
{% endtab %}
{% endtabs %}
