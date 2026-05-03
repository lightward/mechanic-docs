# 🚫 Customer object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
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
