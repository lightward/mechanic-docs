# Can I query external APIs?

_For more general information on third-party APIs, including more options for reading and writing data, see_ [_Does Mechanic work with third-party APIs?_](https://docs.usemechanic.com/article/461-does-mechanic-work-with-third-party-apis)_._

Mechanic only has first-class API support for Shopify. However, you can use [the "http" action](https://docs.usemechanic.com/article/406-the-http-action) to fetch data from any source that's accessible with an HTTP URL. \(APIs that require authentication via query param or header - basic auth, for example - are all supported. APIs that require authentication via OAuth are generally not supported.\)

_Tip: use_ [_Couchdrop's Shared Links_](https://couchdrop.io/features/shared-links) _to make any file available via a secret URL, from an FTP server or cloud storage provider._

For this kind of work, we recommend an execution sequence that looks like this:

1. Use an "http" action to fire a GET request for your data's URL.
2. Set up your task to respond to mechanic/actions/perform, so you can respond to the GET request's downloaded results.
3. If you expect to use the retrieved data frequently, or across several tasks, consider using [the Mechanic cache](https://docs.usemechanic.com/article/457-using-the-cache) to store that data for easy re-use.

### Example

* Task: [Demonstration: Fetch an external configuration file](https://usemechanic.com/task/demonstration-fetch-an-external-configuration-file)

