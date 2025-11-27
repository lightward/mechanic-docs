# Google Sheets

The Google Sheets action allows you to interact with Google Sheets. It supports creating new spreadsheets, appending data to existing sheets, and exporting spreadsheets in various formats. Mechanic interacts with Google Sheets via the Google Sheets API, using OAuth2 for authentication.

{% hint style="info" %}
Due to Google security restrictions, Mechanic can only access spreadsheets that were created through Mechanic itself. To work with Google Sheets:

* First create a spreadsheet using the `"create_spreadsheet"` operation
* Store the returned spreadsheet ID for later use
* Then use operations like `"append_rows"` on this spreadsheet

See this great [example](https://tasks.mechanic.dev/demonstration-add-new-orders-to-google-sheet) in the task library.
{% endhint %}

## Options

<table><thead><tr><th width="165">Option</th><th width="90">Type</th><th>Description</th></tr></thead><tbody><tr><td>account</td><td>string</td><td>Required: the Google account email address to authenticate with</td></tr><tr><td>operation</td><td>string</td><td>Required: the operation to perform. One of: "append_rows", "create_spreadsheet", "export_spreadsheet"</td></tr><tr><td>spreadsheet_id</td><td>string</td><td>Required: for append_rows and export_spreadsheet; the ID of the target spreadsheet</td></tr><tr><td>title</td><td>string</td><td>Required: for create_spreadsheet; the title for the new spreadsheet</td></tr><tr><td>rows</td><td>array</td><td>Required: for append_rows and optional for create_spreadsheet; array of arrays containing the data to write</td></tr><tr><td>sheet_name</td><td>string</td><td>Optional: for append_rows; defaults to "Sheet1"</td></tr><tr><td>file_type</td><td>string</td><td>Optional: for export_spreadsheet; the format to export. One of: "xlsx" (default), "csv", "pdf", "html", "ods", "tsv"</td></tr><tr><td>folder_path</td><td>string</td><td>Optional: for create_spreadsheet; the folder path where the spreadsheet should be created (e.g., "reports/2024/monthly")</td></tr></tbody></table>

## Operations

### append\_rows

Adds new rows to an existing spreadsheet.

#### Required Options

* account
* spreadsheet\_id
* rows

#### Optional Options

* sheet\_name (defaults to "Sheet1")

### create\_spreadsheet

Creates a new spreadsheet, optionally with initial data.

#### Required Options

* account
* title

#### Optional Options

* folder\_path (path where spreadsheet should be created)
* rows (initial data to populate the spreadsheet)

### export\_spreadsheet

Exports a spreadsheet in various formats.

#### Required Options

* account
* spreadsheet\_id

#### Optional Options

* file\_type&#x20;
  * xlsx (default)
  * csv
  * pdf
  * html
  * ods
  * tsv



## Authentication

This action requires connecting a Google account with the appropriate  permissions. To connect an account:

1. Go to the Settings screen
2. Click Authentication
3. Follow the Google account connection flow from the Google tab

## File Access

The action can only access spreadsheets it creates, no other spreadsheets in your drive.

## Folder Support

When creating spreadsheets, you can specify a folder path to organize your files:

* Use forward slashes to separate folder names (e.g., "reports/2024/monthly")
* Folders will be created if they don't exist
* Can only access folders created by this integration
* Invalid characters not allowed: `< > : " / \ | ? *`

### Folder Path Examples

```
reports/monthly           # Two levels deep
data/2024/q1/sales       # Four levels deep
archives/exports/sheets   # Three levels deep
```

## Examples

### Append Rows to Existing Google Sheet

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "append_rows",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Orders",
    "rows": [
      ["Order ID", "Customer", "Total"],
      ["1001", "John Doe", "99.99"],
      ["1002", "Jane Smith", "149.99"]
    ]
  }
{% endaction %}
```

### Create New Google Sheet

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "create_spreadsheet",
    "title": "Monthly Sales Report",
    "rows": [
      ["Month", "Revenue", "Expenses", "Profit"],
      ["January", "5000", "3000", "2000"],
      ["February", "5500", "3200", "2300"]
    ]
  }
{% endaction %}
```

### Export Google Sheet

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "export_spreadsheet",
    "spreadsheet_id": "1234567890abcdef",
    "file_type": "pdf"
  }
{% endaction %}
```

### Dynamic Data Example

```liquid
{% assign order_rows = array %}
{% assign header_row = array %}
{% assign header_row["Order", "Customer", "Total"] %}
{% assign order_rows[header_row] %}

{% for order in shop.orders %}
  {% assign order_row = array %}
  {% assign order_row[order.name, order.customer.name, order.total_price] %}
  {% assign order_rows[order_row] %}
{% endfor %}

{% action "google_sheets" %}
  {
    "account": {{ options.google_account | json }},
    "operation": "append_rows",
    "spreadsheet_id": {{ options.spreadsheet_id | json }},
    "rows": {{ order_rows | json }}
  }
{% endaction %}
```

### Create Google Sheet in a Folder

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "create_spreadsheet",
    "folder_path": "reports/2024/monthly",
    "title": "March Sales",
    "rows": [
      ["Date", "Revenue", "Units"],
      ["2024-03-01", "5000", "50"],
      ["2024-03-02", "6000", "60"]
    ]
  }
{% endaction %}
```

### Read Data From Google Sheet

{% code title="Task subscriptions" %}

```liquid
mechanic/user/trigger
mechanic/actions/perform
```

{% endcode %}

<pre class="language-liquid" data-title="Task code"><code class="lang-liquid"><strong>{% if event.topic == "mechanic/user/trigger" %}
</strong>  {% action "google_sheets" %}
    {
      "account": {{ options.google_account__required | json }},
      "operation": "export_spreadsheet",
      "spreadsheet_id":  {{ options.spreadsheet_id__required | json }},
      "file_type": "csv"    
    }
  {% endaction %}
{% endif %}

{% if event.topic == "mechanic/actions/perform" %}
  {% if action.type == "google_sheets" and action.run.ok %}
    {% assign sheet_data = action.run.result.data_base64 | 
    base64_decode | parse_csv: headers: true %}
      {% action "echo" sheet_data %}
  {% endif %}
{% endif %}
</code></pre>

## Action Responses

The action returns different responses based on the operation performed:

### append\_rows Response

```typescript
{
  "spreadsheet_id": string,
  "updated_range": string,
  "updated_rows": number,
  "updated_columns": number,
  "spreadsheet_url": string
}
```

### Example:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "updated_range": "Sheet1!A1:C3",
  "updated_rows": 3,
  "updated_columns": 3,
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

#### create\_spreadsheet Response

```typescript
{
  "spreadsheet_id": string,
  "spreadsheet_url": string,
  "title": string
}
```

Example:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef",
  "title": "Monthly Sales Report"
}
```

#### create\_spreadsheet Response with Folder

```typescript
{
  "spreadsheet_id": string,
  "spreadsheet_url": string,
  "title": string,
  "folder_path": string
}
```

#### Example:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef",
  "title": "March Sales",
  "folder_path": "reports/2024/monthly"
}
```

#### export\_spreadsheet Response

```typescript
{
  "spreadsheet_id": string,
  "name": string,
  "size": number,
  "file_type": string,
  "data_base64": string
}
```

Example:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "name": "Monthly Sales Report",
  "size": 12345,
  "file_type": "pdf",
  "data_base64": "base64encodeddata..."
}
```

