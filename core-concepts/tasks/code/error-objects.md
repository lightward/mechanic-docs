# Error objects

When a task renders an error object, the task run will be marked as failed. This is a good way to communicate an intentional failure to the user, when your Liquid code detects a certain condition. A task that renders an error object [during preview](../previews/) will prevent the user from saving it in that state, which means that error objects are a good way to enforce additional validation on [task options](../options.md).

An error object is a plain JSON object, having the following structure:

```javascript
{
  "error": ERROR_DETAILS
}
```

The error details can be any JSON value. This value will be represented to the user as the reason for the task failing.

## The Error tag

Mechanic's Liquid implementation includes an Error tag, for quickly rendering error objects.

This tag has several usage styles, each style resulting in a valid action object.

### Block syntax

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% error %}
  {
    "foo": "bar"
  }
{% enderror %}
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
{% error "foobar" %}
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
{% assign details = hash %}
{% assign details["foo"] = "bar" %}

{% error message: "Something went wrong!", details: details %}
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



