# PDF

The **PDF** file generator accepts an object containing an HTML string, and uses [Pdfcrowd](https://pdfcrowd.com/) to render it as a PDF document. Pdfcrowd employs the [Chromium Embedded Framework](https://en.wikipedia.org/wiki/Chromium_Embedded_Framework) for HTML rendering, which uses the same foundation as Google Chrome. This allows Mechanic to generate PDFs with modern CSS and JavaScript features, including chart libraries and web fonts.

## Options

<table data-header-hidden><thead><tr><th width="145">Option</th><th>Description</th></tr></thead><tbody><tr><td>Option</td><td>Description</td></tr><tr><td><code>html</code></td><td>Required; a string containing the HTML, CSS and JavaScript to be rendered</td></tr><tr><td>...</td><td>Additional Pdfcrowd API options supported; see below</td></tr></tbody></table>

```javascript
{
  "pdf": {
    "html": HTML,
    ...
  }
}
```

## Pdfcrowd options

The PDF generator supports all rendering-related options of the Pdfcrowd API, using version 20.10.

For a complete list of options, see [https://pdfcrowd.com/doc/api/html-to-pdf/http/](https://pdfcrowd.com/doc/api/html-to-pdf/http/).

### Debugging

If it's unclear why something isn't rendering properly, start by testing the HTML being used in a Pdfcrowd playground, at [https://pdfcrowd.com/playground/html-to-pdf](https://pdfcrowd.com/playground/html-to-pdf). If the issue is reproducible in the playground, use the "Help" button along the left-hand sidebar to get the ID of your specific playground, and instructions for contacting Pdfcrowd support with the details of your test.

{% embed url="https://www.loom.com/share/94ea5776e2d84b6d92c1e7ca551e6e92" %}
A screencast illustrating an HTML test, and a path for reaching Pdfcrowd support
{% endembed %}

## Example

{% tabs %}
{% tab title="Liquid" %}
\`\`\`liquid \{% capture html %\}

Almost before we knew it, we had left the ground.

\
&#x20; // from https://plotly.com/javascript/getting-started/\
&#x20; TESTER = document.getElementById('tester');\
&#x9;Plotly.newPlot( TESTER, \[{\
&#x9;x: \[1, 2, 3, 4, 5],\
&#x9;y: \[1, 2, 4, 8, 16] }], {\
&#x9;margin: { t: 0 } } );

\{% endcapture %\}

\{% action "files" %\} { "file.pdf": { "pdf": { "html": \{{ html | json \}}, "page\_width": "7in", "page\_height": "5in", "margin\_top": "10mm", "margin\_right": "10mm", "margin\_bottom": "10mm", "margin\_left": "10mm" } } } \{% endaction %\}

```

</div>

</div>
```
{% endtab %}
{% endtabs %}
