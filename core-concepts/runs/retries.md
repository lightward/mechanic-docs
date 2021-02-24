# Retries

Runs are given **automatic retries** when a non-permanent error is encountered. In some cases, Mechanic permits **manual retries** for runs, allowing users to reset a run's results and perform the run again.

When a run is retried, any and all previous results for that specific run are lost.

## Automatic retries

When non-permanent errors are encountered, Mechanic will automatically retry a run. For [HTTP actions](../actions/action-types/http.md), this might be a connection error. For [Email actions](../actions/action-types/email.md), this might be a temporary outage with our email provider.

Mechanic will automatically retry these runs up to 4 times, for a total of 5 attempts. Retries are subject to a variable backoff delay, of approximately 0:30, 1:16, 2:32, and 5:08 respectively, for each of the 4 retries.

## Manual retries

Some task runs may be manually retried, via the Mechanic user interface.

### Task runs

Task runs may be retried...

* ... if the task run itself failed \(due to a Liquid error, an API error while reading data, or something else\)
* ... or, if the task run did not generate any actions

Retried task runs will always use the latest version of a task's code, but are pinned to the [Shopify API version](../tasks/shopify-api-version.md) configured at the time the run was created.

{% hint style="info" %}
During task development, it can be useful to set up a task to only render [log objects](../tasks/code/logging.md). A task run which only rendered log objects can be retried, and this ability to retry can be convenient when rapidly iterating on task code.
{% endhint %}

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e1ae1f004286364bc93c3b2/5e1ae18594370.png)

### Action runs

Only failed action runs may be retried.

Retried action runs will use their original action options. They are entirely unaffected by updates to the task that generated them.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e1ae1f02c7d3a7e9ae61302/5e1ae185f0144.png)

