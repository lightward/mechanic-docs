# Plaintext

The **plaintext** file generator is used implicitly, when a JSON string is given in place of a standard file generator JSON object. The resulting file will contain the content of the string, with no further processing. This makes the plaintext generator suitable for text files, CSV files, TSV files, and any other file format that can be expressed using plain text.

The plaintext generator cannot be invoked explicitly; `"plaintext"` cannot be used as a named generator type.

## Options

Because this file generator is used implicitly, when a string is given instead of a file generator object, this file generator does not use options.

## Example

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "files" %}
  {
    "plain.txt": "This\nis\na\nmulti-line\nplaintext\nfile."
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
      "plain.txt": "This\nis\na\nmulti-line\nplaintext\nfile."
    }
  }
}
```
{% endtab %}
{% endtabs %}
