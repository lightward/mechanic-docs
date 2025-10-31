# Google Drive and Google Sheets

Mechanic provides a streamlined integration with Google Drive and Google Sheets, allowing you to automate common file management and spreadsheet operations without having to directly interface with the Google Drive API.

### Authentication

{% hint style="info" %}
You can connect more than one Google account choosing which account to use when writing your task code.
{% endhint %}

Before using Google integrations connect your Google accounts:

1. Go to your Mechanic Settings screen
2. Click on Authentication
3. Connect your Google account from the Google tab
4. Grant the necessary permissions when prompted

### Google Drive Integration

The Google Drive action allows you to upload and manage files in Google Drive.\
\
See in-depth documentation of the Google Drive action [here](../../core/actions/integrations/google-drive.md).

#### Basic Usage

```liquid
{% action "google_drive" %}
  {
    "account": "your.email@example.com",
    "uploads": {
      "overwrite": true,
      "simple.txt": "Hello, world!",
      "report.pdf": {
        "pdf": {
          "html": "<h1>Report</h1><p>Content here</p>"
        }
      }
    }
  }
{% endaction %}
```

#### Supported Features

* Upload multiple files in a single action
* Paths can include folders (e.g., 'reports/monthly/file.txt')
* Overwrite existing files (optional)
* Generate various file types (TXT, PDF, CSV, HTML, etc.)  using Mechanic [file-generators](../../core/actions/file-generators/ "mention") or linking from external URLs (e.g. Shopify Files - CDN)

### Google Sheets Integration

The Google Sheets action enables focused interaction with spreadsheets, including sheet creation, appending data, and data exports. \


See in-depth documentation of the Google Sheets action [here](../../core/actions/integrations/google-sheets.md).

#### Basic Usage

```liquid
{% action "google_sheets" %}
  {
    "account": "your.email@example.com",
    "operation": "append_rows",
    "spreadsheet_id": "your-spreadsheet-id",
    "sheet_name": "Sheet1",
    "rows": [
      ["Column1", "Column2", "Column3"],
      ["Value1", "Value2", "Value3"]
    ]
  }
{% endaction %}
```

#### Supported Operations

* `create_spreadsheet`: Create new spreadsheets
* `append_rows`: Add data to existing spreadsheets
* `export_spreadsheet`: Export spreadsheets in various formats

### File Type Support

#### Drive Upload Formats

* Text files
* PDFs (generated from HTML)
* CSV files
* HTML files
* Any binary file format (e.g. ZIP)

#### Sheets Export Formats

* XLSX (Excel)
* CSV
* PDF
* HTML
* ODS
* TSV

### Permissions

The integration requires the `https://www.googleapis.com/auth/drive.file` scope, which allows:

* Access to files created by the app
* Access to files explicitly shared with the app
* No access to other files in your Google Drive
