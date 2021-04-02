# Gift card object

Note: This API is only available to Shopify Plus stores, who have configured their Mechanic account with [a custom Shopify API password](https://help.usemechanic.com/en/articles/3486326-using-a-custom-shopify-api-password).

## How to access it

* Use `{% for gift_card in shop.gift_cards %}` to loop through all gift cards
* Use `{% for gift_card in shop.gift_cards.enabled %}` to loop through all enabled gift cards
* Use `{% for gift_card in shop.gift_cards.disabled %}` to loop through all disabled gift cards
*  Use `{{ shop.gift_cards[1234567890] }}` to retrieve a single gift card, by ID

## What it contains

* [Every property from the Gift Card resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/plus/giftcard)
* The related [order object](../order-object.md): `{{ gift_card.order }}` 
* The related [customer object](customer-object.md): `{{ gift_card.customer }}`



