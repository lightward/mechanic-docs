# log

The **log tag** generates a [**log object**](../../../core/tasks/code/log-objects.md).

## Syntax

This tag has several usage styles, each style resulting in a valid action object.

### Block syntax

```liquid
{% log %}
  {
    "foo": "bar"
  }
{% endlog %}
```

### Tag syntax, single argument

```liquid
{% log "foobar" %}
```

### Tag syntax, mapped arguments

```liquid
{% assign details = hash %}
{% assign details["foo"] = "bar" %}

{% log message: "Something to remember", details: details %}
```
