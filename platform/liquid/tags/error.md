# error

The **error tag** generates an [**error object**](../../../core/tasks/code/error-objects.md).

## Syntax

This tag has several usage styles, each style resulting in a valid action object.

### Block syntax

```liquid
{% error %}
  {
    "foo": "bar"
  }
{% enderror %}
```

### Tag syntax, single argument

```liquid
{% error "foobar" %}
```

### Tag syntax, mapped arguments

```liquid
{% assign details = hash %}
{% assign details["foo"] = "bar" %}

{% error message: "Something went wrong!", details: details %}
```
