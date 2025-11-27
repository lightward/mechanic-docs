# URL

The **URL** file generator accepts a string as its options, containing a valid URL. This generator downloads the file at that URL, returning the results.

Downloaded files may be a maximum of 20 megabytes, even when used within other file generators (like [ZIP](zip.md)).

## Options

This file generator accepts a string containing a valid HTTP or HTTPS URL. It does not support any other options.

```javascript
{
  "url": URL
}
```

## Example

{% tabs %}
{% tab title="Liquid" %}
```liquid
{% action "files" %}
  {
    "image_from_url.png": {
      "url": "https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png"
    }
  }
{% endaction %}
```
{% endtab %}
{% endtabs %}
