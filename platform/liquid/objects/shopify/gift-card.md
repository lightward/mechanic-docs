# 🚫 Gift card object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/integrations/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

Note: This API is only available to Shopify Plus stores, who have configured their Mechanic account with [a custom Shopify API password](https://help.usemechanic.com/en/articles/3486326-using-a-custom-shopify-api-password).

## How to access it

* Use `{% for gift_card in shop.gift_cards %}` to loop through all gift cards
* Use `{% for gift_card in shop.gift_cards.enabled %}` to loop through all enabled gift cards
* Use `{% for gift_card in shop.gift_cards.disabled %}` to loop through all disabled gift cards
* Use `{{ shop.gift_cards[1234567890] }}` to retrieve a single gift card, by ID

## What it contains

* [Every property from the Gift Card resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/plus/giftcard)
* The related [order object](order.md): `{{ gift_card.order }}`&#x20;
* The related [customer object](customer-object.md): `{{ gift_card.customer }}`
