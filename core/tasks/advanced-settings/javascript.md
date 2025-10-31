# JavaScript

Shopify allows apps to inject JavaScript into the online storefront. (This is facilitated by [ScriptTag](https://shopify.dev/docs/admin-api/rest/reference/online-store/scripttag) in the Shopify API.)

Mechanic supports this by allowing each task to specify its own JavaScript, to be injected into the online storefront.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Here, the developer can add in their own JavaScript code, taking advantage of Liquid for mixing in data from the current store, or from the current task's options.

{% hint style="info" %}
A task's JavaScript content only has access to to the `shop` and `options` Liquid variables. The rendering context is similar to that of [task subscriptions](../subscriptions.md#using-liquid); Liquid code here does not have access to any data related to events, and cannot dynamically respond to any information about the visitor's current request.
{% endhint %}

![](<../../../.gitbook/assets/Screen Shot 2022-04-01 at 7.25.02 PM.png>)
