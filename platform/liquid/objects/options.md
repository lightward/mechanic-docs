# Options object

Only available within a task, the `options` object contains all values provided by merchants while configuring the task.

Each option value is _itself_ parsed for Liquid. The `options` variable is not available during this pass. :\)

## How to access it

Use `{{ options }}` or `{{ options.foobar }}` in a task's...

* Liquid event subscriptions \(see [Task subscriptions](../../../core/tasks/subscriptions.md)\)
* script
* online store JavaScript
* order status JavaScript

  For more discussion on using this object, see [Task options](../../../core/tasks/options/).

## What it contains

This object is always a hash. The keys always correspond to lookups \(i.e. `{{ options.foobar__required }}`\) in the task script \(and elsewhere\). The values are configured by the user, using a form that's automatically generated by Mechanic.

For more on using this object \(and implicitly constructing the merchant-facing configuration form\), see [Task options](../../../core/tasks/options/).
