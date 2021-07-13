# Error objects

When a task renders an error object, the task run will be marked as failed, and no rendered action runs will be performed. This is a good way to communicate an intentional failure to the user, when your Liquid code detects a certain condition.

A task that renders an error object [during preview](../previews/) will prevent the user from saving it in that state, which means that error objects are a good way to enforce additional validation on [task options](../options/).

{% hint style="warning" %}
Unlike a "raised" exception in other programming languages, a rendered error object is simply added to the list of the task run's JSON objects. At the completion of task code rendering, all objects are evaluated; at that point, if an error object is among them, the error is then raised and shown to the user.

An error object does not halt rendering of the task's Liquid code, but it does prevent any other rendered objects from having an effect. Specifically, this means that the presence of an error object means that any action objects will be ignored.

This also means that rendering an error object will not prevent the task from reaching any syntax errors \(or other problematic code\) later on in the task's Liquid code.
{% endhint %}

An error object is a plain JSON object, having the following structure:

```javascript
{
  "error": ERROR_DETAILS
}
```

The error details can be any JSON value. This value will be represented to the user as the reason for the task failing.

Error objects are most easily generated using the [**error tag**](../../../platform/liquid/tags/error.md).

