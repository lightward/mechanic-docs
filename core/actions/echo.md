# Echo

The **Echo** action has no effects: it returns the options that are given. This action can be useful for testing or debugging, by temporarily replacing some other action with an Echo action having the same options. In this way, a developer can safely get feedback on what data is in play, without side effects.

## Options

This action accepts any and all options, restricted only in that they must be valid JSON values (as with all results of [task code](../tasks/code/)).

### Forcing an error

If the Echo action is given a `"__error"` option, it will raise that error when the action run is performed. Use this feature when it's useful to indicate an issue with a task run, without marking the entire task run as a failure (as would be the case when using an [error object](../tasks/code/error-objects.md)).

{% tabs %}
{% tab title="Liquid" %}
```liquid
{% action "echo", __error: "Forcing an error!" %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "echo",
    "options": {
      "__error": "Forcing an error!"
    }
  }
}
```
{% endtab %}
{% endtabs %}

![](<../../.gitbook/assets/Screen Shot 2022-05-05 at 11.50.17 AM.png>)

## Examples

{% tabs %}
{% tab title="Liquid" %}
```liquid
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
```liquid
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
```liquid
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
```liquid
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
