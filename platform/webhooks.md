# Webhooks

A Mechanic **webhook** allows data to be submitted directly to Mechanic, resulting in a new [**event**](../core/events/) having a particular topic, containing the submitted data. Webhooks are called with simple HTTP POST requests, which means they can be called from any programming language, and from many applications.

Mechanic's webhook API includes CORS support for all origins, making these requests available for use in online user experiences.

{% hint style="info" %}
This article is an introduction to webhooks. To get started quickly, see [Creating a Mechanic webhook](../resources/tutorials/creating-a-mechanic-webhook.md).
{% endhint %}

## Configuration

A Mechanic webhook is configured with a name and a specific event topic, and is assigned a unique, webhook-specific URL. Any valid POST request to the assigned webhook URL will result in a new event being created, having the configured topic, containing the parsed data from the POST request.

![](<../.gitbook/assets/image (11).png>)

## Requests

The Mechanic webhook API only accepts POST requests. (All other methods will receive a 405 Method Not Allowed response.)

The following content types are supported, and will be parsed appropriately as event data:

* text/plain
* application/json
* application/x-www-form-urlencoded

Webhook request headers are never included in event data.

Mechanic's webhook API includes CORS support for all origins, making these requests available for use in online user experiences.

### Client-specific endpoints

When configuring an integration with external apps and services, some require specific behavior from webhook hosts – usually for the purposes of validating a webhook before it's enabled. Mechanic includes specific support for some known webhook clients; request new ones at [mechanic.canny.io](https://mechanic.canny.io/).

A webhook invocation signals its client selection via a path suffix. Given a webhook URL of `webhooks.mechanic.dev/0000..0000`, a client-specific endpoint for Ship24 (for example) would be available at `webhooks.mechanic.dev/0000..0000/ship24`.

| Client | Webhook suffix | Behavior notes                                              |                                                                                                       |
| ------ | -------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Asana  | asana          | Supports the `X-Hook-Secret` header for the handshake phase | [Docs](https://developers.asana.com/docs/webhooks)                                                    |
| Ship24 | ship24         | Supports HEAD requests for webhook verification             | [Docs](https://documenter.getpostman.com/view/14877026/Tz5s4Gtu#3c5245aa-a14e-4995-ba3a-9045b92524e3) |

### Examples

With cURL:

```
curl -X POST -F foo=bar https://webhooks.mechanic.dev/abcdef12-3456-abcd-ef12-3456abcdef12
```

```
curl -X POST -H "content-type: application/json" -d @data.json https://webhooks.mechanic.dev/abcdef12-3456-abcd-ef12-3456abcdef12
```

With fetch:

```javascript
const data = {
  some_data: 'yep here it is',
};

fetch(
  'https://webhooks.mechanic.dev/0000-0000',
  {
    method: 'post',
    body: JSON.stringify(data),
    headers: {
      'content-type': 'application/json'
    },
  }
).then(console.log);
```

With jQuery:

```javascript
$.post('https://webhooks.mechanic.dev/0000-0000', { foo: 'bar' });
```

```javascript
$.ajax({
  url: 'https://webhooks.mechanic.dev/0000-0000',
  method: 'POST',
  data: { foo: 'bar' }
});
```

```javascript
let data = { foo: 'bar' };

$.ajax({
  url: 'https://webhooks.mechanic.dev/0000-0000',
  method: 'POST',
  headers: {
    'content-type': 'application/json'
  },
  data: JSON.stringify(data)
});
```

## Responses

A properly-formatted webhook request will always receive a 204 No Content response, even in the case of an incorrect webhook ID in the URL.

If an active webhook ID was used, then an event will be created with the submitted data. The event will be run asynchronously, along with any generated task and action runs that follow.

### Retrieving run results

The webhook ingress API is solely for ingesting data to form a new event. Because of this, the webhook response necessarily does not contain any data resulting from the runs that might follow. (To further explain the point: there could be zero task runs that follow, or many, and there are no hard guarantees for when those runs would finish.)

This means that the caller must use another avenue for retrieving the results of any generated runs. In general, there are three options for this:

* For a consistently re-occurring call with a single caller, the task responding to the webhook event may write its results to the Mechanic cache, allowing the caller to retrieve results using a [cache endpoint](cache/endpoints.md).
* For calls with variable or multiple callers, consider generating [a UUID](https://en.wikipedia.org/wiki/Universally\_unique\_identifier), and submitting it to the webhook along with the other submitted data. Tasks responding to the webhook event should store their results keyed by that UUID, using an external storage mechanism (possibly via the [HTTP](../core/actions/http.md) or [FTP](../core/actions/ftp.md) actions).
* For calls triggered by customer activity on an online Shopify storefront, consider (a) requiring the customer to be logged in, (b) sending the customer ID in the webhook request data, (c) storing task results in a customer metafield (using the [Shopify](../core/actions/integrations/shopify.md) action), and (d) using storefront Liquid to render the content of that metafield, polling until a value is present.
