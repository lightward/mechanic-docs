# Google Drive

The Google Drive action allows you to upload files to your Google Drive.

It supports various file types and can generate files dynamically using [file generators](file-generators/), including text files, PDFs, CSVs, and HTML files. Mechanic interacts with Google Drive via the Google Drive API, using OAuth2 for authentication. \


## Options

<table><thead><tr><th width="182.33333333333334">Option</th><th width="88">Type</th><th>Description</th></tr></thead><tbody><tr><td>account</td><td>string</td><td>Required: the Google account email address to authenticate with</td></tr><tr><td>uploads</td><td>hash</td><td>Required: a has specifying files to upload and their contents</td></tr></tbody></table>

### Uploads hash structure

The `uploads` hash supports these properties:

<table><thead><tr><th width="174">Property</th><th width="93">Type</th><th>Description</th></tr></thead><tbody><tr><td>overwrite</td><td>boolean</td><td>Optional: when true, files with matching names will be overwritten. Defaults to false</td></tr><tr><td>[path/filename]</td><td>string | hash</td><td>One or more file paths mapped to their content. Paths can include folders (e.g., 'reports/monthly/file.txt'). Content can be either a direct string or a <a href="file-generators/">file generator object</a>.</td></tr></tbody></table>

## Authentication

This action requires connecting a Google account with the appropriate Drive permissions. To connect an account:

1. Go to the Settings screen
2. Click Authentication
3. Follow the Google account connection flow

## Folder Support

Files can be organized in folders by including path information in the filename:

* Use forward slashes to separate folder names (e.g., "reports/2024/monthly/file.pdf")
* Folders will be created automatically if they don't exist
* Can only access folders created by this integration
* Invalid characters not allowed: `< > : " / \ | ? *`

### Path Examples

```
reports/monthly/report.pdf        # Three levels deep
data/2024/q1/sales.csv           # Four levels deep
archives/backups/files.zip        # Three levels deep
```



## Examples

### Simple Text File Upload

```liquid
{% raw %}
{% action "google_drive" %}
  {
    "account": "user@example.com",
    "uploads": {
      "simple.txt": "Hello world!"
    }
  }
{% endaction %}
{% endraw %}
```

### Multiple Files with Overwrite

```liquid
{% raw %}
{% action "google_drive" %}
  {
    "account": "user@example.com",
    "uploads": {
      "overwrite": true,
      "report.pdf": {
        "pdf": {
          "html": "<h1>Monthly Report</h1><p>This is a PDF generated from HTML</p>"
        }
      },
      "data.csv": "Date,Value\n2024-01-01,100"
    }
  }
{% endaction %}
{% endraw %}
```

### Files in Folders

```liquid
{% raw %}
{% action "google_drive" %}
  {
    "account": "user@example.com",
    "uploads": {
      "overwrite": true,
      "reports/monthly/sales.pdf": {
        "pdf": {
          "html": "<h1>Monthly Sales Report</h1><p>Data for this month</p>"
        }
      },
      "data/exports/stats.csv": "Date,Value\n2024-01-01,100",
      "archive/backups/data.zip": {
        "zip": {
          "files": {
            "readme.txt": "Backup files",
            "data.csv": "id,value\n1,test"
          }
        }
      }
    }
  }
{% endaction %}
{% endraw %}
```

### Dynamic File Generation

```liquid
{% raw %}
{% capture report_content %}
  <h1>{{ shop.name }} - Monthly Report</h1>
  <p>Generated on {{ "now" | date: "%Y-%m-%d" }}</p>
  <ul>
    {% for order in shop.orders %}
      <li>{{ order.name }}</li>
    {% endfor %}
  </ul>
{% endcapture %}

{% action "google_drive" %}
  {
    "account": {{ options.google_account | json }},
    "uploads": {
      "overwrite": true,
      "inventory-report.pdf": {
        "pdf": {
          "html": {{ report_content | strip | json }}
        }
      }
    }
  }
{% endaction %}
{% endraw %}
```

## Action Response

The action returns details about the uploaded files. The response is an object with the following structure:

```json
{
  "uploads": {
    [filepath: string]: {
      "id": string,          // Google Drive file ID
      "name": string,        // File name as stored in Drive
      "mime_type": string,   // MIME type of the uploaded file
      "web_view_link": string, // URL to view the file in Google Drive
      "path": string         // Full folder path where file was created
    }
  }
}
```

### Example response

```json
{
  "uploads": {
    "reports/monthly/report.pdf": {
      "id": "1ABC...xyz",
      "name": "report.pdf",
      "mime_type": "application/pdf",
      "web_view_link": "https://drive.google.com/file/d/1ABC...xyz/view",
      "path": "reports/monthly"
    }
  }
}
```

