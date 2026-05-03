# 🚫 Theme object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## How to access it

* Use `{{ theme }}` in tasks responding to shopify/themes events
* Look up specific themes by their ID, using `{{ shop.themes[12345678900] }}`&#x20;
* Loop through all themes: `{% for theme in shop.themes %}`

## What it contains

* [Every property from the Theme resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/online-store/theme)
* An array of [asset objects](theme-asset.md): `{{ theme.assets }}`
