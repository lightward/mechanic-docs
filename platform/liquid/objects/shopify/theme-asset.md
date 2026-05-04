# 🚫 Theme asset object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## How to access it

* Look up specific a specific asset by its ID, using `{{ shop.themes[12345].assets[67890] }}`&#x20;
* Loop through all assets in a theme: `{% for asset in shop.themes[12345]assets %}`

## What it contains

* [Every property from the Asset resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/online-store/asset)
