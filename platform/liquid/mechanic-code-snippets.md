---
description: A list of convenience snippets provided in the Mechanic code editor.
icon: person-running-fast
---

# Mechanic code snippets

### `bulk_operation_query` - GraphQL bulk operation query template <a href="#bulk_operation_query" id="bulk_operation_query"></a>

This snippet expands to provide a complete template for initiating a bulk operation query and responding to the results (see [bulk-operations.md](../../core/shopify/read/bulk-operations.md "mention")). Use it from the top level of a new task after defining the task option fields.

Once the snippet is selected, you will be prompted to select a resource name, which must be in camel case (e.g. `productVariant`). You can type in your own resource name if it doesn't appear in the list. This value will be propagated throughout most of the snippet block.

Hitting `tab` after choosing the initial resource name, you will again be prompted to choose a resource name, this time to be used within the sample JSONL preview block and the resource assignment by `__typename` afterwards. This resource name needs to be in Pascal case to align with Shopify resource identifiers (e.g. `ProductVariant`).

<figure><img src="../../.gitbook/assets/2024-09-06 16.00.07.gif" alt=""><figcaption></figcaption></figure>

***

### `gid` -  Shopify global ID <a href="#gid" id="gid"></a>

Expands inline to provide an example [gid](https://shopify.dev/docs/api/usage/gids) with the resource object you choose. You may also type your resource object if there is not a match in the list.

This snippet is most useful when generating stub/preview data for a GraphQL query.

<figure><img src="../../.gitbook/assets/2024-09-06 16.01.34.gif" alt=""><figcaption></figcaption></figure>

***

### `object_query` - GraphQL object query <a href="#object_query" id="object_query"></a>

This snippet can be used when you want to query a single top-level resource in GraphQL.

Once the snippet is selected, you will be prompted to select a resource name, which must be in camel case (e.g. `productVariant`). You can type in your own resource name if it doesn't appear in the list. This value will be propagated throughout most of the snippet block.

Hitting `tab` after choosing the initial resource name, you will again be prompted to choose a resource name, this time only for use within the sample preview block. This resource name needs to be in Pascal case to align with Shopify resource identifiers (e.g. `ProductVariant`).

<figure><img src="../../.gitbook/assets/2024-09-06 16.02.37.gif" alt=""><figcaption></figcaption></figure>

***

### `paginated_query` - GraphQL paginated query <a href="#paginated_query" id="paginated_query"></a>

Use this snippet to provide a paginated query for a resource within a for loop block. It will prompt you to choose the resource type.

<figure><img src="../../.gitbook/assets/2024-09-06 16.03.38.gif" alt=""><figcaption></figcaption></figure>
