# Customer object

## How to access it

* Use `{{ customer }}`  in tasks responding to shopify/customers events
* Look up specific customers by their ID, using `{{ shop.customers[12345678900] }}`
* Look up specific customers by their email address, using `{{ shop.customers["test@example.com"] }}`

## What it contains

* [Every property from the Customer resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/customers/customer#properties) \(**warning: Shopify delivers `customer.tags` as a comma-delimited string,** _**not**_ **an array of strings!**\)
* `{{ customer.account_activation_url }}`, containing the Shopify-hosted URL where the customer can create a password for their account
* `{{ customer.unsubscribe_url }}`, containing the Mechanic-hosted URL where the customer can mark their own customer account as not accepting marketing; see [How do I add an unsubscribe link to my emails?](../../../../faq/how-do-i-add-an-unsubscribe-link-to-my-emails.md)
* The related [metafields object](metafields.md): `{{ customer.metafields }}`

