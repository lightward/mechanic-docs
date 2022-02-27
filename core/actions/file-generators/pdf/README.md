# PDF

The **PDF** file generator accepts an object containing an HTML string, and generates a PDF document using [Pdfcrowd](https://pdfcrowd.com). This service renders the provided HTML using the same rendering engine as Google Chrome, which means that PDFs may be generated with modern CSS and JavaScript, including chart libraries and web fonts.

{% hint style="info" %}
Mechanic accounts created prior to July 12, 2021 use a different rendering engine by default. [Learn about migrating to Pdfcrowd](migrating-to-pdfcrowd.md)
{% endhint %}

## Options

| Option | Description                                                               |
| ------ | ------------------------------------------------------------------------- |
| `html` | Required; a string containing the HTML, CSS and JavaScript to be rendered |
| ...    | Additional Pdfcrowd API options supported; see below                      |

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

If it's unclear why something isn't rendering properly, start by saving and testing the HTML locally. If it seems like Pdfcrowd is doing something unexpected, add `"debug_log": true` to the generator options, run your task, and send the resulting event inspector URL to [team@usemechanic.com](mailto:team@usemechanic.com). The platform team will be able to access the Pdfcrowd logs for your run on your behalf.

## Example

{% tabs %}
{% tab title="Liquid" %}
```javascript
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
