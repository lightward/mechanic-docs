# ⚠️ Liquid objects

Mechanic-flavored Liquid comes with a complement of [Liquid objects](../../../platform/liquid/objects/), each of which is tied to a resource in the [Shopify Admin REST API](https://shopify.dev/docs/admin-api/rest). Many objects support access to related objects via lookups (e.g. `{{ shop.customers[customer_id].orders.first }}`); in this way, the REST API can be traversed by resource.

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating some of the Shopify Admin REST API. The first round of deprecations involve the product and variant endpoints. Read the deprecation notice [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing). \
\
Our recommendation is to use [GraphQL](../../actions/shopify.md#graphql) going forward. The [product](../../../platform/liquid/objects/shopify/product.md) and [variant](../../../platform/liquid/objects/shopify/variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. It appears that Shopify will gradually phase out the REST API over time.

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:
{% endhint %}

Access to these Liquid objects varies, based on the context in which Liquid is rendered. For example, a task that subscribes to shopify/customers/create will have access to the [Customer](../) object in its code, via a variable called `customer`. To learn more about how these objects are made available to task code, see [environment variables](../../tasks/code/environment-variables.md).

{% hint style="warning" %}
**Shopify variables in Mechanic do not necessarily contain the same attributes as Liquid variables used in Shopify (in places like themes or email templates) – even if they share the same name.**

In Mechanic, Shopify variables always contain data from Shopify events, which are delivered to Mechanic via webhook. This means that Shopify variables always have the same data structure as Shopify webhooks, corresponding to Shopify's REST representation for this data.

For example, while Shopify themes support `{{ customer.name }}`, Mechanic does not (because [Shopify's REST representation of the customer resource](https://shopify.dev/docs/admin-api/rest/reference/customers/customer) does not contain a "name" property). On the other hand, Mechanic supports `{{ customer.created_at }}`, while Shopify themes do not.
{% endhint %}

## Usage

Each task is given a set of [environment variables](../../tasks/code/environment-variables.md) to work with, out of the box. Mechanic's task code editor will tell you which ones are available. For example, for a task responding to a `shopify/orders/` event, you might see this:

![](https://s3.amazonaws.com/helpscout.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e12c7d3a7e9ae69bd2/5e28a1e1a248a.png)

The [cache](../../../platform/liquid/objects/cache.md), [event](../../../platform/liquid/objects/event.md), [options](../../../platform/liquid/objects/options.md), and [shop](../../../platform/liquid/objects/shopify/shop.md) objects are always available for tasks; the [order](../../../platform/liquid/objects/shopify/order.md) object (as in this example) contains the order to which the current event relates.
