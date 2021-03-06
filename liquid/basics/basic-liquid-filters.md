# Filters

We can change the output of a Liquid object by applying a filter. Filters are separated by a `|`. The full range of filters are covered in the Shopify docs [here](https://shopify.github.io/liquid/basics/introduction/) and [here](https://shopify.dev/docs/themes/liquid/reference/filters).

### Example usage of the upcase filter

```javascript
{{ order.billing_address.zip | upcase }}
```

## General filters

### Default filter

This filter set a default value when a variable is either `nil`, `false`, or an empty string `""`.

```javascript
Howdy {{ order.shipping_address.first_name | default: "partner" }}
```

### JSON filter

WIP

### Date filter

Mechanic has a [date filter](../mechanic-liquid/filters/date.md) based on Shopify's date filter \(see  [Shopify / Liquid reference / Additional filters](https://shopify.dev/docs/liquid/reference/filters/additional-filters#date)\).  In addition to taking a timestamp and outputting it in the desired format. It adds a single additional argument: providing a tz value will result in the rendered date being returned in the provided timezone. If this argument is not provided, the store's local timezone will be used instead.

Check out the Mechanic [date filter](../mechanic-liquid/filters/date.md).

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
  {{ quote }}
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

### minus, plus, times, divided\_by, modulo filters

{% tabs %}
{% tab title="Code" %}
```javascript
{{ 25 | minus: 15 }}
{{ 25 | plus: 15 }}
{{ 25 | times: 2 }}
{{ 25 | divided_by: 5 }}
{{ 11 | modulo: 5 }}

```
{% endtab %}

{% tab title="Output" %}
```
10
40
50
5
1

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Remember to check out the full collection of math filters [here](https://shopify.dev/docs/themes/liquid/reference/filters/math-filters).

`abs, at_most, at_least, ceil, divided_by, floor, minus, plus, round, times, modulo.`
{% endhint %}

## Array filters

### join filter

Creates a string from the elements of an array using a character passed as an argument.

{% tabs %}
{% tab title="Code" %}
```javascript
{% comment %} 
  product.tags = "VIP", "New", "Canada"
{% endcomment %}

{{ customer.tags | join: ', ' }}
```
{% endtab %}

{% tab title="Output" %}
```
VIP, New, Canada
```
{% endtab %}
{% endtabs %}

### first and last filters

Returns the first or last element of an array. You can use `first` or `last` in dot notation inside of tags.

{% tabs %}
{% tab title="Code" %}
```javascript
{% comment %} 
  product.tags = "VIP", "New", "Canada"
{% endcomment %}

{{ custom.tags | first }}
{{ customer.tags | last }}

{% if customer.tags.first == "VIP" %}
  This customer is a VIP!
{% endif %

{% if customer.tags.last == "Canada" %}
  Eh!
{% endif %

```
{% endtab %}

{% tab title="Output" %}
```
VIP
Canada
This customer is a VIP!
Eh!

```
{% endtab %}
{% endtabs %}

### concat filter

Concatenates two arrays into a single array.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign lunch = "sandwich, apple, soup" | split: ", " %}
{% assign dinner = "pasta, pizza, salad" | split: ", " %}

{% assign meals = lunch | concat: dinner %}

{{ meals | join: ", " }}

{% assign breakfast = "eggs, oatmeal, toast" | split: ", " %}

{% assign meals = breakfast | concat: lunch | concat: dinner %}

{{ meals | join: ", " }}
```
{% endtab %}

{% tab title="Output" %}
```
sandwich, apple, soup, pasta, pizza, salad
eggs, oatmeal, toast, sandwich, apple, soup, pasta, pizza, salad
```
{% endtab %}
{% endtabs %}

### map filter

Given an array of objects that contain attributes \(e.g. Name\) and values \(e.g. Matt\), we can use `map` to get a new array of values for a specific attribute of the objects in the array. In the below example, we have an array of products, and each product has a title. Using `map` we can create a new array of only product titles.

{% tabs %}
{% tab title="Code" %}
```javascript
{% comment %} 
  product.titles = "Apple", "Orange", "Pepper", "Cheese"
{% endcomment %}

{% assign product_titles = products | map: 'title' %}

{{ product_titles }}
```
{% endtab %}

{% tab title="Output" %}
```
AppleOrangePepperCheese
```
{% endtab %}
{% endtabs %}

### where filter

Take an array of objects and create a new array with only those that have a  given property value  


{% tabs %}
{% tab title="Code" %}
```javascript
All products:
{% for product in collection.products %}
- {{ product.title }}
{% endfor %}

{% assign sports_products = collection.products | where: "type", "sports" %}

sports products:
{% for product in sports_products %}
- {{ product.title }}
{% endfor %}
```
{% endtab %}

{% tab title="Output" %}
```
All products:
- Ball
- Car
- Cheese
- Bat

Sports products:
- Ball
- Batt
```
{% endtab %}
{% endtabs %}

### uniq filter

Removes any duplicate in an array.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign cars = "Honda Ford Toyota Jeep VW Honda VW" %}
{{ cars | split: ' ' | uniq | join: ' ' }}
```
{% endtab %}

{% tab title="Output" %}
```
Honda Ford Toyota Jeep VW
```
{% endtab %}
{% endtabs %}

### sort filter

Sorts an array by a given attribute.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign big_spenders = customers | sort: 'total_spent' %}
{% for customer in big_spenders %}
  <h4>{{ customer.email }}</h4>
{% endfor %}
```
{% endtab %}

{% tab title="Output" %}
```
<h4>jim@joe.com</h4>
<h4>jack@joe.com</h4>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Remember to check out the full collection of array filters [here](https://shopify.dev/docs/themes/liquid/reference/filters/array-filters).

`join, first, last, concat, index, map, reverse, size, sort,where, uniq.`
{% endhint %}

## Shopify Documentation

Eventually you'll want to dig deeper into Liquid.  Make sure to review the [Shopify Liquid language ](https://shopify.github.io/liquid/)and the S[hopify Liquid theme documentation.](https://shopify.dev/docs/themes/liquid/reference)

