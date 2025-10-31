# Google

The Google action allows you to interact directly with the Google Drive and Sheets APIs, and is a more advanced integration type than the streamlined [google-drive-and-google-sheets.md](../../../platform/integrations/google-drive-and-google-sheets.md "mention")actions. The advantage is that it allows use to use any of the features in the Google Drive, Sheets, and Docs REST API. Mechanic interacts with Google using OAuth2 for authentication.&#x20;

{% hint style="warning" %}
Utilizing the Google action beyond the demonstration tasks requires reviewing and comprehending the [Google Drive API reference docs](https://developers.google.com/workspace/drive/api/reference/rest/v3). The Mechanic team will not be able to provide developer assistance with this API.
{% endhint %}

The Google action provides a wrapper around HTTP calls to the Google APIs. The various Google API endpoints supported by this integration all share the same top-level structure in the action options.

## Options

<table><thead><tr><th width="182.33333333333334">Option</th><th width="88">Type</th><th>Description</th></tr></thead><tbody><tr><td>account</td><td>string</td><td>Required: the Google account email address to authenticate with. Must match one of the Google accounts linked in the Mechanic authentication settings.</td></tr><tr><td>method</td><td>string</td><td>Required: the HTTP verb as required by the Google API for the specific method (e.g. "GET", "POST", "PATCH")</td></tr><tr><td>url_path</td><td>string</td><td>Required: the API endpoint for the relevant Google Drive service (e.g. "/drive/v3/files")</td></tr><tr><td>headers</td><td>hash</td><td><p>Required: different combinations of API endpoints and usage will dictate which headers are required. Review the Google API docs for more details.</p><p></p><p>Mechanic will use the following headers if they are missing and the request meets certain conditions.</p><pre class="language-json"><code class="lang-json">"Accept": "application/json",
"Content-Type": "application/json"
</code></pre></td></tr><tr><td>body</td><td>hash</td><td>Required: for creating/editing sheets and files, the object that contains a JSON representation of the properties and content of the file; for listing files, this optional object will contain the query parameters used for filtering, pagination, etc.</td></tr></tbody></table>

## Authentication

This action requires connecting a Google account with the appropriate Drive permissions. To connect an account:

1. Go to the Settings screen
2. Click Authentication
3. Follow the Google account connection flow from the Google tab

## Examples

### Create Simple Text File

```liquid
{% action "google" %}
  {
    "account": "user@example.com",
    "method": "POST",
    "url_path": "/upload/drive/v3/files?uploadType=media",
    "headers": {
      "Accept": "application/json",    
      "Content-Type": "text/plain"
    },
    "body": "Hello world!"
  }
{% endaction %}
```

### Update Simple Text File

```liquid
{% action "google" %}
  {
    "account": "user@example.com",
    "method": "PATCH",
    "url_path": "/upload/drive/v3/files/{{ google_file_id }}?uploadType=media",
    "headers": {
      "Accept": "application/json",    
      "Content-Type": "text/plain"
    },
    "body": "Hello again world!"
  }
{% endaction %}
```

### List Drive Files with Name Filter

```liquid
{% action "google" %}
  {
    "account": "user@example.com",
    "method": "GET",
    "url_path": "/drive/v3/files",
    "headers": {
      "Accept": "application/json",
      "Content-Type": "application/json"      
    },
    "body": {
      "q": "name contains 'Mechanic'",
      "pageSize": 10,
      "fields": "files(id,name,mimeType,createdTime)",
      "trashed": false
    }
  }
{% endaction %}
```

### New Sheet with Data Rows

```liquid
{% action "google" %}
  {
    "account": "user@example.com",
    "method": "POST",
    "url_path": "/sheets/v4/spreadsheets",
    "headers": {
      "Accept": "application/json",
      "Content-Type": "application/json"
    },
    "body": {
      "properties": {
        "title": "Sheet File Title"
      },
      "sheets": [
        {
          "properties": {
            "title": "Sheet Tab Name"
          },
          "data": [
            {
              "rowData": [
                {
                  "values": [
                    {
                      "userEnteredValue": {
                        "stringValue": "Order name"
                      }
                    },
                    {
                      "userEnteredValue": {
                        "stringValue": "Date"
                      }
                    }
                  ]
                {
                  "values": [
                    {
                      "userEnteredValue": {
                        "stringValue": "#1357"
                      }
                    },
                    {
                      "userEnteredValue": {
                        "stringValue": "2025-10-05T12:00:00Z"
                      }
                    }
                  ]
                }
              ]
            }
          ]
        }
      ]
    }
  }
{% endaction %}
```

## Action Response

The body of the action response will vary based on which API endpoint is called. Generally, the response is an object with the following structure (most fields removed for brevity). Running a Google action task and reviewing the response is often the best way to see what will be returned in the body.

```json
{
  "type": "google",
  "run": {
    "ok": true,
    "result": {
      "status": 200,
      "body": {
        ...
      }
    }
  }
}
```
