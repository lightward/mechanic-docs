# Google Sheets

The Google Sheets action lets tasks create, read, write, and manage Google Sheets spreadsheets. Mechanic interacts with Google Sheets via the Google Sheets API, using OAuth2 for authentication — see [Authentication](#authentication).

{% hint style="info" %}
Due to Google security restrictions, Mechanic can only access spreadsheets that were created through Mechanic itself — no other spreadsheets in your drive. To work with Google Sheets:

* First create a spreadsheet using the `"create_spreadsheet"` operation
* Store the returned spreadsheet ID for later use
* Then use the other operations on this spreadsheet

See this great [example](https://tasks.mechanic.dev/demonstration-add-new-orders-to-google-sheet) in the task library.
{% endhint %}

## Operations

| Operation                                       | Use it to                                                  |
| ----------------------------------------------- | ---------------------------------------------------------- |
| [`create_spreadsheet`](#create_spreadsheet)     | Create a spreadsheet, with one or many named sheets        |
| [`append_rows`](#append_rows)                   | Add rows to the end of a sheet's data                      |
| [`update_rows`](#update_rows)                   | Overwrite rows at a specific range                         |
| [`clear_values`](#clear_values)                 | Clear a range, or a whole sheet                            |
| [`get_values`](#get_values)                     | Read values into the action result                         |
| [`delete_rows`](#delete_rows)                   | Delete a span of rows (rows below shift up)                |
| [`add_sheet`](#add_sheet)                       | Add a sheet (tab) to an existing spreadsheet               |
| [`rename_sheet`](#rename_sheet)                 | Rename a sheet (tab)                                       |
| [`delete_sheet`](#delete_sheet)                 | Delete a sheet (tab)                                       |
| [`export_spreadsheet`](#export_spreadsheet)     | Export the spreadsheet as XLSX, CSV, PDF, HTML, ODS, or TSV |

Every operation requires `"account"` (the email address of a [connected Google account](#authentication)), and every operation except `create_spreadsheet` requires `"spreadsheet_id"`. Each operation's remaining options are listed in its section below.

## Targeting sheets and ranges

These conventions apply across the operations:

* **`sheet_name`** targets a sheet (tab) by its exact name. Where it's optional (`append_rows`, `update_rows`, `clear_values`, `get_values`), omitting it targets the spreadsheet's **first sheet** — Mechanic looks up the actual name, so language-specific defaults ("Sheet1", "Foglio1", "Hoja1", …) are handled automatically. Pass plain names (e.g. `"Order Data"`); Mechanic quotes them for A1 notation as needed.
* **`sheet_range`** is an A1-notation range (e.g. `"Orders!A2:C10"`). A range without a sheet qualifier (e.g. `"A2:C10"`) combines with `sheet_name` when one is given. A qualified range that conflicts with `sheet_name` is rejected as an error.

## Writing values

Operations that write data (`create_spreadsheet`, `append_rows`, `update_rows`, `add_sheet`) accept `"value_input_option"`:

* `"raw"` (default) — values are stored exactly as given
* `"user_entered"` — values are parsed as if typed into the Sheets UI: dates become dates, numbers become numbers, and strings starting with `=` become formulas

{% hint style="warning" %}
**Only use `"user_entered"` with data you trust.** Under `user_entered`, any cell value beginning with `=` executes as a live formula in the spreadsheet. Customer-supplied values (order notes, names, line item properties) should be written with the default `"raw"` mode.
{% endhint %}

## Retries

* **Reads and idempotent writes** (`get_values`, `update_rows`, `clear_values`, `rename_sheet`) are retried automatically on transient errors.
* **Ambiguous writes** (`append_rows`, `create_spreadsheet`, `add_sheet`, `delete_rows`, `delete_sheet`) are not retried automatically, because a timeout may occur after Google already applied the change — retrying could duplicate rows or delete the wrong ones. `append_rows` supports opting in via `"retry_on_transient_errors": true`.
* **Rate limits** (HTTP 429) are always retried automatically, for every operation: Google rejects rate-limited requests before applying anything, so they're safe.

***

## create\_spreadsheet

Creates a new spreadsheet — with a single sheet (optionally populated via `rows`), or with multiple named sheets via `sheets`.

| Option                 | Required | Notes                                                                                       |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------- |
| `title`                | no       | Defaults to "New Spreadsheet"                                                                |
| `rows`                 | no       | Array of arrays; initial data for the default sheet. Mutually exclusive with `sheets`        |
| `sheets`               | no       | Array of `{"name": ..., "rows": [...]}` objects, to create multiple named sheets             |
| `folder_path`          | no       | Folder to create the spreadsheet in — see [Folders](#folders)                                |
| `value_input_option`   | no       | See [Writing values](#writing-values)                                                        |

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "create_spreadsheet",
    "title": "Monthly Sales Report",
    "rows": [
      ["Month", "Revenue", "Expenses", "Profit"],
      ["January", "5000", "3000", "2000"]
    ]
  }
{% endaction %}
```

With multiple sheets:

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "create_spreadsheet",
    "title": "Sales Report",
    "sheets": [
      { "name": "Orders", "rows": [["Order ID", "Total"]] },
      { "name": "Refunds", "rows": [["Refund ID", "Amount"]] }
    ]
  }
{% endaction %}
```

Response:

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

(`folder_path` is null when not given; `sheets` is present when created with `sheets`.)

## append\_rows

Adds new rows after a sheet's existing data.

| Option                       | Required | Notes                                                                      |
| ---------------------------- | -------- | --------------------------------------------------------------------------- |
| `rows`                       | yes      | Array of arrays                                                              |
| `sheet_name`                 | no       | Defaults to the first sheet — see [Targeting](#targeting-sheets-and-ranges)  |
| `sheet_range`                | no       | An explicit A1 range to append within                                        |
| `value_input_option`         | no       | See [Writing values](#writing-values)                                        |
| `retry_on_transient_errors`  | no       | Defaults to false — see [Retries](#retries)                                  |

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "append_rows",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Orders",
    "rows": [
      ["Order ID", "Customer", "Total"],
      ["1001", "John Doe", "99.99"]
    ]
  }
{% endaction %}
```

Building rows dynamically:

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

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "updated_range": "Orders!A1:C3",
  "updated_rows": 3,
  "updated_columns": 3,
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

## update\_rows

Writes rows at a specific range, overwriting whatever is there.

| Option                | Required               | Notes                                                                 |
| --------------------- | ---------------------- | ---------------------------------------------------------------------- |
| `rows`                | yes                    | Array of arrays                                                         |
| `sheet_range`         | one of these two       | e.g. `"Orders!A5:C5"`                                                   |
| `sheet_name`          | one of these two       | Alone, writes starting at the sheet's A1                                |
| `value_input_option`  | no                     | See [Writing values](#writing-values)                                   |

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "update_rows",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_range": "Orders!A5:C5",
    "rows": [["1001", "John Doe", "129.99"]]
  }
{% endaction %}
```

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "updated_range": "Orders!A5:C5",
  "updated_rows": 1,
  "updated_columns": 3,
  "updated_cells": 3,
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

## clear\_values

Clears the values in a range; formatting is left intact.

| Option         | Required          | Notes                                |
| -------------- | ----------------- | ------------------------------------- |
| `sheet_range`  | one of these two  | The range to clear                    |
| `sheet_name`   | one of these two  | Alone, clears the entire sheet        |

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "clear_values",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_range": "Orders!A2:C100"
  }
{% endaction %}
```

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "cleared_range": "Orders!A2:C100",
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

## get\_values

Reads values from a range into the action's result. Results are capped at 20MB; narrow the range for very large sheets.

| Option                 | Required | Notes                                                                                                 |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| `sheet_range`          | no       | Defaults to the entire first sheet when neither this nor `sheet_name` is given                           |
| `sheet_name`           | no       | Alone, reads the entire named sheet                                                                      |
| `value_render_option`  | no       | `"formatted_value"` (default), `"unformatted_value"` (raw numbers), or `"formula"` (cell formulas)       |

Pair it with a `mechanic/actions/perform` subscription to use the values in a task — for example, finding which row holds a particular order before updating or deleting it:

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

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "range": "Orders!A1:C100",
  "values": [["Order ID", "Customer", "Total"], ["1001", "John Doe", "99.99"]],
  "row_count": 2,
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

## delete\_rows

Deletes a span of rows entirely — rows below shift up. Row numbers are 1-based and inclusive, matching what you see in the Sheets UI.

{% hint style="warning" %}
This operation is destructive, and is never retried automatically — see [Retries](#retries). `sheet_name` is always required, so a reordering of tabs can never redirect a deletion to the wrong sheet.
{% endhint %}

| Option       | Required | Notes                                          |
| ------------ | -------- | ----------------------------------------------- |
| `sheet_name` | yes      |                                                 |
| `start_row`  | yes      | 1-based                                         |
| `end_row`    | no       | Defaults to `start_row` (deletes a single row)  |

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

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "sheet_name": "Orders",
  "start_row": 5,
  "end_row": 7,
  "deleted_rows": 3,
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

## add\_sheet

Adds a new sheet (tab) to an existing spreadsheet, optionally populated with initial rows.

| Option                | Required | Notes                                  |
| --------------------- | -------- | --------------------------------------- |
| `sheet_name`          | yes      | Must not already exist                  |
| `rows`                | no       | Initial data for the new sheet          |
| `value_input_option`  | no       | See [Writing values](#writing-values)   |

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

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "sheet_id": 5678,
  "sheet_name": "Q3",
  "updated_rows": 1,
  "updated_columns": 2,
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

(`updated_rows`/`updated_columns` are present when `rows` were written.)

## rename\_sheet

Renames a sheet (tab), located by its current name.

| Option            | Required | Notes                       |
| ----------------- | -------- | ---------------------------- |
| `sheet_name`      | yes      | The current name             |
| `new_sheet_name`  | yes      | Must not already exist       |

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "rename_sheet",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Q3",
    "new_sheet_name": "Q3 Archive"
  }
{% endaction %}
```

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "sheet_id": 5678,
  "previous_sheet_name": "Q3",
  "sheet_name": "Q3 Archive",
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

## delete\_sheet

Deletes a sheet (tab) by name. A spreadsheet's last remaining sheet cannot be deleted. Like `delete_rows`, this is never retried automatically — see [Retries](#retries).

| Option       | Required |
| ------------ | -------- |
| `sheet_name` | yes      |

```liquid
{% action "google_sheets" %}
  {
    "account": "user@example.com",
    "operation": "delete_sheet",
    "spreadsheet_id": "1234567890abcdef",
    "sheet_name": "Q3 Archive"
  }
{% endaction %}
```

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "sheet_id": 5678,
  "sheet_name": "Q3 Archive",
  "spreadsheet_url": "https://docs.google.com/spreadsheets/d/1234567890abcdef"
}
```

## export\_spreadsheet

Exports a spreadsheet. The exported file arrives base64-encoded in the action result.

| Option      | Required | Notes                                                          |
| ----------- | -------- | --------------------------------------------------------------- |
| `file_type` | no       | `"xlsx"` (default), `"csv"`, `"pdf"`, `"html"`, `"ods"`, `"tsv"` |

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

Response:

```json
{
  "spreadsheet_id": "1234567890abcdef",
  "name": "Monthly Sales Report",
  "size": 12345,
  "file_type": "pdf",
  "data_base64": "base64encodeddata..."
}
```

***

## Authentication

This action requires connecting a Google account with the appropriate permissions. To connect an account:

1. Go to **Settings → Authentication**
2. Select **Google** in the provider list
3. Follow the Google account connection flow

## Folders

When creating spreadsheets, use `folder_path` to organize your files:

* Use forward slashes to separate folder names (e.g., `"reports/2026/monthly"`)
* Folders will be created if they don't exist
* Mechanic can only access folders created by this integration
* Invalid characters not allowed: `< > : " / \ | ? *`
