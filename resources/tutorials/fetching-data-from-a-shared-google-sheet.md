# Fetching data from a shared Google sheet

In this tutorial, you'll learn how to publish a Google sheet to the web as a comma-separated values (CSV) file and then fetch that data from Mechanic.

## Instructions

### 1. Create a Google sheet with data.

{% hint style="info" %}
The data in the sheet should be in a format that makes sense as a CSV file. The first row should contain the column headers and there shouldn't be any data on the sheet outside of those columns.
{% endhint %}

You can either create a sheet with the sample data shown below or you can use your own data for this tutorial. Keep in mind that the column headers in the first row will be the exact keys that you need to reference in the task when iterating over the data rows.

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

### 2. Publish the sheet to the web as a CSV.

{% hint style="warning" %}
Sharing sheets openly this way on the web so that is accessible by Mechanic works best for non-identifying data. Be sure to clean all customer-specific data and branding from your sheet data before publishing.&#x20;
{% endhint %}

From the **File / Share** menu, choose the **Publish to web** option.

<div data-full-width="false">

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

</div>

From the **Link** tab of the modal dialog that opens, select the sheet you wish to share and the _Comma-separated values (.csv)_ option, and then click the **Publish** button.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

After clicking OK on the confirmation dialog, the modal will update to show you the URL link that you will need to copy into the demonstration task configuration settings. You can safely close this dialog window now.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### 3. Add the demonstration task to your Mechanic instance.

Navigate to the "Pages" section of the Shopify admin (under "Online Store"), and click the "Add page" button (or search the admin for "add page"). Name the page "Feed" (or another name of your liking), and change the page template to "page.feed.liquid".

Save the page.

### You're done!

Open up the page you just created, and you should see the contents of your feed. :) If you have any questions, head to [our community Slack](../slack.md).
