# Webhooks

A Mechanic **webhook** allows data to be submitted directly to Mechanic, resulting in a new [**event**](../core/events/) having a particular topic, containing the submitted data. Webhooks are called with simple HTTP POST requests, which means they can be called from any programming language, and from many applications.

Mechanic's webhook API includes CORS support for all origins, making these requests available for use in online user experiences.

{% hint style="info" %}
This article is an introduction to webhooks. To get started quickly, see [Creating a Mechanic webhook](../resources/tutorials/creating-a-mechanic-webhook.md).
{% endhint %}

{% hint style="info" %}
Tools like [Postman](https://www.postman.com/) ([docs](https://learning.postman.com/docs/sending-requests/requests/)) can be useful for exercising Mechanic webhooks without code, for verifying their behavior.
{% endhint %}

## Configuration

![](<../.gitbook/assets/Screenshot 2023-03-07 at 11.04.17 AM.png>)

* Name — Something to remember this webhook by ✨
* Event topic — The [topic](../core/events/topics.md) used for events created via this webhook
* Event data mode — Either "Auto" or "Full request"; see below
* Webhook URL — A unique, permanent, webhook-specific URL, supplied by Mechanic after creating the webhook

### Event data mode

Each webhook has a choice of modes used to translate request data into event data.

<figure><img src="../.gitbook/assets/Screenshot 2023-03-08 at 1.23.17 PM.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Changing a webhook's event data mode takes effect immediately for all incoming requests. It does not apply retroactively, i.e. it has no impact on webhook events that were received prior to the change.
{% endhint %}

#### Auto (default)

In "Auto" mode, event data is the result of merging an interpreted hash of parameters from the request body with an interpreted hash of parameters from the request query string.

If no parameters from the request body are found, or if parsing the request body fails for any reason, event data is set to parameters from the request query string.

If no structured parameters are found from either the request body or the request query string, event data is set to the request body in string form. If the string is blank (i.e. zero-length or consisting only of whitespace), event data is set to `null`.

#### Full request

In "Full request" mode, event data is set to a hash with the following keys:

<table><thead><tr><th width="171.33333333333331">Key</th><th width="213">Type</th><th>Notes</th></tr></thead><tbody><tr><td><code>body_base64</code></td><td>Base64-encoded string</td><td>The raw request body, encoded in base64</td></tr><tr><td><code>body</code></td><td>Hash or null</td><td>The interpreted set of structured parameters from the request body, or null if parsing fails for any reason</td></tr><tr><td><code>client_ip</code></td><td>String</td><td>The IP address of the requesting client</td></tr><tr><td><code>headers</code></td><td>Hash</td><td>Represents the request headers, keyed by lowercased header names, containing arrays of string values (⚠️ see warning below)</td></tr><tr><td><code>mime_type</code></td><td>String or null</td><td>The MIME type of the request as interpreted from the <code>content-type</code> request header, or null if that header was not found</td></tr><tr><td><code>query_string</code></td><td>String or null</td><td>Contains everything after the "?" character in the request URL, or null if no such value was found</td></tr><tr><td><code>query</code></td><td>Hash or null</td><td>Contains the interpreted set of structured parameters from the query string, or null if none were found</td></tr><tr><td><code>request_id</code></td><td>UUID string</td><td>Uniquely identifies the request, having the same value found in the original webhook request response in the <code>x-request-id</code> header</td></tr><tr><td><code>webhook_id</code></td><td>UUID string</td><td>Identifies the Mechanic webhook which handled the request</td></tr></tbody></table>

{% hint style="info" %}
Decode the base64-encoded request body with Liquid, using Mechanic's \[decode\\\_base64]\(liquid/filters/#base64-decode\_base64) filter.

```liquid
{% assign raw_request_body = event.data.body_base64 | decode_base64 %}
```
{% endhint %}

{% hint style="danger" %}
Mechanic's webhook request header representation has an important limitation: the array of values for each header is currently limited to a size of one (1). If multiple request headers are given using the same header name, their values will be concatenated with commas and represented in a single string, as the sole array element.

This means that a request which defines both `X-Foo: Bar` and `X-Foo: Baz` headers will be represented thusly:

```json
"headers": {
  "x-foo": [
    "Bar,Baz"
  ]
}
```

Let us first acknowledge that this is ridiculous. 💯 ✅

As of this writing (2023-03-08), an upstream provider of Mechanic's does not have proper support for requests containing repeated header names. Mechanic's behavior here is a compromise chosen (1) to allow us to resolve this in the future without requiring a change to the format of this representation, and (2) for consistency with [HTTP action's response format](../core/actions/http.md#response-headers).

When this issue is resolved, the header example above will be represented as follows:

```json
"headers": {
  "x-foo": [
    "Bar",
    "Baz"
  ]
}
```

Task code that reads webhook request headers should take these two possible representations into account.
{% endhint %}

{% code title="Sample " %}

```json
{
  "body_base64": "LS0tLS0[...]S0NCg==",
  "body": {
    "file_upload": {
      "content_base64": "iVBORw0[...]K5CYII=",
      "mime_type": "image/png",
      "name": "tiny.png",
      "size": 1265
    },
    "key": "value",
    "hash": {
      "key": "hash_value"
    },
    "array": [
      "array_value"
    ]
  },
  "client_ip": "8.8.8.8",
  "headers": {
    "content-length": [
      "1759"
    ],
    "content-type": [
      "multipart/form-data; boundary=----WebKitFormBoundaryqcjvhXaEdqng2TkC"
    ],
    "x-custom": [
      "Value"
    ],
    "x-foo": [
      "Bar,Baz"
    ]
  },
  "mime_type": "multipart/form-data",
  "query_string": "key=value&hash[key]=hash_value&array[]=array_value",
  "query": {
    "key": "value",
    "hash": {
      "key": "hash_value"
    },
    "array": [
      "array_value"
    ]
  },
  "request_id": "a8723f25-20ff-4b89-a46c-73fc387a16d3",
  "webhook_id": "d166806f-1f79-4283-b4b4-80a38ee04431"
}
```

{% endcode %}

## Requests

The Mechanic webhook API only accepts POST requests. (All other methods will receive a 405 Method Not Allowed response.)

Mechanic's webhook API includes CORS support for all origins, making these requests available for use in online user experiences.

### File uploads

File uploads are supported via multipart/form-data webhook requests. Each uploaded file will be represented in event data (regardless of event data mode) as a structured hash having the following keys:

* `content_base64` — A base64 string representation of the file content
* `mime_type` — The MIME type of the file, as declared in the headers for that file
* `name` — The original name of the file, as declared in the headers for that file
* `size` — An integer reflecting the size of the file in bytes

{% hint style="info" %}
Decode base64-encoded file content with Liquid, using Mechanic's \[decode\\\_base64]\(liquid/filters/#base64-decode\_base64) filter.

```liquid
{% assign file = event.data.body.my_upload %}
{% assign raw_file_content = file.content_base64 | decode_base64 %}
```
{% endhint %}

{% code title="File upload sample representation" %}

```json
{
  "my_upload": {
    "content_base64": "PD94bWw[...]mc+Cg==",
    "mime_type": "image/svg+xml",
    "name": "logo.svg",
    "size": 12345
  }
}
```

{% endcode %}

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

### Webhook request ID

All POST requests to webhooks.mechanic.dev will receive an `x-request-id` response header, containing a UUID. This ID can be used as a search term when looking up a Mechanic event, and can be referenced in Liquid as a part of the [Event object](liquid/objects/event.md) via `event.webhook_request_id`.

### Retrieving run results

The webhook ingress API is solely for ingesting data to form a new event. Because of this, the webhook response necessarily does not contain any data resulting from the runs that might follow. (To further explain the point: there could be zero task runs that follow, or many, and there are no hard guarantees for when those runs would finish.)

This means that the caller must use another avenue for retrieving the results of any generated runs. In general, there are three options for this:

* For a consistently re-occurring call with a single caller, the task responding to the webhook event may write its results to the [Mechanic cache](cache/), allowing the caller to retrieve results using a [cache endpoint](cache/endpoints.md).
* For calls with variable or multiple callers, use the webhook request ID by reading the `x-request-id` response header in your client code, and by referencing `event.webhook_request_id` in your task's Liquid code in. Tasks responding to the webhook event should store their results keyed by that UUID, using an external storage mechanism (possibly via the [HTTP](../core/actions/http.md) or [FTP](../core/actions/ftp.md) actions).
* For calls triggered by customer activity on an online Shopify storefront, consider (a) requiring the customer to be logged in, (b) sending the customer ID in the webhook request data, (c) storing task results in a customer metafield (using the [Shopify](../core/actions/integrations/shopify.md) action), and (d) using storefront Liquid to render the content of that metafield, polling until a value is present.
