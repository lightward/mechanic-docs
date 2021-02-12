# Shopify API version

Each task is configured with a [**Shopify API version**](https://shopify.dev/concepts/about-apis/versioning), defaulting to the latest version at the time of the task's creation.

This version is used in all activity related to the current task, including:

* REST API calls performed to support Liquid lookups
* GraphQL calls performed by [the shopify Liquid filter](../../liquid/mechanic-liquid-wd40/filters/shopify.md)
* All Shopify API calls performed by [the Shopify action](../actions/action-types/shopify.md), including [bulk operations]()

## Changing versions

The selector for a task's Shopify API version is available towards the end of the task's advanced settings.

![](../../.gitbook/assets/2021-02-09-16.48.53.gif)

{% hint style="info" %}
Changing a task's Shopify API version only affects task and action runs that are created after the change. Each run is pinned to the Shopify API version that was active at the time the run was created; [retrying](../runs/retries.md) a run does not change its version.
{% endhint %}

## Automatic version upgrades

Shopify supports each version for 12 months. 30 days before a task's version becomes unsupported, Mechanic will automatically begin calling the closest supported version instead.

