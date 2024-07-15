# PDF

The **PDF** file generator accepts an object containing an HTML string, and uses [Pdfcrowd](https://pdfcrowd.com/) to render it as a PDF document. Pdfcrowd employs the [Chromium Embedded Framework](https://en.wikipedia.org/wiki/Chromium\_Embedded\_Framework) for HTML rendering, which uses the same foundation as Google Chrome. This allows Mechanic to generate PDFs with modern CSS and JavaScript features, including chart libraries and web fonts.

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
```liquid
{% raw %}
{% capture html %}
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Liu+Jian+Mao+Cao&display=swap" rel="stylesheet">
<style>p { font-family: 'Liu Jian Mao Cao', cursive; }</style>

<p>Almost before we knew it, we had left the ground.</p>
<div id="tester" style="width:100%;height:40vh;"></div>

<script src="https://cdn.plot.ly/plotly-2.2.0.min.js"></script>
<script>
  // from https://plotly.com/javascript/getting-started/
  TESTER = document.getElementById('tester');
	Plotly.newPlot( TESTER, [{
	x: [1, 2, 3, 4, 5],
	y: [1, 2, 4, 8, 16] }], {
	margin: { t: 0 } } );
</script>
{% endcapture %}

{% action "files" %}
  {
    "file.pdf": {
      "pdf": {
        "html": {{ html | json }},
        "page_width": "7in",
        "page_height": "5in",
        "margin_top": "10mm",
        "margin_right": "10mm",
        "margin_bottom": "10mm",
        "margin_left": "10mm"
      }
    }
  }
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "files",
    "options": {
      "file.pdf": {
        "pdf": {
          "html": "\n<link rel=\"preconnect\" href=\"https://fonts.googleapis.com\">\n<link rel=\"preconnect\" href=\"https://fonts.gstatic.com\" crossorigin>\n<link href=\"https://fonts.googleapis.com/css2?family=Liu+Jian+Mao+Cao&display=swap\" rel=\"stylesheet\">\n<style>p { font-family: 'Liu Jian Mao Cao', cursive; }</style>\n\n<p>Almost before we knew it, we had left the ground.</p>\n<div id=\"tester\" style=\"width:100%;height:40vh;\"></div>\n\n<script src=\"https://cdn.plot.ly/plotly-2.2.0.min.js\"></script>\n<script>\n  // from https://plotly.com/javascript/getting-started/\n  TESTER = document.getElementById('tester');\n\tPlotly.newPlot( TESTER, [{\n\tx: [1, 2, 3, 4, 5],\n\ty: [1, 2, 4, 8, 16] }], {\n\tmargin: { t: 0 } } );\n</script>\n",
          "page_width": "7in",
          "page_height": "5in",
          "margin_top": "10mm",
          "margin_right": "10mm",
          "margin_bottom": "10mm",
          "margin_left": "10mm"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
