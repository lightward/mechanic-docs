# Log objects

Log objects are useful for recording information for later reference. They have no side-effects, and they are not prominently displayed to the user. Carefully chosen log objects can massively simplify post-hoc debugging, especially \(as we've found\) when investigating merchant bug reports.

An log object is a plain JSON object, having the following structure:

```javascript
{
  "log": LOG_DETAILS
}
```

The log details can be any JSON value.

## The Log tag

Mechanic's Liquid implementation includes a Log tag, for quickly rendering error objects.

This tag has several usage styles, each style resulting in a valid action object.

### Block syntax

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% log %}
  {
    "foo": "bar"
  }
{% endlog %}
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
{% log "foobar" %}
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
{% assign details = hash %}
{% assign details["foo"] = "bar" %}

{% log message: "Something to remember", details: details %}
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



