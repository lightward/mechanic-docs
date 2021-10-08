# Can I send data to Google Sheets?

Yes! There are two options: One uses the IMPORTDATA function within Google Sheets to pull data from your store into your spreadsheet. The other lets you dynamically push new data to Google Sheets, using their REST API.

## Using IMPORTDATA

Use Mechanic to publish a formatted CSV to your Shopify storefront. Use this guide to get started: [Creating scheduled CSV feeds](https://docs.usemechanic.com/article/378-creating-scheduled-csv-feeds).

Then, enter this formula into a cell in Google Sheets:

```text
=IMPORTDATA("https://yourshopifystore.com/pages/feed")
```

... adjusting the URL as appropriate, given your store's domain name and the page you've created in Shopify.

Learn more from Google: [IMPORTDATA](https://support.google.com/docs/answer/3093335?hl=en)

Please note: As of this writing, and [per documentation](https://support.google.com/docs/answer/58515?hl=en), the IMPORTDATA formula re-fetches data once every hour.

## Using a live push

Pushing data to Google Sheets requires an intermediary service, like IFTTT or Zapier. To get started with this route, configure IFTTT or Zapier to perform a Google Sheets operation, triggered by a webhook. Here's where to get started:

* [Google Sheets on IFTTT](https://ifttt.com/google_sheets)
* [Google Sheets + Webhooks on Zapier](https://zapier.com/apps/google-sheets/integrations/webhook)

Then, use [the "http" action](https://docs.usemechanic.com/article/406-the-http-action) to send data to the webhook URL that IFTTT or Zapier provides. Learn more about this general technique with our tutorial: [Triggering tasks with IFTTT](https://docs.usemechanic.com/article/389-triggering-tasks-with-ifttt).

