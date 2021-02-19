# Pauses

In some cases, Mechanic will **pause** runs immediately upon their creation. A paused run will display the pause icon in the event inspector, along with an explanation for the pause. A paused run can be performed on demand.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5eb4421d2c7d3a5ea54ab0b0/file-gWtSJIESCx.png)

## Reasons for pauses

### Action run sequences

Tasks may be configured to [perform action runs in sequence](../tasks/advanced-settings/perform-action-runs-in-sequence.md). When this setting is active for a task, action runs waiting their turn will be paused, and automatically resumed in turn.

### Shopify update loops

Mechanic detects scenarios in which a task continuously attempts to apply the same update to the same Shopify resource. When an update is performed, Shopify follows up with an "update" event for that resource; if the task attempts to perform the same update again, Mechanic will pause the Shopify action attempting the update.

