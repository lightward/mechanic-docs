# Webhooks

A Mechanic **webhook** allows data to be submitted directly to Mechanic, resulting in a new [**event**](../core/events/) having a particular topic, containing the submitted data. Webhooks are called with simple HTTP POST requests, which means they can be called from any programming language, and from many applications.

{% hint style="info" %}
This article is an introduction to webhooks. To get started quickly, see [Creating a Mechanic webhook](../resources/tutorials/creating-a-mechanic-webhook.md).
{% endhint %}

## Configuration

A Mechanic webhook is configured with a specific event topic, and is assigned a unique, webhook-specific URL. Any valid POST request to that webhook URL will result in a new event being created, having the configured topic, containing the parsed data from the POST request.

Any request using an HTTP method other than POST will receive a 405 Method Not Allowed response.

## Requests

The Mechanic webhook URL accepts these common request content types:

* text/plain
* application/json
* application/x-www-form-urlencoded

When called in-browser via JavaScript, one must be respectful of CORS origins. Mechanic webhooks support cross-domain requests from the Shopify store's myshopify.com subdomain, and from the store's primary domain \(if applicable\). Additional approved CORS origins may be configured in the store's Mechanic account settings.

### Examples

With cURL:

```text
curl -X POST -F foo=bar https://usemechanic.com/webhook/abcdef12-3456-abcd-ef12-3456abcdef12
```

```text
curl -X POST -H "content-type: application/json" -d @data.json https://usemechanic.com/webhook/abcdef12-3456-abcd-ef12-3456abcdef12
```

With jQuery:

```text
$.post('https://usemechanic.com/webhook/abcdef12-3456-abcd-ef12-3456abcdef12', { foo: 'bar' });
```

```text
$.ajax({
  url: 'https://usemechanic.com/webhook/abcdef12-3456-abcd-ef12-3456abcdef12',
  method: 'POST',
  data: { foo: 'bar' }
});
```

```text
let data = { foo: 'bar' };

$.ajax({
  url: 'https://usemechanic.com/webhook/abcdef12-3456-abcd-ef12-3456abcdef12',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  data: JSON.stringify(data)
});
```

## Responses

A successful webhook request will always receive a 204 No Content response, indicating that an event has been created with the submitted data. The event will be run asynchronously, along with any generated task and action runs that follow.

### Retrieving run results

The webhook ingress API is solely for ingesting data to form a new event. Because of this, the webhook response necessarily does not contain any data resulting from the runs that might follow. \(To further explain the point: there could be zero task runs that follow, or many, and there are no hard guarantees for when those runs would finish.\)

This means that the caller must use another avenue for retrieving the results of any generated runs. In general, there are three options for this:

* For a consistently re-occurring call with a single caller, the task responding to the webhook event may write its results to the Mechanic cache, allowing the caller to retrieve results using a [cache endpoint](cache/endpoints.md).
* For calls with variable or multiple callers, consider generating [a UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier), and submitting it to the webhook along with the other submitted data. Tasks responding to the webhook event should store their results keyed by that UUID, using an external storage mechanism \(possibly via the [HTTP](../core/actions/http.md) or [FTP](../core/actions/ftp.md) actions\).
* For calls triggered by customer activity on an online Shopify storefront, consider \(a\) requiring the customer to be logged in, \(b\) sending the customer ID in the webhook request data, \(c\) storing task results in a customer metafield \(using the [Shopify](../core/actions/shopify.md) action\), and \(d\) using storefront Liquid to render the content of that metafield, polling until a value is present.

