# How do I send images with my emails?

## Attaching images

The [Email action](../core/actions/email.md) supports attaching anything that you can express using JSON. (The rest of this article will assume you're familiar with this action.)

If you happen to have your base64-encoded image data on hand, you can attach it with a line like this:

```javascript
"attachments": {
  "image.jpg": {
    "base64": {{ image_jpg_base64 | json }}
  }
}
```

## Embedding images

You may add image tags in your emails (useful for adding logos!), but please note that Mechanic does not support embedding attached images.

Instead, upload your image to Shopify ([learn how](https://help.shopify.com/en/manual/shopify-admin/productivity-tools/file-uploads#upload-a-file-on-the-files-page)), and use the URL provided by Shopify to add your image using HTML:

```javascript
{% raw %}
{% capture email_body %}
  <p>Welcome!</p>
  <img src="https://cdn.shopify.com/s/files/1/0000/0000/0000/files/welcome_email.jpg">
{% endcapture %}
{% endraw %}
```

```javascript
"body": {{ email_body | unindent | json }}
```
