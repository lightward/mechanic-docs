# Migrating to Pdfcrowd

Mechanic accounts created prior to July 12, 2021 default to using [wkhtmltopdf](https://wkhtmltopdf.org/) instead of [Pdfcrowd](https://pdfcrowd.com/). This rendering engine uses a version of WebKit from 2012, and therefore does not support many features of the modern web.

We strongly encourage all users to migrate to Pdfcrowd, which uses a modern release of Chrome for rendering HTML.

{% hint style="info" %}
This page is about migrating to Pdfcrowd. To learn more about using Pdfcrowd, see the [PDF file generator](./) documentation.
{% endhint %}

## Switching to Pdfcrowd

For accounts created prior to July 12, 2021, an option labeled "Opt in to Pdfcrowd" is available in the Mechanic account settings. To start using Pdfcrowd for the entire account, enable this option, and click the "Save settings" button.

![](<../../../../.gitbook/assets/Screen Shot 2022-05-05 at 11.54.42 AM.png>)

Or, to start using Pdfcrowd with just a single file generator, add `"__force_pdfcrowd": true` to the PDF generator options.

```javascript
{% raw %}
{% action "files" %}
  {
    "migration_test.pdf": {
      "html": "<h1>hello world!</h1>",
      "__force_pdfcrowd": true
    }
  }
{% endaction %}
{% endraw %}
```

### Pdfcrowd options

See [https://pdfcrowd.com/doc/api/html-to-pdf/http/](https://pdfcrowd.com/doc/api/html-to-pdf/http/) for a complete list of rendering options supported by Pdfcrowd.

### wkhtmltopdf options

For reference while migrating, these are the PDF generator options supported by wkhtmltopdf:

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

##
