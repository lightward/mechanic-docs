# Judge.me

### What is Judge.me?

[Judge.me](https://judge.me/) helps you collect and display reviews about your products and Shopify store. This increases your conversion rate, organic traffic, and buyer engagement by leveraging your user-generated content.

### Judge.me --> Mechanic

Judge.me sends Mechanic events when a new review is created (judgeme/review/create) and when a review is updated (judgeme/review/updated).

The data for each review is available in an [environment variable](../../core/tasks/code/environment-variables.md) called `review`. Its contents exactly mirror `event.data.review`.

#### Event data samples

{% code title="judgeme/review/created" %}
```json
{
  "event": "review/created",
  "platform": "shopify",
  "review": {
    "body": "fulfiment",
    "created_at": "2019-05-02T08:39:09+00:00",
    "curated": "ok",
    "featured": false,
    "hidden": false,
    "id": 434,
    "ip_address": null,
    "pictures": [
      {
        "urls": {
          "small": "https://small",
          "compact": "https://compact",
          "huge": "https://huge",
          "original": "https://original"
        },
        "hidden": false
      }
    ],
    "product_external_id": 0,
    "rating": 5,
    "reviewer": {
      "accepts_marketing": true,
      "email": "o+import11@o.com",
      "external_id": null,
      "id": 602,
      "name": "ob ",
      "phone": null,
      "source_email": "o@o.com",
      "tags": null,
      "unsubscribed_at": null
    },
    "source": "admin",
    "title": null,
    "updated_at": "2019-05-02T08:39:09+00:00",
    "verified": "not-yet"
  },
  "shop_domain": "foobar.myshopify.com"
}
```
{% endcode %}

{% code title="judgeme/review/updated" %}
```json
{
  "event": "review/updated",
  "platform": "shopify",
  "review": {
    "body": "fulfiment",
    "created_at": "2019-05-02T08:39:09+00:00",
    "curated": "ok",
    "featured": false,
    "hidden": false,
    "id": 434,
    "ip_address": null,
    "pictures": [
      {
        "urls": {
          "small": "https://small",
          "compact": "https://compact",
          "huge": "https://huge",
          "original": "https://original"
        },
        "hidden": false
      }
    ],
    "product_external_id": 0,
    "rating": 5,
    "reviewer": {
      "accepts_marketing": true,
      "email": "o+import11@o.com",
      "external_id": null,
      "id": 602,
      "name": "ob ",
      "phone": null,
      "source_email": "o@o.com",
      "tags": null,
      "unsubscribed_at": null
    },
    "source": "admin",
    "title": null,
    "updated_at": "2019-05-02T08:39:09+00:00",
    "verified": "not-yet"
  },
  "shop_domain": "foobar.myshopify.com"
}
```
{% endcode %}

{% hint style="info" %}
Requires enabling the Mechanic integration within Judge.me; see [Judge.me's integration announcement](https://blog.judge.me/blog/new-integration-mechanic-app).
{% endhint %}
