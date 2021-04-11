# Action objects

An action object defines work to be performed by an [**action**](../../actions/), after the task is fully finished rendering. Action objects are most easily generated using the [**action tag**](../../../platform/liquid/tags/action.md).

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

{% hint style="info" %}
Use the [action tag](../../../platform/liquid/tags/action.md) to skip the boilerplate while writing actions.
{% endhint %}

