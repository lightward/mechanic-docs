# ZIP

The **ZIP** file generator accepts an options object, specifying a set of files (themselves defined using file generators) to be compressed into a single ZIP file. The resulting ZIP file may optionally be password-protected.

## Options

| Option     | Description                                                                 |
| ---------- | --------------------------------------------------------------------------- |
| `files`    | Required; an object specifying a set of filenames mapped to file generators |
| `password` | Optional; a string specifying a password to use for encrypting the file     |

```javascript
{
  "zip": {
    "files": FILENAMES_AND_FILE_GENERATORS,
    "password": PASSWORD
  }
}
```

## Example

{% tabs %}
{% tab title="Liquid" %}
```liquid
{% action "files" %}
  {
    "secure.zip": {
      "zip": {
        "password": "opensesame",
        "files": {
          "confirmations.txt": "this data is protected with zipcrypto encryption",
          "image.png": {
            "url": "https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png"
          },
          "receipt.pdf": {
            "pdf": {
              "html": "<h1>!!</h1>"
            }
          }
        }
      }
    }
  }
{% endaction %}
```
{% endtab %}
{% endtabs %}
