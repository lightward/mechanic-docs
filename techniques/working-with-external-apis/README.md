# Working with external APIs

## Loading data into Mechanic

Mechanic is an event-driven platform. This means that all data _used_ by Mechanic needs to arrive in the form of an event. (The only exception here is Shopify itself: see [Interacting with Shopify](../../core/shopify/)).

To create events using third-party data, use one of these techniques:

* Use [the HTTP action](../../core/actions/http.md) to request the data you require, subscribing to mechanic/actions/perform to actually _use_ the downloaded data.
* Use [Couchdrop's Shared Links](https://couchdrop.io/features/shared-links) feature with an external FTP server or other cloud storage provider, to make any file available via a secret URL. Then, use [the HTTP action](../../core/actions/http.md) to request that data.
* Use Mechanic's [webhooks](../../platform/webhooks.md) to POST your data directly to Mechanic.
* Use inbound email to deliver your data to Mechanic, either in the message body or as an attachment. See [Receiving email](../../platform/email/receiving-email.md).
* If you only need to move files around, without actually using the file _contents_, use the [Files](../../core/actions/files.md) action with the [URL](../../core/actions/file-generators/url.md) file generator to download external files to a temporary Mechanic URL.

## Writing data to an external service

* Use the [HTTP](../../core/actions/http.md) action, using standard HTTP requests, with options for authenticating with custom headers.
* Use the [FTP](../../core/actions/ftp.md) action to upload data to third-party locations. Optionally, use a connecting service like [Couchdrop](https://couchdrop.io/) to connect to another cloud storage provider (e.g. Dropbox, Google Drive, S3, [etc](https://couchdrop.io/features/cloud-storage)).
* Use a [cache endpoint](../../platform/cache/endpoints.md) to save your data to an unguessable URL, where an external service may download it.

## Examples

* [Task: Add new customers to GetResponse](https://usemechanic.com/task/add-new-customers-to-getresponse)
* [Task: Send an event to Klaviyo when a fulfillment is delayed](https://tasks.mechanic.dev/send-an-event-to-klaviyo-when-a-fulfillment-is-delayed)
* [Demonstration: Query external paginated API](https://tasks.mechanic.dev/demonstration-query-external-paginated-api)
