# File generators

**File generators** are invoked by [**actions**](../) to create new files, using options provided by the action, and handing the resulting file back to the action for further use. In this way, [**tasks**](../../tasks/) can make choices about what files to generate, and what to do with the results.

| File generator | Purpose |
| :--- | :--- |
| [Base64](base64.md) | Decodes base64-encoded content, returning a file containing the results |
| [PDF](pdf.md) | Renders HTML using a full Webkit browser, returning a PDF file of the results |
| [URL](url.md) | Downloads and returns a file |
| [ZIP](zip.md) | Accepts its own set of file generators, returning a ZIP archive of the results |

## Usage

File generator objects, like [action objects](../../tasks/code/action-objects.md), are plain JSON objects each having a single key, and a single value. The object key specifies which file generator is to be invoked; the object value contains the options used for that generator.

```text
{
  FILE_GENERATOR_TYPE: FILE_GENERATOR_OPTIONS
}
```

In practice, file generator objects are given as values in a larger JSON object, in which filenames are mapped to file generators.

In the following example, a [Files](../action-types/files.md) action is defined, mapping filenames \(`"invoice.pdf"` and `"external.jpg"`\) to file generators \(a PDF generator, and a URL generator\). Note how the options provided to each generator vary, based on the specific file generator invoked.

```javascript
{% action "files" %}
  {
    "invoice.pdf": {
      "pdf": {
        "html": "<h1>Order #12345</h1>\n<p>It's due!</p>"
      }
    },
    "external.jpg": {
      "url": "https://upload.wikimedia.org/wikipedia/en/a/a9/Example.jpg"
    }
  }
{% endaction %}
```

## Supported actions

| Action | Usage |
| :--- | :--- |
| [Email](../action-types/email.md) | Uses file generators to prepare email attachments |
| [Files](../action-types/files.md) | Uses file generators to prepare temporary URLs, from which the generated files can be downloaded |
| [FTP](../action-types/ftp.md) | Uses file generators to prepare FTP uploads |

