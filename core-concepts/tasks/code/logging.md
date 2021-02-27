# Log objects

Log objects are useful for recording information for later reference. They have no side-effects, and they are not prominently displayed to the user. Carefully chosen log objects can massively simplify post-hoc debugging, especially \(as we've found\) when investigating merchant bug reports.

An log object is a plain JSON object, having the following structure:

```javascript
{
  "log": LOG_DETAILS
}
```

The log details can be any JSON value.

Log objects are most easily generated using the [**log tag**](../../../liquid/mechanic/tags/log.md).



