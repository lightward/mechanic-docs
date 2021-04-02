# Theme asset object

##  How to access it

* Look up specific a specific asset by its ID, using `{{ shop.themes[12345].assets[67890] }}` 
* Loop through all assets in a theme: `{% for asset in shop.themes[12345]assets %}`

## What it contains

* [Every property from the Shopify API](https://shopify.dev/docs/admin-api/rest/reference/online-store/asset)

