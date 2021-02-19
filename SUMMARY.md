# Table of contents

* [Welcome!](README.md)

## Getting Started

* [Introduction](getting-started/overview.md)
* [Resources](getting-started/slack-community.md)

## Core Concepts

* [Events](core-concepts/events/README.md)
  * [Event topics](core-concepts/events/event-topic-reference/README.md)
    * [Mechanic](core-concepts/events/event-topic-reference/mechanic.md)
    * [Shopify](core-concepts/events/event-topic-reference/shopify.md)
    * [User](core-concepts/events/event-topic-reference/user.md)
    * [Third-party](core-concepts/events/event-topic-reference/third-party.md)
  * [Parent and child events](core-concepts/events/parent-and-child-events.md)
  * [Event filters](core-concepts/events/event-filters/README.md)
    * [Debouncing events](core-concepts/events/event-filters/debouncing-events.md)
* [Tasks](core-concepts/tasks/README.md)
  * [Subscriptions](core-concepts/tasks/subscriptions.md)
  * [Code](core-concepts/tasks/code/README.md)
    * [Liquid variables](core-concepts/tasks/code/liquid-variables.md)
    * [Action objects](core-concepts/tasks/code/action-objects.md)
    * [Error objects](core-concepts/tasks/code/error-objects.md)
    * [Log objects](core-concepts/tasks/code/logging.md)
  * [Options](core-concepts/tasks/options.md)
  * [Previews](core-concepts/tasks/previews/README.md)
    * [Stub data](core-concepts/tasks/previews/stub-data.md)
  * [Shopify API version](core-concepts/tasks/shopify-api-version.md)
  * [Advanced settings](core-concepts/tasks/advanced-settings/README.md)
    * [Documentation](core-concepts/tasks/advanced-settings/documentation.md)
    * [JavaScript](core-concepts/tasks/advanced-settings/javascript.md)
  * [Import and export](core-concepts/tasks/import-and-export.md)
* [Actions](core-concepts/actions/README.md)
  * [Action types](core-concepts/actions/action-types/README.md)
    * [Cache](core-concepts/actions/action-types/cache.md)
    * [Echo](core-concepts/actions/action-types/echo.md)
    * [Email](core-concepts/actions/action-types/email.md)
    * [Event](core-concepts/actions/action-types/event.md)
    * [Files](core-concepts/actions/action-types/files.md)
    * [FTP](core-concepts/actions/action-types/ftp.md)
    * [HTTP](core-concepts/actions/action-types/http.md)
    * [Shopify](core-concepts/actions/action-types/shopify.md)
  * [File generators](core-concepts/actions/file-generators/README.md)
    * [Base64](core-concepts/actions/file-generators/base64.md)
    * [PDF](core-concepts/actions/file-generators/pdf.md)
    * [URL](core-concepts/actions/file-generators/url.md)
    * [ZIP](core-concepts/actions/file-generators/zip.md)
* [Runs](core-concepts/runs/README.md)
  * [Retries](core-concepts/runs/retries.md)
* [REST](core-concepts/rest.md)
* [Integrations](core-concepts/integrations.md)

## Liquid

* [Basics](liquid/basics/README.md)
  * [Comments](liquid/basics/comments.md)
  * [Variables](liquid/basics/variables.md)
  * [Types](liquid/basics/types.md)
  * [Comparisons](liquid/basics/comparison-operators.md)
  * [Conditionals](liquid/basics/conditional-statements.md)
  * [Iteration](liquid/basics/iteration.md)
  * [Filters](liquid/basics/basic-liquid-filters.md)
  * [Whitespace](liquid/basics/whitespace.md)
* [Mechanic Liquid](liquid/mechanic-liquid/README.md)
  * [Arrays and Hashes](liquid/mechanic-liquid/arrays-and-hashes.md)
  * [Filters](liquid/mechanic-liquid/filters/README.md)
    * [add\_tag, add\_tags, remove\_tag, remove\_tags](liquid/mechanic-liquid/filters/add_tag-add_tags-remove_tag-remove_tags.md)
    * [base64, decode\_base64](liquid/mechanic-liquid/filters/decode_base64.md)
    * [browser](liquid/mechanic-liquid/filters/browser.md)
    * [date](liquid/mechanic-liquid/filters/date.md)
    * [parse\_json](liquid/mechanic-liquid/filters/parse_json.md)
    * [shopify](liquid/mechanic-liquid/filters/shopify.md)
  * [Tags](liquid/mechanic-liquid/tags/README.md)
    * [assign](liquid/mechanic-liquid/tags/assign.md)

## GraphQL

* [Basics](graphql/basics/README.md)
  * [Queries](graphql/basics/queries.md)
  * [Mutations](graphql/basics/mutations.md)
* [Bulk Operations](graphql/bulk-operations.md)

## Advanced Topics

* [Contributing to the Task Library](advanced-topics/contributing-to-the-task-library.md)
* [Email Templates](advanced-topics/email-templates.md)
* [Custom Email Domain](advanced-topics/custom-email-domain.md)
* [Mechanic Webhooks](advanced-topics/webhooks.md)
* [External APIs](advanced-topics/external-apis.md)
* [Mechanic Cache](advanced-topics/mechanic-cache.md)
* [Cache Endpoints](advanced-topics/cache-endpoints.md)
* [CORS](advanced-topics/cors.md)

## FAQ

* [Can I query external APIs?](faq/can-i-query-external-apis.md)

## Tutorials <a id="developer-tutorials"></a>

* [How to Write a Custom Task](developer-tutorials/how-to-write-a-custom-task.md)
* [Working with Bulk Operations](developer-tutorials/working-with-bulk-operations.md)
* [Working with Metafields](developer-tutorials/untitled-1.md)
* [Tag Orders Using GraphQL](developer-tutorials/tag-orders-using-graphql.md)
* [Trigger a Task From a Contact Form](developer-tutorials/trigger-a-task-from-a-contact-form.md)

## Video Tutorials

* [Creating products in bulk](video-tutorials/creating-products-in-bulk.md)
* [Auto-tag customers by sales channel](video-tutorials/auto-tag-customers-by-sales-channel.md)
* [Send an email when a product's price goes below its cost](video-tutorials/send-an-email-when-a-products-price-goes-below-its-cost.md)
* [Send recurring reminders about unpaid orders](video-tutorials/send-recurring-reminders-about-unpaid-orders.md)
* [Send an email when a specific product is shipped](video-tutorials/send-an-email-when-a-specific-product-is-shipped.md)
* [Delete all orders](video-tutorials/delete-all-orders.md)
* [Adding an optional time delay to your Mechanic task](video-tutorials/adding-an-optional-time-delay-to-your-mechanic-task.md)
* [Email the customer when tracking numbers are added to their order](video-tutorials/email-the-customer-when-tracking-numbers-are-added-to-their-order.md)
* [Upgrading a Mechanic task: Adding a time delay](video-tutorials/upgrading-a-mechanic-task-adding-a-time-delay.md)
* [Email a report of customers who haven't ordered in X days](video-tutorials/email-a-report-of-customers-who-havent-ordered-in-x-days.md)
* [Auto-publish new products](video-tutorials/auto-publish-new-products.md)
* [Auto-tag products when their SKU\(s\) change](video-tutorials/auto-tag-products-when-their-sku-s-change.md)
* [Sync inventory for shared SKUs](video-tutorials/sync-inventory-for-shared-skus.md)
* [Auto-tag orders with their tracking numbers](video-tutorials/auto-tag-orders-with-their-tracking-numbers.md)
* [Maintain a tag for orders processed today](video-tutorials/maintain-a-tag-for-orders-processed-today.md)
* [Auto-tag orders by originating staff member](video-tutorials/auto-tag-orders-by-originating-staff-member.md)

---

* [Under construction](under-construction.md)

