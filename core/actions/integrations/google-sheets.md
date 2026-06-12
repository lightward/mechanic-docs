# Google Sheets

The Google Sheets action allows you to interact with Google Sheets. It supports creating spreadsheets (with one or many sheets), reading, appending, updating, and clearing data, deleting rows, managing sheets (tabs), and exporting spreadsheets in various formats. Mechanic interacts with Google Sheets via the Google Sheets API, using OAuth2 for authentication.

{% hint style="info" %}
Due to Google security restrictions, Mechanic can only access spreadsheets that were created through Mechanic itself. To work with Google Sheets:

* First create a spreadsheet using the `"create_spreadsheet"` operation
* Store the returned spreadsheet ID for later use
* Then use operations like `"append_rows"` on this spreadsheet

See this great [example](https://tasks.mechanic.dev/demonstration-add-new-orders-to-google-sheet) in the task library.
{% endhint %}

## Options

<table><thead><tr><th width="200">Option</th><th width="90">Type</th><th>Description</th></tr></thead><tbody><tr><td>account</td><td>string</td><td>Required: the Google account email address to authenticate with</td></tr><tr><td>operation</td><td>string</td><td>Required: the operation to perform. One of: "create_spreadsheet", "append_rows", "update_rows", "clear_values", "get_values", "delete_rows", "add_sheet", "rename_sheet", "delete_sheet", "export_spreadsheet"</td></tr><tr><td>spreadsheet_id</td><td>string</td><td>Required for every operation except create_spreadsheet; the ID of the target spreadsheet</td></tr><tr><td>title</td><td>string</td><td>For create_spreadsheet; the title for the new spreadsheet (defaults to "New Spreadsheet")</td></tr><tr><td>rows</td><td>array</td><td>Array of arrays containing the data to write. Required for append_rows and update_rows; optional for create_spreadsheet and add_sheet</td></tr><tr><td>sheets</td><td>array</td><td>For create_spreadsheet; an array of <code>{"name": ..., "rows": [...]}</code> objects to create a spreadsheet with multiple named sheets. Mutually exclusive with "rows"</td></tr><tr><td>sheet_name</td><td>string</td><td>The name of the sheet (tab) to target. Required for add_sheet, delete_rows, rename_sheet, and delete_sheet. Optional for append_rows, update_rows, clear_values, and get_values — when omitted where allowed, the first sheet is used (whatever its name; Mechanic never assumes "Sheet1")</td></tr><tr><td>new_sheet_name</td><td>string</td><td>Required for rename_sheet; the new name for the sheet</td></tr><tr><td>sheet_range</td><td>string</td><td>An A1-notation range (e.g. "Orders!A2:C10"). Used by append_rows, update_rows, clear_values, and get_values. A range without a sheet qualifier combines with sheet_name when one is given</td></tr><tr><td>start_row / end_row</td><td>number</td><td>For delete_rows; 1-based, inclusive row numbers. end_row defaults to start_row (a single row)</td></tr><tr><td>value_input_option</td><td>string</td><td>For operations that write values: "raw" (default — values are stored exactly as given) or "user_entered" (values are parsed as if typed into the Sheets UI: dates become dates, numbers become numbers, and strings starting with "=" become formulas)</td></tr><tr><td>value_render_option</td><td>string</td><td>For get_values: "formatted_value" (default), "unformatted_value", or "formula"</td></tr><tr><td>file_type</td><td>string</td><td>For export_spreadsheet; one of: "xlsx" (default), "csv", "pdf", "html", "ods", "tsv"</td></tr><tr><td>folder_path</td><td>string</td><td>For create_spreadsheet; the folder path where the spreadsheet should be created (e.g., "reports/2024/monthly")</td></tr><tr><td>retry_on_transient_errors</td><td>boolean</td><td>For append_rows; when true, allows Mechanic to retry the append on ambiguous transient errors (e.g. timeouts). Defaults to false</td></tr></tbody></table>

{% hint style="warning" %}
**Only use `"value_input_option": "user_entered"` with data you trust.** Under `user_entered`, any cell value beginning with `=` is executed as a live formula in the spreadsheet. Customer-supplied values (order notes, names, line item properties) should be written with the default `"raw"` mode.
{% endhint %}

## Operations

### create\_spreadsheet

Creates a new spreadsheet — with a single sheet (optionally populated via `rows`), or with multiple named sheets via `sheets`.

#### Required Options

* account

#### Optional Options

* title (defaults to "New Spreadsheet")
* folder\_path (path where spreadsheet should be created)
* rows (initial data for the default sheet)
* sheets (an array of `{"name": ..., "rows": [...]}` objects; mutually exclusive with rows)
* value\_input\_option

### append\_rows

Adds new rows to the end of a sheet's existing data.

#### Required Options

* account
* spreadsheet\_id
* rows

#### Optional Options

* sheet\_name (defaults to the spreadsheet's first sheet)
* sheet\_range (an explicit A1 range to append within)
* value\_input\_option
* retry\_on\_transient\_errors (defaults to false)

{% hint style="warning" %}
The `retry_on_transient_errors` option is opt-in because some transient failures are ambiguous — for example, a timeout or connection reset may occur after Google has already accepted the write but before Mechanic receives confirmation. Retrying in that case can result in duplicate rows. Rate-limited requests (HTTP 429) are rejected before any data is written, so Mechanic always retries those automatically; this option only governs the ambiguous cases.
{% endhint %}

### update\_rows

Writes rows at a specific range, overwriting whatever is there. Provide `sheet_range` (e.g. `"Orders!A5:C5"`), or `sheet_name` alone to write starting at the sheet's A1. Updates to a fixed range are idempotent, so Mechanic retries transient failures automatically.

#### Required Options

* account
* spreadsheet\_id
* rows
* sheet\_range or sheet\_name

#### Optional Options

* value\_input\_option

### clear\_values

Clears the values in a range (formatting is left intact). Provide `sheet_range`, or `sheet_name` alone to clear the entire sheet.

#### Required Options

* account
* spreadsheet\_id
* sheet\_range or sheet\_name

### get\_values

Reads values from a range into the action's result, where a task subscribing to `mechanic/actions/perform` can use them — for example, to find which row holds a particular order before updating or deleting it. When neither `sheet_range` nor `sheet_name` is given, the entire first sheet is read. Results are capped at 20MB; narrow the range for very large sheets.

#### Required Options

* account
* spreadsheet\_id

#### Optional Options

* sheet\_range or sheet\_name
* value\_render\_option ("formatted\_value" by default; "unformatted\_value" returns raw numbers; "formula" returns cell formulas)

### delete\_rows

Deletes a span of rows entirely — rows below shift up. Row numbers are 1-based and inclusive, matching what you see in the Sheets UI.

{% hint style="warning" %}
This operation is destructive, and is never retried automatically: if a deletion fails ambiguously after Google may have applied it, retrying could remove different rows. (Rate-limited rejections are the one exception — Google refuses those before applying anything, so they are safe to retry.) `sheet_name` is always required, so a reordering of tabs can never redirect a deletion to the wrong sheet.
{% endhint %}

#### Required Options

* account
* spreadsheet\_id
* sheet\_name
* start\_row

#### Optional Options

* end\_row (defaults to start\_row, deleting a single row)

### add\_sheet

Adds a new sheet (tab) to an existing spreadsheet, optionally populated with initial rows.

#### Required Options

* account
* spreadsheet\_id
* sheet\_name

#### Optional Options

* rows
* value\_input\_option

### rename\_sheet

Renames a sheet (tab), located by its current name.

#### Required Options

* account
* spreadsheet\_id
* sheet\_name (the current name)
* new\_sheet\_name

### delete\_sheet

Deletes a sheet (tab) by name. A spreadsheet's last remaining sheet cannot be deleted. Like delete\_rows, this operation is never retried automatically.

#### Required Options

* account
* spreadsheet\_id
* sheet\_name

### export\_spreadsheet

Exports a spreadsheet in various formats.

#### Required Options

* account
* spreadsheet\_id

#### Optional Options

* file\_type
  * xlsx (default)
  * csv
  * pdf
  * html
  * ods
  * tsv

## Sheet names and ranges

* Sheet names are matched exactly, and Mechanic looks up the actual sheet names from the spreadsheet — language-specific default names ("Sheet1", "Foglio1", "Hoja1", …) are handled automatically.
* When Mechanic composes a range from a `sheet_name`, names containing spaces or special characters are quoted automatically — pass the plain name (e.g. `"Order Data"`).
* A `sheet_range` without a sheet qualifier (e.g. `"A5:C10"`) combines with `sheet_name` when one is given. A qualified `sheet_range` (e.g. `"Orders!A5:C10"`) that conflicts with `sheet_name` is rejected as an error.

## Authentication

This action requires connecting a Google account with the appropriate permissions. To connect an account:

1. Go to **Settings → Authentication**
2. Select **Google** in the provider list
3. Follow the Google account connection flow

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

### Create a Spreadsheet with Multiple Sheets

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "create_spreadsheet",
    "title": "Sales Report",
    "sheets": [
      {
        "name": "Orders",
        "rows": [["Order ID", "Total"]]
      },
      {
        "name": "Refunds",
        "rows": [["Refund ID", "Amount"]]
      }
    ]
  }
{% endaction %}
```

### Add a Sheet to an Existing Spreadsheet

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "add_sheet",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Q3",
    "rows": [["Date", "Revenue"]]
  }
{% endaction %}
```

### Update Rows at a Specific Range

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "update_rows",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_range": "Orders!A5:C5",
    "rows": [
      ["1001", "John Doe", "129.99"]
    ]
  }
{% endaction %}
```

### Parse Dates and Formulas with user\_entered

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "append_rows",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Totals",
    "value_input_option": "user_entered",
    "rows": [
      ["2026-06-09", 49.99, "=SUM(B:B)"]
    ]
  }
{% endaction %}
```

### Delete Rows

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "delete_rows",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Orders",
    "start_row": 5,
    "end_row": 7
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

### Append Rows with Transient Error Retries

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "append_rows",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Sheet1",
    "retry_on_transient_errors": true,
    "rows": [
      ["Order ID", "Customer Email", "Total"],
      [{{ order.id | json }}, {{ order.email | json }}, {{ order.total_price | json }}]
    ]
  }
{% endaction %}
```

### Read Data from a Google Sheet

Use `get_values` together with a `mechanic/actions/perform` subscription to read data back into a task — for example, as the first pass of a find-then-update workflow.

{% code title="Task subscriptions" %}

```liquid
mechanic/user/trigger
mechanic/actions/perform
```

{% endcode %}

{% code title="Task code" %}

```liquid
{% if event.topic == "mechanic/user/trigger" %}
  {% action "google_sheets" %}
    {
      "account": "user@example.com",
      "operation": "get_values",
      "spreadsheet_id": "1234567890abcdef",
      "sheet_range": "Orders!A1:C100"
    }
  {% endaction %}
{% elsif event.topic == "mechanic/actions/perform" %}
  {% assign sheet_rows = action.run.result.values %}
  {% action "echo" sheet_rows %}
{% endif %}
```

{% endcode %}

### Dynamic Data Example

```liquid
{% assign order_rows = array %}

{% for order in shop.orders %}
  {% assign order_row = array %}
  {% assign order_row[0] = order.name %}
  {% assign order_row[1] = order.customer.name %}
  {% assign order_row[2] = order.total_price %}
  {% assign order_rows[order_rows.size] = order_row %}
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

### create\_spreadsheet Response

```typescript
{
  "spreadsheet_id": string,
  "spreadsheet_url": string,
  "title": string,
  "folder_path": string | null,
  "sheets": [{ "name": string, "sheet_id": number }] // present when created with "sheets"
}
```

Example:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef",
  "title": "Sales Report",
  "folder_path": "reports/2026",
  "sheets": [
    { "name": "Orders", "sheet_id": 0 },
    { "name": "Refunds", "sheet_id": 1234 }
  ]
}
```

### add\_sheet Response

```typescript
{
  "spreadsheet_id": string,
  "sheet_id": number,
  "sheet_name": string,
  "spreadsheet_url": string,
  "updated_rows": number,    // present when rows were written
  "updated_columns": number  // present when rows were written
}
```

### update\_rows Response

```typescript
{
  "spreadsheet_id": string,
  "updated_range": string,
  "updated_rows": number,
  "updated_columns": number,
  "updated_cells": number,
  "spreadsheet_url": string
}
```

### clear\_values Response

```typescript
{
  "spreadsheet_id": string,
  "cleared_range": string,
  "spreadsheet_url": string
}
```

### get\_values Response

```typescript
{
  "spreadsheet_id": string,
  "range": string,
  "values": string[][],
  "row_count": number,
  "spreadsheet_url": string
}
```

### delete\_rows Response

```typescript
{
  "spreadsheet_id": string,
  "sheet_name": string,
  "start_row": number,
  "end_row": number,
  "deleted_rows": number,
  "spreadsheet_url": string
}
```

### rename\_sheet Response

```typescript
{
  "spreadsheet_id": string,
  "sheet_id": number,
  "previous_sheet_name": string,
  "sheet_name": string,
  "spreadsheet_url": string
}
```

### delete\_sheet Response

```typescript
{
  "spreadsheet_id": string,
  "sheet_id": number,
  "sheet_name": string,
  "spreadsheet_url": string
}
```

### export\_spreadsheet Response

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
