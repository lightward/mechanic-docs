# HTTP

The **HTTP** action performs HTTP requests. It is commonly used to invoke third-party APIs.

All HTTP methods are supported; the results of HTTP requests may be reused by a task via [mechanic/actions/perform](../../events/event-topic-reference/mechanic.md#actions).

## Options

| Option | Description |
| :--- | :--- |
| `method` | Required; must be one of `"get"`, `"post"`, `"put"`, `"patch"`, or `"delete"` |
| `url` | Required; must be a string beginning with `https://` or `http://` |
| `body` | Required unless `method` is `"get"`; format varies, see below |
| `headers` | Optional; may be set to a JSON object, mapping header names to header values |
| `verify` | Optional; may be set to `false` to disable SSL certificate verification |
| `error_on_5xx` | Optional; may be set to `true` to have 5xx HTTP response codes be considered action errors |

### Request format

The HTTP action has intelligently varying behavior, based on the presence and value of the Content-Type header, and the data type of the `body` option.

#### JSON

If the Content-Type header is unspecified or set to `application/json`, and if the `body` option is set to a JSON object or array, the request body will be automatically serialized to a JSON string, and the request will contain a Content-Type header set to `application/json`.

#### Form-encoded data

If the Content-Type header is set to `application/x-www-form-urlencoded`, and if the `body` option is set to a JSON object or array, the request body will be serialized to a form-encoded string.

### Basic authentication

To authenticate a request using [the Authorization header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) and the "Basic" authentication type, use something like this:

```javascript
{% assign username = "guest" %}
{% assign password = "guest" %}
{% assign authorization_header = username | append: ":" | append: password | base64 | prepend: "Basic " %}

{% action "http" %}
  {
    "method": "get",
    "url": "https://jigsaw.w3.org/HTTP/Basic/",
    "headers": {
      "Authorization": {{ authorization_header | json }}
    }
  }
{% endaction %}
```

## Result

This action returns an object containing the following keys:

| File property | Description |
| :--- | :--- |
| `status` | An integer, specifying the response code |
| `headers` | An object containing response headers, where each key is a string and each value is an array of values found for that header |
| `body` | The interpreted value of the response body; see below |
| `body_base64` | The original response body, encoded using base64 |

### Response headers

Because HTTP allows for the same header name to be present multiple times, this action's result specifies an array for each response header – even if the header was only present once.

To retrieve a specific header in a task responding to [mechanic/actions/perform](../), use something like this:

```javascript
{% log response_type_header: action.run.result.headers['content-type'][0] %}
```

### Response body

If the response contained a Content-Type header set to `application/json`, the `body` result value will be the result of parsing the response body for JSON.

For all other cases, the `body` result value will be an UTF8 string, regardless of the response body's original encoding. To access the response body in its original encoding, use the `body_base64` result value, passing it through the [decode\_base64](http.md) Liquid filter if necessary.

### Handling errors

By default, this action will consider any valid HTTP response to be a success, regardless of its response code.

However, because 5xx responses should often be considered a retryable error, this action supports the `error_on_5xx` option. When set to `true`, this action will interpret any 5xx responses as an action error.

As with all runs, HTTP action errors are subject to [Mechanic's retry policy](../../runs/retries.md).

## Example

This task prompts the user for text input, and submits it to a public API that returns everything submitted to it. The task then re-invokes itself, using the [Echo](echo.md) action to display the response status, content type, and body.

{% tabs %}
{% tab title="Subscriptions" %}
```text
mechanic/user/text
```
{% endtab %}

{% tab title="Code" %}
```javascript
{% if event.topic == "mechanic/user/text" %}
  {% action "http" %}
    {
      "method": "post",
      "url": "https://postman-echo.com/post",
      "body": {{ event.data | json }}
    }
  {% endaction %}
{% else %}
  {% action "echo",
    response_status: action.run.result.status,
    response_content_type: action.run.result.headers['content-type'][0],
    response_body: action.run.result.body %}
{% endif %}
```
{% endtab %}
{% endtabs %}

