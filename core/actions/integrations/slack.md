# Slack

The Slack action allows Mechanic to post messages to public and private channels in your Slack instance (as the Mechanic app bot or a customer username).

The Slack action provides a wrapper around HTTP calls to the [Slack Web API](https://docs.slack.dev/apis/web-api/). The various Slack API methods supported by this integration all share the same top-level structure in the action options.

## Options

<table><thead><tr><th width="182.33333333333334">Option</th><th width="88">Type</th><th>Description</th></tr></thead><tbody><tr><td>account</td><td>string</td><td>Required: the Slack account to use. Must match one of the Slack accounts linked in the Mechanic authentication settings.</td></tr><tr><td>method</td><td>string</td><td>Required: the HTTP verb as required by the Slack API for the specific method (e.g. "GET", "POST")</td></tr><tr><td>url_path</td><td>string</td><td>Required: the Slack API method (e.g. "/chat.postMessage")</td></tr><tr><td>headers</td><td>hash</td><td>Required: "Content-Type": "application/json"</td></tr><tr><td>body</td><td>hash</td><td>Required: the object that contains a JSON representation of the properties and content of the message</td></tr></tbody></table>

### Supported API Methods

Currently, these are the only Slack API methods supported by this integration.

* [chat.postMessage](https://docs.slack.dev/reference/methods/chat.postmessage)
* [chat.update](https://docs.slack.dev/reference/methods/chat.update/)
* [chat.delete](https://docs.slack.dev/reference/methods/chat.delete/)

## Authentication

This action requires installing the Mechanic Slack app in your Slack account with the appropriate permissions. To install the app:

1. Go to the Settings screen
2. Click Authentication
3. Install the Mechanic Slack app from the Slack tab

## Examples

### Post a Simple Message

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

### Post a Message with Custom Username and Icon

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
      "username": "AiRobot",
      "icon_emoji": ":robot_face:",
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

## Action Response

The body of the action response will vary based on which Slack API method is called. Generally, the response is an object with the following structure (most fields removed for brevity). Running a Slack action task and reviewing the response is often the best way to see what will be returned in the body.

```json
{
  "type": "slack",
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
