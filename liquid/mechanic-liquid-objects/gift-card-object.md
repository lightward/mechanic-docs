# Gift card object

Note: This API is only available to Shopify Plus stores, who have configured their Mechanic account with [a custom Shopify API password](https://help.usemechanic.com/en/articles/3486326-using-a-custom-shopify-api-password).

## How to access it

* Use `{% for gift_card in shop.gift_cards %}` to loop through all gift cards
* Use `{% for gift_card in shop.gift_cards.enabled %}` to loop through all enabled gift cards
* Use `{% for gift_card in shop.gift_cards.disabled %}` to loop through all disabled gift cards
*  Use `{{ shop.gift_cards[1234567890] }}` to retrieve a single gift card, by ID

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/plus/giftcard)
* The related [order object](https://help.usemechanic.com/en/articles/2365693-the-order-object): `{{ gift_card.order }}` 
* The related [customer object](https://help.usemechanic.com/en/articles/2365717-the-customer-object): `{{ gift_card.customer }}`

