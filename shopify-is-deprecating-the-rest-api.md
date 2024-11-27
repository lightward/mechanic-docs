# 📣 Shopify is deprecating the REST API

Shopify is evolving its platform to enhance performance and provide more powerful features. As part of this evolution, Shopify has announced the deprecation of the Shopify Admin REST API. In response, we are updating our services to align with this change, transitioning fully to the GraphQL Admin API.

## What Is Changing?

* **Deprecation of REST Admin API**: Starting October 1, 2024, the REST Admin API is considered a legacy API. Shopify will begin phasing it out, with critical endpoints like product and variant endpoints ceasing to function on February 1, 2025. While product and variant endpoints are the first to be deprecated, Shopify plans to deprecate all REST endpoints in the future.
* **GraphQL Admin API**: All new developments and updates will utilize the GraphQL Admin API exclusively. This shift is aimed at leveraging the enhanced capabilities and efficiencies that GraphQL offers.

## Why is Shopify Making this Change?

Shopify wants to maintain one API and have decided to discontinue the REST Admin API in favor of the GraphQL Admin API. GraphQL addresses several limitations of REST, offering significant benefits such as:

* **Efficient Data Retrieval**: Fetch precisely the data you need in a single request.
* **Enhanced Flexibility**: Customize queries to suit your specific needs.

## How Does This Affect You?

* **Library Tasks Update**: All our library tasks will be updated to use GraphQL only. You will get a notification via email and in app if there is an update available for a task you have installed. Tasks that can be auto-updated will be updated for you. These updated tasks will serve as examples to help you migrate your custom tasks.
* **Custom Tasks Migration**: If you have custom tasks relying on the REST API, they will need to be migrated to use the GraphQL API before the deprecation deadlines. See our guide [here](resources/converting-tasks-from-shopify-rest-to-graphql/).
* **Action Required**: To ensure uninterrupted service, please begin updating your custom tasks to GraphQL as soon as possible.

### Specific Impacts

#### After February 1, 2025:

* Product and Variant REST liquid objects will cease functioning
* [Admin Links](core/shopify/admin-action-links.md) for Product and Variants will now pass resource IDs instead of full REST objects - tasks will need to look up objects using these IDs
* REST lookups in email task options will no longer work
* Any tasks using REST API for product/variant operations will stop working

**We recommend migrating all tasks to GraphQL now, not just those affected by the February deadline, as all REST endpoints will eventually be deprecated.**

#### Next Steps

1. **Review Our Migration Guides**: We have prepared comprehensive guides to assist you in migrating your custom tasks to GraphQL. Access them [here](resources/converting-tasks-from-shopify-rest-to-graphql/).
2. **Update Tasks from the Library**: You will get a notification via email and in app if there is an update available for a task you have installed. Tasks that can be auto-updated will be updated for you.
3. **Learn More About GraphQL**: Familiarize yourself with GraphQL by visiting Shopify's official [migration guide](https://shopify.dev/docs/apps/build/graphql/migrate).

#### Getting Help

* **Hire a Partner**: Browse our [partner directory](https://partners.mechanic.dev/) for expert help
* **Community Support**: Join our [Slack community](resources/slack.md) to ask questions about migrating your tasks

