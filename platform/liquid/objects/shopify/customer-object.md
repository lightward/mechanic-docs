# 🚫 Customer object

{% hint style="danger" %}
**Important Notice**

Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).

Use the [GraphQL](../../../../core/actions/integrations/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:

Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

## How to access it

* Use `{{ customer }}`  in tasks responding to shopify/customers events
* Look up specific customers by their ID, using `{{ shop.customers[12345678900] }}`
* Look up specific customers by their email address, using `{{ shop.customers["test@example.com"] }}`

## What it contains

* [Every property from the Customer resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/customers/customer#properties)
  * Note: Shopify delivers `customer.tags` as a comma-delimited string, _not_ an array of strings!
  * Note: As of version 2025-01, Shopify no longer includes customer tags in webhook payloads. In cases of tasks responding to webhooks that include customer payloads, Mechanic does the background querying necessary to support direct references to `customer.tags`.
* `{{ customer.account_activation_url }}`, containing the Shopify-hosted URL where the customer can create a password for their account
* `{{ customer.unsubscribe_url }}`, containing the Mechanic-hosted URL where the customer can mark their own customer account as not accepting marketing; see [How do I add an unsubscribe link to my emails?](../../../../faq/how-do-i-add-an-unsubscribe-link-to-my-emails.md)
* The related [metafields object](metafields/metafield-collection.md): `{{ customer.metafields }}`
