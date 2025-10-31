# Airtable

Connect Mechanic to your [Airtable](https://airtable.com/) bases! You can then use any of the features in the [Airtable API](https://airtable.com/developers/web/api/introduction). You now have a full Shopify to Airtable integration.

### Authentication

{% hint style="info" %}
You can connect more than one Airtable account choosing which account to use when writing your task code.
{% endhint %}

Before using Airtable integrations connect your Airtable accounts:

1. Go to your Mechanic Settings screen
2. Click on Authentication
3. Connect your Airtable account from the Airtable tab

### Basic Usage

See full usage [here](../../core/actions/integrations/airtable.md).

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
