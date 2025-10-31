# Slack

Connect Mechanic to your [Slack](https://slack.com/) workspace, so that you can post messages from Mechanic to a channel in your workspace. New order notifications? [Error Notifications](../error-handling.md)? The options are endless. You now have a Slack to Shopify integration.

### Authentication

{% hint style="info" %}
You can install the Mechanic Slack app on more than one Slack workspace, choosing which account to use when writing your task code.
{% endhint %}

Before using the Slack integration install the Mechanic Slack app in your Slack workspace:

1. Go to your Mechanic Settings screen
2. Click on Authentication
3. Install the Mechanic Slack app from the Slack tab

### Basic Usage

See full usage [here](../../core/actions/integrations/slack.md).

```liquid
{% action "slack" %}
  {
    "account": "SLACK_ACCOUNT_NAME",
    "method": "POST",
    "url_path": "/chat.postMessage",
    "headers": {
      "Content-Type": "application/json"
    },
    "body": {
      "channel": "CHANNEL_ID",
      "text": "Slack Example Message",
      "blocks": [
        {
          "type": "section",
          "text": {
            "type": "mrkdwn",
            "text": "Lorem ipsum dolor sit amet consectetur adipiscing elit."
          }
        }
      ]
    }
  }
{% endaction %}
```
