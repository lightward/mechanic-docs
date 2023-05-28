# Report Toaster

Mechanic allows developers to integrate directly with [Report Toaster](https://apps.shopify.com/report-toaster) to retrieve and update data. Use Report Toaster's powerful reporting engine to retrieve and operate on your Shopify data.

{% hint style="info" %}
Report Toaster is offered by [Cloudlab](https://www.cloudlab.com/). Get assistance with this integration at [support.cloudlab.com](https://support.cloudlab.com/), or by emailing [support@cloudlab.com](mailto:support@cloudlab.com).
{% endhint %}

## What is Report Toaster?

Report Toaster is a Shopify app that allows merchants to access their data in ways not possible via the Shopify Admin.

From [Report Toaster's app store listing](https://apps.shopify.com/report-toaster):

> Introducing Report Toaster, created by Cloudlab. The most powerful reporting and analytics app available for Shopify merchants. Access any data in your store to create, schedule and export custom reports across all of your sales channels. You can also select from our list of over 80 pre-built reports.

### Terminology

Some terms to know when using Report Toaster.

#### **Pre-Built Reports**

Any report that comes included when the Report Toaster app is installed. Report Toaster includes over 80 pre-built reports.

#### **My Reports**

The set of reports customized and saved by a Report Toaster user. Each account will have its own unique set of "My Reports".

#### **Report ID**

Used to uniquely identify any Report Toaster report. Found in the URL when navigating to a specific report, as in the example below with the Report ID being `v_qygPGcofjQ`.

> https://reporttoaster.cloudlab.com/my-reports/v\_qygPGcofjQ

## Action

The Report Toaster integration includes a **report\_toaster** action which can be used to perform one or more operations on data within Report Toaster.

This action accepts two options, one specifying a Report Toaster operation, and the other varying based on the specified operation.

| Option                                                                                        | Description                                                                                                                 |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| operation                                                                                     | Required; The name of the operation to run as part of the Report Toaster action. Available types are "report" and "update". |
| \[varies, see [Report](report-toaster.md#report) or [Update](report-toaster.md#update) usage] | Required; see specific usage documentation                                                                                  |

### Report

The Report operation is used to run a report using the Report Toaster engine. The report can be from Pre-Built reports or My Reports.

This operation is most useful in concert with the `report_toaster/reports/create` webhook, by which a task may take the resulting report and pass to another service, or parse to complete additional tasks.

Report results can be returned inline, in which case there will be a 10MB limit. Alternatively, a file URL can be returned in a variety of formats.

<table><thead><tr><th width="150">Operation option</th><th>Description</th></tr></thead><tbody><tr><td>reports</td><td>Required; an array of objects defining the reports that will be executed. See below for the report options.</td></tr></tbody></table>

#### **Report object definition**

<table><thead><tr><th width="150">Report option</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>Required; a string specifying the unique identifier for a pre-defined report to be executed</td></tr><tr><td>file</td><td>Optional; an object describing a file to be returned when inline JSON results are not desired. Available file formats are "json", "csv", and "pdf"</td></tr><tr><td>meta</td><td>Optional; an object providing context for the report execution. This value will be returned with the  <code>report_toaster/reports/create</code> webhook</td></tr></tbody></table>

#### **Basic Example: Single operation returning JSON Data (limit 10 MB)**

```liquid
{% raw %}
{% action "report_toaster" %}
  { 
    {% comment %}
      Generate one or more reports. When each report is complete, a
      'report_toaster/reports/create' event will be created with the result.
    {% endcomment %}
    "operation": "report",
  
    {% comment %}
      This batch array identifies one or more reports to be run.
    {% endcomment %}
    "batch": [
      {
        {% comment %}
          The id of one of your 'My Reports'. This id is on the query string
          when you run the report in Report Toaster.
        {% endcomment %}
        "id": "vVYKue9k7P",
  
        {% comment %}
          Optional custom context that will be returned with the report
          data in the 'report_toaster/reports/create' event.
        {% endcomment %}
        "meta": {
          "internal_request_id": "something something",
          "task_id": {{ task.id | json }}
        }
      }
    ]
  }
{% endaction %}
{% endraw %}
```

#### **Basic Example: Batch operation - return one CSV file and two JSON**

```liquid
{% raw %}
{% action "report_toaster" %}
  {
    {% comment %}
      Generate one or more reports. When each report is complete, a
      'report_toaster/reports/create' event will be created with the result.
    {% endcomment %}
    "operation": "report",

    {% comment %}
      This batch array identifies one or more reports to be run.
    {% endcomment %}
    "batch": [
      {
        {% comment %}
          The id of one of your 'My Reports'. This id is on the query string
          when you run the report in Report Toaster.
        {% endcomment %}
        "id": "v_t3a23v4e23",

        {% comment %}
          Optionally set this to create a download link to the report data
          instead of returning the report data in the event. Options are
          'json', 'csv' and 'pdf'.
        {% endcomment %}
        "file": {
          "format": "csv"
        },

        {% comment %}
          Optional custom context that will be returned with the report
          data in the 'report_toaster/reports/create' event.
        {% endcomment %}
        "meta": {
          "internal_request_id": "something something",
          "task_id": {{ task.id | json }}
        }
      },
      { "id": "v_123" },
      { "id": "v_456" }
    ]
  }
{% endaction %}
{% endraw %}
```

### Update

The Update operation is used to update some data in the Report Toaster dataset. Results will be returned as part of the standard `mechanic/actions/perform` webhook.

The update operation will be limited to attributes that are intended for update via external sources.

| Operation option                                                               | Description                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[the name of the model to be updated; currently, only `"Order"` is supported] | <p>Required; an array of update objects which use the <a href="https://datatracker.ietf.org/doc/html/rfc7396">Merge/Patch</a> format. Current limitations are as follows:<br><br>1. The "id" attribute must be used to identify an Order.<br>2. Currently only "shipping_cost" and "transaction<em>_</em>fees" can be updated.</p> |

#### **Basic Example: Update shipping costs**

```liquid
{% raw %}
{% action "report_toaster" %}
  {
    {% comment %}
      Update operation to update values in Report Toaster
    {% endcomment %}
    "operation": "update",

    {% comment %}
      Update any number of orders with their corresponding shipping costs 
      and/or transaction fees
    {% endcomment %}
    "Order": [
      { "id": 4192644956357, "shipping_cost": 6.54 },
      { "id": 4192637419717, "transaction_fees": 5.32 },
      { "id": 4192651182277, "shipping_cost": 4.76, "transaction_fees": null }
    ]
  }
{% endaction %}
{% endraw %}
```

## Event topics

The Report Toaster action sends webhook events to Mechanic, reporting back on the results of an action.

### report\_toaster/reports/create

Occurs when a Report Toaster report has been created and is available for download.

#### **Basic Example: Return JSON data**

```json
{
  "id": "vVYKue9k7P",
  "name": "30-Day Summary", 
  "data": {....},
  "meta": {
    "internal_request_id": "something something",
    "task_id": "fc3872b8-66a0-4ffb-bcaf-6f1b4a966aba"
  },
  "definition": {....}
}
```

#### **Basic Example: Return file**

```json
{
  "id": "vVYKue9k7P",
  "name": "30-Day Summary", 
  "file": {
    "format": "pdf",
    "url": "https://example.com/report.pdf"
    "size": 1234567
  },
  "meta": {
    "internal_request_id": "something something",
    "task_id": "fc3872b8-66a0-4ffb-bcaf-6f1b4a966aba"
  },
  "definition": {....}
}
```

### report\_toaster/reports/fail

Occurs when a requested Report Toaster report has failed.

#### **Basic Example**

```json
{
  "id": "vVYKue9k7P",
  "name": "30-Day Summary",
  "error": {....},
  "meta": {
    "internal_request_id": "something something",
    "task_id": "fc3872b8-66a0-4ffb-bcaf-6f1b4a966aba"
  },
  "definition": {....}
}
```
