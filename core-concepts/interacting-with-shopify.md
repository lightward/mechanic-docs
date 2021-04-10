# Interacting with Shopify

Most Mechanic tasks respond to Shopify events, read

## Responding to Shopify events

Shopify uses webhooks to notify apps like Mechanic about new activity. Mechanic supports every type of Shopify webhook in its set of [Shopify event topics](events/event-topic-reference/shopify.md). By setting up [subscriptions](tasks/subscriptions.md) to these topics, a task may respond to any supported type of Shopify activity.

### Responding to changes in specific data

Shopify's "update" webhooks do not contain information about what piece of data has changed. \(For example, a product update webhook does not specify what attribute of the product has changed.\) For this reason, it's not possible to subscribe to changes in specific resource attributes \(like product SKUs, or order tags\).

If a task needs to react to a specific attribute change, the task must scan for and "remember" the original value of that attribute, so as to compare incoming updates with that remembered value. A task could use the [Cache](actions/action-types/cache.md) action to store these values in the Mechanic cache, or it could use the [Shopify](actions/action-types/shopify.md) action to save the remembered value in a metafield.

For an example implementation, see the [Auto-tag products when their variants change](https://usemechanic.com/task/auto-tag-products-when-their-skus-change) task.

## Reading Shopify data

Mechanic supports three methods of reading from the Shopify Admin API.

### GraphQL via Liquid filter

Tasks may use the [shopify](../liquid/filters.md#shopify) Liquid filter to convert GraphQL query strings into simple result objects, by sending the query to the [Shopify GraphQL Admin API](https://shopify.dev/docs/admin-api/graphql). The easiest way to build these queries is via the [Shopify Admin API GraphiQL explorer](https://shopify.dev/tools/graphiql-admin-api), which allows queries to be interactively constructed.

Note that this Liquid filter does not support running mutations \(i.e. writing Shopify data via GraphQL\).

### REST via Liquid objects

Mechanic-flavored Liquid comes with a complement of [Liquid objects](../liquid/mechanic/objects/), each of which is tied to a resource in the [Shopify Admin REST API](https://shopify.dev/docs/admin-api/rest). Many objects support access to related objects via lookups \(e.g. `{{ shop.customers[customer_id].orders.first }}`\); in this way, the REST API can be traversed by resource.

Access to these Liquid objects varies, based on the context in which Liquid is rendered. For example, a task that subscribes to shopify/customers/create will have access to the [Customer](interacting-with-shopify.md) object in its code, via a variable called `customer`. To learn more about how these objects are made available to task code, see [Liquid variables](tasks/code/environment-variables.md).

{% hint style="warning" %}
**Shopify variables in Mechanic do not necessarily contain the same attributes as Liquid variables used in Shopify \(in places like themes or email templates\) – even if they share the same name.**

In Mechanic, Shopify variables always contain data from Shopify events, which are delivered to Mechanic via webhook. This means that Shopify variables always have the same data structure as Shopify webhooks, corresponding to Shopify's REST representation for this data.

For example, while Shopify themes support `{{ customer.name }}`, Mechanic does not \(because [Shopify's REST representation of the customer resource](https://shopify.dev/docs/admin-api/rest/reference/customers/customer) does not contain a "name" property\). On the other hand, Mechanic supports `{{ customer.created_at }}`, while Shopify themes do not.
{% endhint %}

### GraphQL or REST via the Shopify action

In rare cases, it may be necessary to make a call to the Shopify API that cannot be expressed via the [shopify](../liquid/filters.md#shopify) Liquid filter, or via Liquid object lookups. In these cases, the [Shopify](actions/action-types/shopify.md) action can be used

## Writing Shopify data

