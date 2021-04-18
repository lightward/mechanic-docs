# Working with external APIs

## Loading data into Mechanic

Mechanic is an event-driven platform. This means that all data _used_ by Mechanic needs to arrive in the form of an event. \(The only exception here is Shopify itself: see [Interacting with Shopify](../core/interacting-with-shopify/)\).

To create events using third-party data, use one of these techniques:

* Use [the HTTP action](../core/actions/types/http.md) to request the data you require, subscribing to mechanic/actions/perform to actually _use_ the downloaded data.
* Use [Couchdrop's Shared Links](https://couchdrop.io/features/shared-links) feature with an external FTP server or other cloud storage provider, to make any file available via a secret URL. Then, use [the HTTP action](../core/actions/types/http.md) to request that data.
* Use Mechanic's [webhooks](../platform/webhooks.md) to POST your data directly to Mechanic.
* Use inbound email to deliver your data to Mechanic, either in the message body or as an attachment. See [Receiving email](https://docs.usemechanic.com/article/445-receiving-email).
* If you only need to move files around, without actually using the file _contents_, use [the "files" action](https://docs.usemechanic.com/article/449-the-files-action) with [the "url" generator](https://docs.usemechanic.com/article/338-generating-files#url) to download external files to a temporary Mechanic URL.

## Writing data to an external service

* Use [the "http" action](https://docs.usemechanic.com/article/406-the-http-action), using standard HTTP requests, with options for authenticating with custom headers.
* Use [the "ftp" action](https://docs.usemechanic.com/article/379-the-ftp-action) to upload data to third-party locations. Optionally, use a connecting service like [Couchdrop](https://couchdrop.io/) to connect to another cloud storage provider \(e.g. Dropbox, Google Drive, S3, [etc](https://couchdrop.io/features/cloud-storage)\).
* Use a cache endpoint to save your data to an unguessable URL, where an external service may download it. See [Using cache endpoints to share data](https://docs.usemechanic.com/article/446-using-cache-endpoints-to-share-data).

## Examples

* [Task: Send an SMS via Nexmo when a product is created](https://usemechanic.com/task/send-an-sms-via-nexmo-when-a-product-is-created)
* [Task: Send new customer signups to IFTTT](https://usemechanic.com/task/send-new-customer-signups-to-ifttt)
* [Task: Add new customers to GetResponse](https://usemechanic.com/task/add-new-customers-to-getresponse)

