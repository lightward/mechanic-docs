# Can I query external APIs?

{% hint style="info" %}
For more general information on third-party APIs, including more options for reading and writing data, see [Working with external APIs](../techniques/working-with-external-apis.md).
{% endhint %}

Mechanic only has first-class API support for Shopify. However, you can use the [HTTP](../core/actions/http.md) action to fetch data from any source that's accessible with an HTTP URL. \(APIs that require authentication via query param or header - basic auth, for example - are all supported. APIs that require authentication via OAuth are generally not supported.\)

{% hint style="info" %}
Tip: use [Couchdrop's Shared Links](https://couchdrop.io/features/shared-links) to make any file available via a secret URL, from an FTP server or cloud storage provider.
{% endhint %}

For this kind of work, we recommend an execution sequence that looks like this:

1. Use an HTTP action to fire a GET request for your data's URL.
2. Set up your task to respond to mechanic/actions/perform, so you can respond to the GET request's downloaded results.
3. If you expect to use the retrieved data frequently, or across several tasks, consider using the [cache](../platform/cache/) to store that data for easy re-use.

## Example

* [Demonstration: Fetch an external configuration file](https://usemechanic.com/task/demonstration-fetch-an-external-configuration-file)

