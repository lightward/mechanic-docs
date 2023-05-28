# HTTP

The **HTTP** action performs HTTP requests. It is commonly used to invoke third-party APIs.

To use the response from an HTTP action, add a task subscription to [mechanic/actions/perform](../../techniques/responding-to-action-results.md).

{% hint style="info" %}
When developing task code, verify your HTTP action's behavior with [webhook.site](https://webhook.site/) (making sure not to share sensitive information with this service).
{% endhint %}

## Options

<table><thead><tr><th width="211.33333333333331">Option</th><th width="196">Type</th><th>Notes</th></tr></thead><tbody><tr><td><code>method</code></td><td>String, required</td><td>Must be one of <code>"options"</code>, <code>"head"</code>, <code>"get"</code>, <code>"post"</code>, <code>"put"</code>, <code>"patch"</code>, or <code>"delete"</code></td></tr><tr><td><code>url</code></td><td>String, required</td><td>Must start with <code>https://</code> or <code>http://</code></td></tr><tr><td><code>body</code></td><td>String, required for non-GET requests</td><td>Format varies, see below</td></tr><tr><td><code>files</code></td><td>Hash, optional</td><td>May be set to a JSON object, mapping filenames to <a href="file-generators/">file generators</a></td></tr><tr><td><code>headers</code></td><td>Hash, optional</td><td>May be set to a JSON object, mapping header names to header values</td></tr><tr><td><code>follow_redirects</code></td><td>Boolean, optional</td><td>Defaults to <code>true</code>, may be set to <code>false</code>; controls whether or not 3xx responses with <code>Location</code> headers are automatically followed to their destination</td></tr><tr><td><code>proxy</code></td><td>String, optional</td><td>May be a proxy URI string beginning with <code>https://</code>, <code>http://</code>, or <code>socks5://</code>; see "Using a proxy" below</td></tr><tr><td><code>verify</code></td><td>Boolean, optional</td><td>May be set to <code>false</code> to disable SSL certificate verification</td></tr><tr><td><code>error_on_5xx</code></td><td>Boolean, optional</td><td>May be set to <code>true</code> to have 5xx HTTP response codes be considered action errors</td></tr></tbody></table>

### Request format

The HTTP action has intelligently varying behavior, based on the presence and value of the Content-Type header, and the data type of the `body` option.

#### JSON

If the Content-Type header is unspecified or set to `application/json`, and if the `body` option is set to a JSON object or array, the request body will be automatically serialized to a JSON string, and the request will contain a Content-Type header set to `application/json`.

#### Form-encoded data

If the `files` option is given, its contents will be evaluated for [file generators](file-generators/), and the results will be used to construct a `multipart/form-data` upload request, combining generated files with any key-value pairs found in the `body` option.

```liquid
{% raw %}
{% action "http" %}
  {
    "method": "post",
    "url": "https://postman-echo.com/post",
    "body": {
      "hello": "world"
    },
    "files": {
      "robots.txt": {
        "url": "https://www.shopify.com/robots.txt"
      }
    }
  }
{% endaction %}
{% endraw %}
```

If the `files` option is not given, and if the Content-Type header is set to `application/x-www-form-urlencoded`, and if the `body` option is set to a JSON object or array, the request body will be serialized to a form-encoded string.

### Basic authentication

To authenticate a request using [the Authorization header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) and the "Basic" authentication type, use something like this:

```liquid
{% raw %}
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
{% endraw %}
```

### Using a proxy

The HTTP action supports HTTPS, HTTP, and SOCKS5 proxy connections via the `"proxy"` option, set to a URI string beginning with `https://`, `http://`, or `socks5://`. When configured, Mechanic will open a connection to your proxy server, and pass your request through that connection.

{% code title="Example HTTP action using a proxy" %}
```liquid
{% raw %}
{% action "http" %}
  {
    "method": "get",
    "url": "https://api.ipify.org?format=json",
    "proxy": "socks5://user:password@host.domain:port"
  }
{% endaction %}
{% endraw %}
```
{% endcode %}

{% hint style="warning" %}
We recommend using an HTTPS proxy server (rather than HTTP or SOCKS5) for a secure connection between Mechanic and your proxy. [QuotaGuard Shield](https://www.quotaguard.com/quotaguard-shield/) is a good option for this kind of service.
{% endhint %}

{% hint style="info" %}
Mechanic does not use static IP addresses for outbound requests. Using a connection proxy for your HTTP actions can allow you to control the client IP address of your API requests, for API vendors that require fixed IPs.
{% endhint %}

## Result

This action returns an object containing the following keys:

| File property | Description                                                                                                                  |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `status`      | An integer, specifying the response code                                                                                     |
| `headers`     | An object containing response headers, where each key is a string and each value is an array of values found for that header |
| `body`        | The interpreted value of the response body; see below                                                                        |
| `body_base64` | The original response body, encoded using base64                                                                             |

### Response headers

Because HTTP allows for the same header name to be present multiple times, this action's result specifies an array for each response header – even if the header was only present once.

To retrieve a specific header in a task responding to [mechanic/actions/perform](../../techniques/responding-to-action-results.md), use something like this:

```javascript
{% raw %}
{% log response_type_header: action.run.result.headers['content-type'][0] %}
{% endraw %}
```

### Response body

If the response contained a Content-Type header set to `application/json`, the `body` result value will be the result of parsing the response body for JSON.

For all other cases, the `body` result value will be an UTF8 string, regardless of the response body's original encoding. To access the response body in its original encoding, use the `body_base64` result value, passing it through the [decode\_base64](http.md) Liquid filter if necessary.

### Handling errors

By default, this action will consider any valid HTTP response to be a success, regardless of its response code.

However, because 5xx responses should often be considered a retryable error, this action supports the `error_on_5xx` option. When set to `true`, this action will interpret any 5xx responses as an action error.

As with all runs, HTTP action errors are subject to [Mechanic's retry policy](../runs/retries.md).

## Example

This task prompts the user for text input, and submits it to a public API that returns everything submitted to it. The task then re-invokes itself, using the [Echo](echo.md) action to display the response status, content type, and body.

{% tabs %}
{% tab title="Subscriptions" %}
```
mechanic/user/text
mechanic/actions/perform
```
{% endtab %}

{% tab title="Code" %}
```liquid
{% raw %}
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
{% endraw %}
```
{% endtab %}
{% endtabs %}
