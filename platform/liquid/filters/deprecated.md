# Deprecated filters

These filters are still a part of Mechanic Liquid, but are no longer documented or supported.

* add\_tag, add\_tags, remove\_tag, remove\_tags
  * These filters were conveniences from a time when resource tagging exclusively took place via the Shopify Admin REST API, which treats sets of tags as a single comma-delimited string
  * Instead, use the Shopify Admin GraphQL API for iterating on and manipulating tags as individual single-tag strings
* img\_url
  * Based on Shopify Liquid's now-unsupported [img\_url](https://www.shopify.com/partners/blog/img-url-filter) filter
* base64
  * Replaced by [base64\_encode](https://shopify.dev/docs/api/liquid/filters/base64\_encode)
* decode\_base64
  * Replaced by [base64\_decode](https://shopify.dev/docs/api/liquid/filters/base64\_decode)
* base64url
  * Replaced by [base64\_url\_safe\_encode](https://shopify.dev/docs/api/liquid/filters/base64\_url\_safe\_encode)
* decode\_base64url
  * Replaced by [base64\_url\_safe\_decode](https://shopify.dev/docs/api/liquid/filters/base64\_url\_safe\_decode)
