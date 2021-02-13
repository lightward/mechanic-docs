# PDF

The **PDF** file generator accepts an options object, containing at minimum an HTML string, and possibly containing supported options for rendering. This generator renders the provided HTML in a complete Webkit browser, and saves the result as a PDF document.

Because a complete Webkit browser is used for rendering, PDFs may be generated with complex CSS and JavaScript, including dynamically loaded fonts and rendered charts.

## Options

| Option | Description |
| :--- | :--- |
| `html` | Required; a string containing the HTML, CSS and JavaScript to be rendered |
|  |  |

```javascript
{
  "base64": {
    "html": HTML,
    ...
  }
}
```

## Example

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% action "files" %}
  {
    "receipt.pdf": {
      "html": "<h1>ohai</h1>"
    }
  }
{% endaction %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "files",
    "options": {
      "receipt.pdf": {
        "html": "<h1>ohai</h1>"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}



