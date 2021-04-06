# hash

The `hash` keyword literal may be used in any Liquid code to instantiate a [hash](../../basics/types.md#hash).

Hashes support [assignment by key](../tags/assign.md#assigning-into-arrays). The [keys](../../filters.md#keys) and [values](../../filters.md#values) filters may be used to quickly an array of hash keys or values, respectively.

{% hint style="info" %}
The term "hash" comes from Ruby. In Ruby \([docs](https://ruby-doc.org/core-2.5.1/Hash.html)\), "A Hash is a dictionary-like collection of unique keys and their values."

In Mechanic's Liquid implementation, a hash can only have string keys.
{% endhint %}

## Example

```javascript
{% assign sizes = hash %}
{% assign sizes["S"] = "Small" %}
{% assign sizes["M"] = "Medium" %}
{% assign sizes["L"] = "Large" %}

{% assign size_abbreviations = sizes | keys %}
{% assign size_labels = sizes | values %}

{% for keyval in sizes %}
  {% assign size_abbreviation = keyval[0] %}
  {% assign size_label = keyval[1] %}

  {{ size_abbreviation }}: {{ size_label }}
{% endfor %}
```

