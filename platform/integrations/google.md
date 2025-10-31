# Google

Mechanic provides powerful integration with the Google APIs, including Google Drive, Sheets and Docs, allowing you to automate file management, spreadsheet operations, and more. Using the Google action is more complex than using the [google-drive-and-google-sheets.md](google-drive-and-google-sheets.md "mention")action, but the advantage is that it allows use to use any of the features in the Google Drive, Sheets, and Docs REST API.

### Supported APIs

* [Google Docs](https://developers.google.com/workspace/docs/api/reference/rest)
* [Google Drive](https://developers.google.com/workspace/drive/api/reference/rest/v3)
* [Google Sheets](https://developers.google.com/workspace/sheets/api/reference/rest/)

### Authentication

{% hint style="info" %}
You can connect more than one Google account choosing which account to use when writing your task code.
{% endhint %}

Before using Google integrations connect your Google accounts:

1. Go to your Mechanic Settings screen
2. Click on Authentication
3. Connect your Google account from the Google tab
4. Grant the necessary permissions to Mechanic when prompted (there will likely be multiple prompts)

### Google Integration

The Google action allows you to use the Google Drive, Google Docs, and Google Sheets APIs.\
\
See in-depth documentation of the Google action [here](../../core/actions/integrations/google.md).

#### Basic Usage

```liquid
{% action "google" %}
  {
    "account": "your.email@example.com",
    "method": "GET",
    "url_path": "/drive/v3/files",
    "headers": {
      "Accept": "application/json"
    },
    "body": {
      "pageSize": 5,
      "fields": "files(id,name,mimeType)"
    }
  }
{% endaction %}
```

{% hint style="info" %}
#### The GET method in the Google action differs slightly from conventional HTTP GET usage, in that parameters may be passed in a body object in lieu of appending a query string to the URL.
{% endhint %}

{% hint style="warning" %}
#### If any traditional query string parameters are included in the URL path, then any key-value pairs in the body object will be ignored.
{% endhint %}

### Permissions

The integration requires the `https://www.googleapis.com/auth/drive.file` scope, which allows:

* Access to files created by the app
* Access to files explicitly shared with the app
* No access to other files in your Google Drive
