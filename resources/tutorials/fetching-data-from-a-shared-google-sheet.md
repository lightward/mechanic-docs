# Fetching data from a shared Google sheet

In this tutorial, you'll learn how to publish a Google sheet to the web as a comma-separated values (CSV) file and then fetch that data from Mechanic.

## Instructions

### 1. Create a Google sheet with data.

{% hint style="info" %}
The data in the sheet should be in a format that makes sense as a CSV file. The first row should contain the column headers and there shouldn't be any data on the sheet outside of those columns.
{% endhint %}

You can either create a sheet with the sample data shown below or you can use your own data for this tutorial. Keep in mind that the column headers in the first row will be the exact keys that you need to reference in the task when iterating over the data rows for your own usage.

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

### 3. Add and configure the demonstration task.

You can either add the demonstration task using the **Try this task** button from this task library link - [Demonstration task: Fetch data from a shared Google sheet](https://tasks.mechanic.dev/demonstration-fetch-data-from-a-shared-google-sheet) - or you can add it from within the **Add task** screen inside of Mechanic.

After adding the task you should update the **Gsheet URL** option field with the link to your sheet that was generated in the prior step. Update the **Alert email recipients** with one or more email addresses where you want to be notified in case Mechanic is not able to access the shared sheet (e.g. the share is disabled).

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

### 4. Run the task and review the output.

Run the task manually using the **Run task** button and it will run the first sequence of the task, which will make an **HTTP** request to **GET** the sheet data.&#x20;

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

To see the results of the data retrieval you need to click on the **mechanic/actions/perform** child event after it appears.

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

### Next steps

Using the reference information available in these docs, write your own Mechanic script to iterate over the rows of data (array of hashes) that is parsed from the CSV, and make useful [updates to your Shopify data](../../core/actions/shopify.md) using the GraphQL or REST APIs.

If you have any questions, head to [our community Slack](../slack.md).
