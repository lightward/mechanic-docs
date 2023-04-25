# Log objects

Log objects are useful for recording information for later reference. They have no side-effects. Carefully chosen log objects can massively simplify post-hoc debugging, especially (as we've found) when investigating merchant bug reports.

A log object is a plain JSON object, having the following structure:

```javascript
{
  "log": LOG_DETAILS
}
```

The log details can be any JSON value.

Log objects are most easily generated using the [**log tag**](../../../platform/liquid/tags/log.md).

Log objects appear wherever task run results are visible, including the task preview and when viewing an event.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-04-25 at 3.16.30 PM.png" alt=""><figcaption><p>A log object visible in a task preview</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Screenshot 2023-04-25 at 3.16.58 PM.png" alt=""><figcaption><p>A log object visible in a task run's result</p></figcaption></figure>
