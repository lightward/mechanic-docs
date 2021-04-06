# PDF

The **PDF** file generator accepts an options object, containing at minimum an HTML string, and possibly containing supported options for rendering. This generator renders the provided HTML in a complete Webkit browser, and saves the result as a PDF document.

Because a complete Webkit browser is used for rendering, PDFs may be generated with complex CSS and JavaScript, including dynamically loaded fonts and rendered charts.

## Options

| Option | Description |
| :--- | :--- |
| `html` | Required; a string containing the HTML, CSS and JavaScript to be rendered |
| ... | Additional wkhtmltopdf options supported; see below |

```javascript
{
  "base64": {
    "html": HTML,
    ...
  }
}
```

### Additional wkhtmltopdf options

This generator creates uses [wkhtmltopdf](https://wkhtmltopdf.org/) internally, and supports additional options as defined in [wkhtmltopdf's documentation](https://wkhtmltopdf.org/usage/wkhtmltopdf.txt):

collate, no-collate, grayscale, image-dpi, image-quality, lowquality, margin-bottom, margin-left, margin-right, margin-top, orientation, page-height, page-size, page-width, no-pdf-compression, title, outline, no-outline, outline-depth, background, no-background, default-header, encoding, disable-external-links, enable-external-links, disable-forms, enable-forms, images, no-images, load-media-error-handling, minimum-font-size, exclude-from-outline, include-in-outline, page-offset, disable-smart-shrinking, enable-smart-shrinking, disable-toc-back-links, enable-toc-back-links, zoom, footer-center, footer-font-name, footer-font-size, footer-html, footer-left, footer-line, no-footer-line, footer-right, footer-spacing, header-center, header-font-name, header-font-size, header-html, header-left, header-line, no-header-line, header-right, header-spacing, replace, disable-dotted-lines, toc-header-text, toc-level-indentation, disable-toc-links, toc-text-size-shrink

To use additional options from the list above, add them alongside the `html` option:

```javascript
{
  "pdf": {
    "html": "<h1>This is a document.</h1>",
    "page-width": "6in",
    "page-height": "6in",
    "margin-top": "0.75in",
    "margin-right": "0.75in",
    "margin-bottom": "0.75in",
    "margin-left": "0.75in"
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
      "pdf": {
        "html": "<h1>ohai</h1>"
      }
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
        "pdf": {
          "html": "<h1>ohai</h1>"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

