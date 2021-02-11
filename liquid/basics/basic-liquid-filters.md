# Filters

We can change the output of a Liquid object by applying a filter. Filters are separated by a `|`. The full range of filters are covered in the Shopify docs [here](https://shopify.github.io/liquid/basics/introduction/) and [here](https://shopify.dev/docs/themes/liquid/reference/filters).

### Example usage of the upcase filter

```javascript
{{ order.billing_address.zip | upcase }}
```

## Default filter

This filter set a default value when a variable is either `nil`, `false`, or an empty string `""`.

```javascript
Howdy {{ order.shipping_address.first_name | default: "partner" }}
```

## String filters

String filters are used to change the output and variables of type String. We've covered the some of the most popular ones, but make sure to look at the full docs [here](https://shopify.dev/docs/themes/liquid/reference/filters/string-filters).

### append filter

{% tabs %}
{% tab title="Code" %}
```javascript
{{ 'report' | append: '.pdf' }}
```
{% endtab %}

{% tab title="Output" %}
```
report.pdf
```
{% endtab %}
{% endtabs %}

### prepend filter



{% tabs %}
{% tab title="Code" %}
```javascript
{{ 'great to meet you.' | prepend: 'It was ' }}

```
{% endtab %}

{% tab title="Output" %}
```
It was geat to meet you
```
{% endtab %}
{% endtabs %}

### upcase and downcase filters

{% tabs %}
{% tab title="Code" %}
```javascript
{{ 'stop yelling it at me' | upcase }}
{{ 'STOP YELLING AT ME' | downcase }}

```
{% endtab %}

{% tab title="Output" %}
```
STOP YELLING AT ME
stop yelling at me
```
{% endtab %}
{% endtabs %}

### remove and remove\_first filter

Removes substring from a string.

{% tabs %}
{% tab title="Code" %}
```javascript
{{ "Hi everyone! Nice to meet everyone!" | remove: "everyone" }}
{{ "Hi everyone! Nice to meet everyone!" | remove_first: "everyone" }}
```
{% endtab %}

{% tab title="Output" %}
```
Hi ! Nice to meet !
Hi ! Nice to meet everyone!
```
{% endtab %}
{% endtabs %}

### replace and replace\_first filter

{% tabs %}
{% tab title="Code" %}
```javascript
{{ "Matt and Megan love to travel and travel." | replace: 'travel', 'party' }}
{{ "Matt and Megan love to travel and travel | replace_first : 'travel', 'party' }}

```
{% endtab %}

{% tab title="Output" %}
```
Matt and Megan love to party and party.
Matt and Megan love to party and travel.
```
{% endtab %}
{% endtabs %}

### split filter

The `split` filter takes a substring and uses it split an string into an array.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign quote = "love,is,all,you,need!" | split: ',' %}

{% for word in quote %}
  {{ quorte }}
{% endfor %}
```
{% endtab %}

{% tab title="Output" %}
```
love
is
all
you
need!
```
{% endtab %}
{% endtabs %}

### strip filter

{% tabs %}
{% tab title="Code" %}
```javascript
{{ '   why do we have so many spaces?      ' | strip }}

```
{% endtab %}

{% tab title="Output" %}
```
why do we have so many spaces? 
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Remember to check out the full collection of string filters [here](https://shopify.dev/docs/themes/liquid/reference/filters/string-filters).

`append, camelcase, capitalize, downcase, upcase, escape, handle/handleize, md5, sha1, sha256, hmac_sha1, hmac_sha256, newline_to_br, pluralize, prepend, remove, remove_all, replace, replace_first, slice, split, strip, lstip, rstrip, strip_html, strip_newlines, truncate, truncatewords, url_encode, url_escape, url_param_escape, reverse.`
{% endhint %}

## Math filters

### minus, plus, times filters

{% tabs %}
{% tab title="Code" %}
```javascript
aaaa
```
{% endtab %}

{% tab title="Output" %}
```

```
{% endtab %}
{% endtabs %}



{% hint style="info" %}
Remember to check out the full collection of math filters filters [here](https://shopify.dev/docs/themes/liquid/reference/filters/math-filters).

`abs, at_most, at_least, ceil, divided_by, floor, minus, plus, round, times, modulo.`
{% endhint %}

## Date filter



## Array filters

