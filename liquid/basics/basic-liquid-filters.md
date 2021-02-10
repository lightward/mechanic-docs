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

### replace filter

### slice filter

### split filter

### strip filter

{% hint style="info" %}
Remember to check out the full collection of string filters [here](https://shopify.dev/docs/themes/liquid/reference/filters/string-filters).
{% endhint %}

## Math filters

## Array filters

## Date filter



