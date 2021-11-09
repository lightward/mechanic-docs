# Liquid objects

Mechanic-flavored Liquid comes with a complement of [Liquid objects](../../../platform/liquid/objects/), each of which is tied to a resource in the [Shopify Admin REST API](https://shopify.dev/docs/admin-api/rest). Many objects support access to related objects via lookups (e.g. `{{ shop.customers[customer_id].orders.first }}`); in this way, the REST API can be traversed by resource.

Access to these Liquid objects varies, based on the context in which Liquid is rendered. For example, a task that subscribes to shopify/customers/create will have access to the [Customer](../) object in its code, via a variable called `customer`. To learn more about how these objects are made available to task code, see [environment variables](../../tasks/code/environment-variables.md).

{% hint style="warning" %}
**Shopify variables in Mechanic do not necessarily contain the same attributes as Liquid variables used in Shopify (in places like themes or email templates) – even if they share the same name.**

In Mechanic, Shopify variables always contain data from Shopify events, which are delivered to Mechanic via webhook. This means that Shopify variables always have the same data structure as Shopify webhooks, corresponding to Shopify's REST representation for this data.

For example, while Shopify themes support `{{ customer.name }}`, Mechanic does not (because [Shopify's REST representation of the customer resource](https://shopify.dev/docs/admin-api/rest/reference/customers/customer) does not contain a "name" property). On the other hand, Mechanic supports `{{ customer.created_at }}`, while Shopify themes do not.
{% endhint %}

## Usage

Each task is given a set of [environment variables](../../tasks/code/environment-variables.md) to work with, out of the box. Mechanic's task code editor will tell you which ones are available. For example, for a task responding to a `shopify/orders/` event, you might see this:

![](https://s3.amazonaws.com/helpscout.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e12c7d3a7e9ae69bd2/5e28a1e1a248a.png)

The [cache](../../../platform/liquid/objects/cache.md), [event](../../../platform/liquid/objects/event.md), [options](../../../platform/liquid/objects/options.md), and [shop](../../../platform/liquid/shopify/shop.md) objects are always available for tasks; the [order](../../../platform/liquid/shopify/order.md) object (as in this example) contains the order to which the current event relates.

Use [Mechanic's Liquid object documentation](../../../platform/liquid/objects/) to discover what data is available for each Liquid object.

## Use Liquid objects when...

* ... your task's event gives you an [environment variable](../../tasks/code/environment-variables.md) containing the data you need. For example, for a task responding to a "shopify/customers/" event, you'll get an automatic `customer` variable. Feel free to use this variable to get to additional data, like `customer.orders.first.name`.
* ... you know you're not going to need to load an enormous amount of data. For example, a `{% for customer in shop.customers %}` loop is just fine if you know your store will have only hundreds or thousands of customers.
* ... when it's easy to get to the right data, allowing future versions of you to easily understand what you were doing. ;) There are plenty of scenarios where it's easier to use Liquid objects than it is to use GraphQL, and if you can do so without accidentally downloading too much data (see above), go for it.

## Don't use Liquid objects when...

* ... there's a more efficient way to get to what you need. For example, getting all orders tagged "sale" via Liquid objects will require loading in _all_ orders, and then using Liquid to skip orders that don't have that tag. This takes a long time, _and_ it's unnecessary, because GraphQL supports searching for orders by tag.
