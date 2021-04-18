# Echo

The **Echo** action has no effects: it returns the options that are given. This action can be useful for testing or debugging, by temporarily replacing some other action with an Echo action having the same options. In this way, a developer can safely get feedback on what data is in play, without side effects.

## Options

This action accepts any and all options, restricted only in that they must be valid JSON values \(as with all results of [task code](../tasks/code/)\).

## Examples

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

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% action "echo", "foo", "bar", "baz" %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "echo",
    "options": [
      "foo",
      "bar",
      "baz"
    ]
  }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% action "echo", "foo" %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "echo",
    "options": "foo"
  }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% action "echo" %}
  {
    "foo": "bar",
    "baz": "qux"
  }
{% endaction %}
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

