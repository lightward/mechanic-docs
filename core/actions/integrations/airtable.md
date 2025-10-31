# Airtable

The Airtable action allows you to create and update tables and records in your Airtable bases.

It provides an authenticated client for HTTP calls to the [Airtable API](https://airtable.com/developers/web/api). The various Airtable API methods supported by this integration all share the same top-level structure in the action options.

## Options

<table><thead><tr><th width="182.33333333333334">Option</th><th width="88">Type</th><th>Description</th></tr></thead><tbody><tr><td>account</td><td>string</td><td>Required: the Airtable account to use. Must match one of the Airtable accounts linked in the Mechanic authentication settings.</td></tr><tr><td>method</td><td>string</td><td>Required: the HTTP verb as required by the Airtbale API for the specific method (e.g. "GET", "POST")</td></tr><tr><td>url_path</td><td>string</td><td>Required: the Airtbale API endpoint (e.g. "/v0/meta/bases/AIRTABLE_BASE_ID/tables")</td></tr><tr><td>headers</td><td>hash</td><td>Required: "Content-Type": "application/json"</td></tr><tr><td>body</td><td>hash</td><td>Required: the object that contains a JSON representation of the properties and content of the table, record, or fields being modified</td></tr></tbody></table>

### Supported API Permissions

This integration supports these Airtable API scopes.

* Read/Write Records
* Read/Write Comments
* Read/Write Base Schema

## Authentication

This action requires connecting an Airtable account with the appropriate permissions. To connect an account:

1. Go to the Settings screen
2. Click Authentication
3. Follow the Airtable account connection flow from the Airtable tab

## Examples

### Create a New Table

```liquid
{% action "airtable" %}
  {
    "account": "AIRTABLE_ACCOUNT_NAME",
    "method": "POST",
    "url_path": "/v0/meta/bases/AIRTABLE_BASE_ID/tables",
    "headers": {
      "Content-Type": "application/json"
    },
    "body": {
      "name": "Example Checklist",

      "fields": [
        {
          "name": "TODO",
          "type": "singleLineText"
        },
        {
          "name": "Complete",
          "type": "checkbox",
          "options": {
            "color": "greenBright",
            "icon": "check"
          }
        }
      ]
    }
  }
{% endaction %}
```

### Add Records to a Table

```liquid
{% action "airtable" %}
  {
    "account": "AIRTABLE_ACCOUNT_NAME",
    "method": "POST",
    "url_path": "/v0/meta/bases/AIRTABLE_BASE_ID/AIRTABLE_TABLE_ID",
    "headers": {
      "Content-Type": "application/json"
    },
    "body": {
      "records": [
        {
          "fields": {
            "TODO": "Review Mechanic",
            "Complete": false
          }
        }
      ]
    }
  }
{% endaction %}
```

## Action Response

The body of the action response will vary based on which Airtable API method was called. Generally, the response is an object with the following structure (most fields removed for brevity). Running an Airtable action task and reviewing the response is often the best way to see what will be returned in the body.

```json
{
  "type": "airtable",
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
