# Practicing writing tasks

In our own internal education, we've found that the following exercises work particularly well. They're all in sequence – the task to create for each subsequent exercise modifies the code you wrote previously.

{% hint style="info" %}
Working on getting better at task-writing? See [Writing a high-quality task](../../techniques/writing-a-high-quality-task.md).
{% endhint %}

## Assignments

* [ ] 1\. Auto-tag customers with @gmail.com email addresses, with "gmail"
  * Configure the task's event subscriptions appropriately
  * Support case-insensitivity – recognize "@gmail.com", _and_ "@Gmail.com"
  * Ignore domains with more extensions – don't tag for "@gmail.com.au"
  * Make sure that any existing tags on the customer's account are kept, not lost
  * Use the REST API for this operation
  * Use [a static preview action](../../core/tasks/previews/#static-preview-actions), to show the merchant a preview of what the task will do
* [ ] 2\. Move to [a dynamic preview action](../../core/tasks/previews/#dynamic-preview-actions), using stub data
* [ ] 3\. Remove the stub data, and add two [event preview definitions](../../core/tasks/previews/events.md): one for a @gmail.com address, and one for another address
* [ ] 4\. Allow the merchant to configure the domain name to look for, and the tag to apply
  * Help the merchant make sure they enter a real domain name – return an error if the domain doesn't include a "."
  * Support case-insensitivity – if the merchant enters "Gmail.com", and the customer's email address ends with "@GMAIL.COM", they should still be tagged
* [ ] 5\. Allow the merchant to add any number of domain name and customer tag pairings
* [ ] 6\. Support responding to customer updates, in which the customer's email address changes
  * Remove any domain name tags that do not apply, and add the tag (if any that does apply
  * Do _not_ remove any tags that contain a domain name tag – if the tag "google" should be removed, do not accidentally remove "google-foo"
  * Make this optional – allow the merchant to choose whether or not Mechanic listens for this
* [ ] 7\. Move to GraphQL for removing and adding tags
* [ ] 8\. Allow waiting a configurable number of minutes
  * Account for the customer tags or email address having changed during the waiting period
* [ ] 9\. Create a backfill mode for processing all existing customers, that the merchant can run manually
  * Use `{% for customer in shop.customers %}` for this operation
  * Note: this technique is useful for [reconciling missing events](../../core/shopify/events/reconciling-missing-events.md)
* [ ] 10\. Move to GraphQL for scanning customers
  * See [Querying Shopify](../../core/shopify/read/) to learn about looping through results using cursors
* [ ] 11\. Move to GraphQL bulk operations for scanning customers
