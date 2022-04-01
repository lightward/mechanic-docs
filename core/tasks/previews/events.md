# Defining preview events

During [**task preview**](./), Mechanic scans the task's [**subscriptions**](../subscriptions.md). For each [**event topic**](../../events/topics.md) found, Mechanic constructs a synthetic **preview event**, resembling one that the task might encounter during live use.

By default, each preview event's data is sampled from previous events that the Mechanic account has seen, for the same topic.

However, developers may define their own preview events, containing whatever data the developer wishes to use for preview. This may be useful for several reasons:

* Multiple preview events may be defined per event topic. This allows developers to verify that their task renders the appropriate results under a variety of circumstances.
  * Defined preview events can be labeled with a description, which is visible in the task preview pane. This makes it easy to identify the scenario that a preview event is meant to represent.
* Defining preview event data is usually simpler than defining [stub data](stub-data.md) for the `event` variable.
  * Stubbing the `event` variable removes any intelligence from the objects found in `event.data`, a drawback avoided by defining a preview event and its data. Using the [Order object](../../../platform/liquid/shopify/order.md) as an example, a task may typically access its custom attributes via `order.note_attributes.color`, or via `order.note_attributes[0].value`. This dynamic behavior is lost if the `event` variable is stubbed out, which can result in behaviors that are difficult to diagnose.

## Configuration

Preview events may be defined using the "Edit preview events" button, in the task preview pane.

The configuration area for preview events contains a quickstart link for each event topic the task subscribes to, allowing developers to get started using sample data if the event topic is known to Mechanic. Or, the developer may start with a blank preview event definition, filling in whatever topic and data are useful.

A developer may define any number of preview events per topic. If no preview events are defined for a given topic, Mechanic will construct its own ad-hoc event during preview.

### Properties

#### Description

Displayed beneath the event topic in the preview pane, allowing the developer to distinguish one scenario from another.

#### Topic

Identifies the event definition to Mechanic, when Mechanic goes to construct preview events by topic.

#### Data

Used to construct `event.data`, and may be set to whatever values are useful in representing a specific scenario. The data structures used here should resemble what Mechanic will receive for a live event of the same topic.

Notably, the data here _can_ be limited to just the properties that are useful. In the example below, defining two preview events for shopify/customers/create, we limit the event data to just the email address.

### Example

For a trivial task, subscribing to shopify/customers/create, and having the following task code...

```
{% raw %}
{% if customer.email contains "gmail.com" %}
  {% log message: "got a gmail user!", email: customer.email %}
{% else %}
  {% log message: "got someone else!", email: customer.email %}
{% endif %}
{% endraw %}
```

... we define two preview events, one which represents a Gmail user, and one which does not. This allows us to easily assert that the task behaves properly in both scenarios.

![](<../../../.gitbook/assets/2022-04-01 17.23.24.gif>)

## Versioning

Preview event definitions are stored along with the task itself, and thus are present in the tasks version history (and, naturally, in task exports).

Because definitions are a part of the task itself, they're appropriate for use as a testing tool, allowing the developer to verify that a task behaves as intended at every stage of the task's development.
