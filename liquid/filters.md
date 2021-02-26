# Filters

This page contains definitions for all Mechanic-supported filters. Filters that are unique to Mechanic \(and not available in Shopify or on other platforms\) are noted with an asterisk.

{% hint style="info" %}
This page is a developer reference for Liquid filters supported by Mechanic. For an introduction to the concept of filters, see [Filters](basics/filters.md).
{% endhint %}

## Data filters

### base64, decode\_base64 \*

Allows for converting between strings and their base64-encoded representations.

```javascript
{% assign json_safe_image = image | base64 }}
{% assign original_image = json_safe_image | decode_base64 }}
```

### browser \*

This filter converts a browser user agent string into an object that represents the browser itself. \(This filter uses data from [ Browserscope](https://github.com/ua-parser/uap-core) to match user agents.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign browser = "Mozilla/5.0 (iPhone; CPU iPhone OS 12_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) GSA/79.0.259819395 Mobile/16G77 Safari/604.1" | browser %}

{{ browser }}

name: {{ browser.name }}

version: {{ browser.version }}
major version: {{ browser.version.major }}
minor version: {{ browser.version.minor }}

os: {{ browser.os }}
os name: {{ browser.os.name }}
os version: {{ browser.os.version }}
os major version: {{ browser.os.version.major }}
os minor version: {{ browser.os.version.minor }}

device: {{ browser.device }}
device name: {{ browser.device.name }}
device brand: {{ browser.device.brand }}
device model: {{ browser.device.model }}
```
{% endtab %}

{% tab title="Output" %}
```
Google 79.0.259819395

name: Google

version: 79.0.259819395<br>major version: 79<br>minor version: 0

os: iOS 12.4<br>os name: iOS<br>os version: 12.4<br>os major version: 12<br>os minor version: 4

device: iPhone<br>device name: iPhone<br>device brand: Apple<br>device model: iPhone
```
{% endtab %}
{% endtabs %}

### csv \*

Returns a CSV string representing the input.

```javascript
{% assign order_as_csv = order | csv }}
```

### date, parse\_date \*

Mechanic's date filter is based on [Shopify's date filter](https://shopify.dev/docs/themes/liquid/reference/filters/additional-filters#date). Mechanic's implementation has all the functionality of Shopify's. It accepts a date format, using [the same format as Ruby's strftime](http://www.ruby-doc.org/core/Time.html#method-i-strftime). \(Sites like [strfti.me](http://www.strfti.me/) offer convenient references for this format.\) Under the hood, this filter uses [ActiveSupport::TimeZone\#strptime](https://api.rubyonrails.org/classes/ActiveSupport/TimeZone.html#method-i-strptime), and inherits its behavior with regard to missing upper components.

#### Choosing a timezone

Mechanic's date filter supports a `tz` option, which accepts [a timezone name from the TZ database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). If given, the resulting time string will be in the specified timezone. If this option is not provided, the store's local timezone will be used instead.

```javascript
{{ "now" | date: "%Y-%m-%d %H:%M %z" }}
=> "2019-01-01 09:00 +0900"

{{ "now" | date: "%Y-%m-%d %H:%M %z", tz: "UTC" }}
=> "2019-01-01 00:00 +0000"
```

#### Using the current time

This filter also accepts the special value `"now"`, and values offset from now, as in `"now + 5 days"` or `"now - 5 weeks"`. In this way, the filter supports simple date math. Note that durations are calculated using variable duration lengths, given the naturally varying length of specific days, weeks, months, and years, given DST and other calendar variances, all informed by the store's timezone. This math is backed by [ActiveSupport::TimeWithZone\#+](https://api.rubyonrails.org/classes/ActiveSupport/TimeWithZone.html#method-i-2B). An example, quoting from that documentation: "a time + 24.hours will advance exactly 24 hours, while a time + 1.day will advance 23-25 hours, depending on the day".

```javascript
{{ "now + 6 weeks" | date: "%Y-%m-%d %H:%M %z" }}
{{ "now + 3 months" | date: "%Y-%m-%d %H:%M %z" }}
{{ "now - 3 months" | date: "%Y-%m-%d %H:%M %z" }}

=> "2021-02-15 14:06 -0500"
=> "2021-04-04 14:06 -0400"
=> "2020-10-04 14:06 -0400"
```

#### Parsing dates

Use parse\_date to parse a date string, when its exact format is known. This filter is useful for strings that contain an ambiguous date value, like `"01/01/01"`.

This filter returns an ISO8601 string, representing the parsed date value in the store's local timezone. If the supplied date string cannot be parsed successfully, the filter will return nil.

```javascript
{{ "01-01-20" | parse_date: "%m-%d-%y" }}
=> "2020-01-01T00:00:00+11:00"

{{ "01-01-20" | parse_date: "%m-%d-%y" | date: "%Y-%m-%d" }}
=> "2020-01-01"

{{ "ab-cd-ef" | parse_date: "%m-%d-%y" }}
=> nil
```

### default

Returns either the input value, or – if the input is either `nil`, `false`, or an empty string `""` – it returns its argument.

```javascript
Howdy {{ order.shipping_address.first_name | default: "partner" }}
```

### gzip, gunzip \*

These filters allow you to compress and decompress strings, using gzip compression.

In general, all strings passing through Mechanic must be UTF-8, and must ultimately be valid when represented as JSON. However, because gzip'd content may _not_ be UTF-8, and because it may be important to preserve the original encoding, the gunzip filter supports a `force_utf8: false` option. Use this when you're certain the original encoding must be preserved, _if_ you ultimately intend to pass along the string in a JSON-friendly representation. \(For example, you might gunzip a value, and then use the base64 filter to represent it safely within JSON.\)

```javascript
{{ "testing" | gzip | gunzip }}
=> "testing"

{{ "hello world" | gzip | base64 }}
=> "H4sIABwbfl8AA8tIzcnJVyjPL8pJAQCFEUoNCwAAAA=="

{{ "H4sIANAafl8AA8tIzcnJVyjPL8pJAQCFEUoNCwAAAA==" | decode_base64: force_utf8: false | gunzip }}
=> "hello world"

{% assign base64_non_utf8_string = "H4sIACP1fV8AAyvPSCxRSMlPLVbILFHITU3MUyjJV0hKVXjUMKc4J7/8UcNcewAYP+lTIwAAAA==" %}
{{ base64_non_utf8_string | decode_base64: force_utf8: false | gunzip: force_utf8: false }}
=> (a string that is not UTF-8, and cannot be exported to JSON as-is)

{% assign base64_non_utf8_string = "H4sIACP1fV8AAyvPSCxRSMlPLVbILFHITU3MUyjJV0hKVXjUMKc4J7/8UcNcewAYP+lTIwAAAA==" %}
{{ base64_non_utf8_string | decode_base64: force_utf8: false | gunzip }}
=> "what does it mean to be “slow”?"
```

### graphql\_arguments \*

\[todo\]

### img\_url \*

\[todo\]

### json, parse\_json, parse\_jsonl \*

Allows converting objects to their JSON representations, and parsing that JSON into hashes.

```javascript
{% assign order_as_json = order | json }}
{% assign plain_order = order_as_json | parse_json %}
```

The parse\_jsonl filter can be used to parse a series of JSON strings, each on their own line, into an array of hashes. Useful when preparing [stub data](../core-concepts/tasks/previews/stub-data.md) for [bulk operations](../graphql/bulk-operations.md).

```javascript
{% capture jsonl_string %}
  {"id":"gid://shopify/Customer/12345","email":"foo@bar.baz"}
  {"id":"gid://shopify/Customer/67890","email":"bar@baz.qux"}
{% endcapture %}

{% assign json_objects = jsonl_string | parse_jsonl %}

{{ json_objects | map: "email" | join: ", " }}
```

### parse\_xml \*

Use this filter to parse an XML string. \(Under the hood, this filter calls [Hash::from\_xml](https://api.rubyonrails.org/classes/Hash.html#method-c-from_xml).\) Useful for processing output from third-party APIs, either by [responding to](https://docs.usemechanic.com/article/431-responding-to-action-results) "http" actions, or by parsing content from [inbound webhooks](https://docs.usemechanic.com/article/439-creating-events-with-webhooks).

```javascript
{% capture xml_string %}
<foo>
  <bar>baz</bar>
  <bar>
    <qux>quux</qux>
  </bar>
</foo>
{% endcapture %}

{% assign xml = xml_string | parse_xml %}

{{ xml | json }}
```

```text
{"foo":{"bar":["baz",{"qux":"quux"}]}}
```

### shopify \*

\[todo\]

## String filters

Mechanic supports 

### append

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

### capitalize

{% tabs %}
{% tab title="Code" %}
```javascript
{{ 'mechanic' | capitalize }}
```
{% endtab %}

{% tab title="Output" %}
```
Mechanic
```
{% endtab %}
{% endtabs %}

### downcase, upcase

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

### e164 \*

This filter accepts a phone number – country code is required! – and outputs it in [standard E.164 format](https://en.wikipedia.org/wiki/E.164). If the number does not appear valid, the filter returns `nil`.

```javascript
{{ "1 (312) 456-7890" | e164 }}
=> "13124567890"

{{ "+43 670 1234567890" | e164 }}
=> "436701234567890"

{{ "000" | e164 | json }}
=> "null"
```

### escape, escape\_once

### match \*

Use this filter to match a string with a Ruby-compatible regular expression pattern \(see [Regexp](https://ruby-doc.org/core/Regexp.html)\).

This filter returns the entire matched string \(i.e. [MatchData\#to\_s](https://ruby-doc.org/core/MatchData.html#method-i-to_s)\). Use the "captures" or "named\_captures" lookups to receive an array or hash of captures, respectively \(i.e. [MatchData\#captures](https://ruby-doc.org/core/MatchData.html#method-i-captures), [MatchData\#named\_captures](https://ruby-doc.org/core/MatchData.html#method-i-named_captures)\).

```javascript
{{ "It's a lovely day!" | match: "(?<=a ).*(?= day)" }}
=> "lovely"

{% assign match = "It's a lovely day!" | match: "a (bucolic|lovely) day" %}
{{ match.captures }}
=> ["lovely"]

{% assign match = "It's a lovely day!" | match: "a (?<adjective>bucolic|lovely) day" %}
{{ match.named_captures }}
=> {"adjective"=>"lovely"}

{% assign match = "It's a lovely day!" | match: "a (?i:LOVELY) day" %}
{{ match }}
=> "a lovely day"
```

### md5

### newline\_to\_br

### prepend

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

### remove, remove\_first

Removes a substring from a string.

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

### replace, replace\_first

Replaces a substring with something else.

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

### sha256, hmac\_sha1, hmac\_sha256

Generates SHA checksums of strings.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign signature = "mechanic" | sha256 %}
sha256: {{ signature }}

{% assign signature = "mechanic" | hmac_sha1: "sincerely" %}
hmac_sha1: {{ signature }}

{% assign signature = "mechanic" | hmac_sha256: "sincerely" %}
hmac_sha256: {{ signature }}
```
{% endtab %}

{% tab title="Output" %}
```
sha256: 6c8a739536961bcf34dccc343908406d48139344da4754d4cfe43dcf8d662607

hmac_sha1: 0425a4dbbe0588be87fb51b5706c2244401bc73a

hmac_sha256: 4b8e2bcf66f95b21f74f491eacc1459b0c9ea6723355174af52ded391f9326ea
```
{% endtab %}
{% endtabs %}

### size

### slice

### split

Takes a substring, and uses it to split a string into an array.

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

### strip, lstrip, rstrip

The strip filter removes whitespace from both sides of a string. The lstrip filter removes whitespace from the left side; the rstrip filter removes whitespace from the right side.

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

### strip\_html

### strip\_newlines

### truncate, truncatewords

### unindent \*

Use this filter on strings to remove indentation from strings.

{% tabs %}
{% tab title="Code" %}
```javascript
{% capture message %}
  Hello, friend!
  It's a mighty fine day!
{% endcapture %}

{{ message }}
{{ message | unindent }}
```
{% endtab %}

{% tab title="Output" %}
```
  Hello, friend!
  It's a mighty fine day!


Hello, friend!
It's a mighty fine day!
```
{% endtab %}
{% endtabs %}

### url\_decode, url\_encode

## Math filters

Mechanic supports [the same set of math filters as Shopify](https://shopify.dev/docs/themes/liquid/reference/filters/math-filters#modulo).

* abs – Returns the absolute value of the input. No arguments.
* at\_most – Accepts one argument; returns either the input or the argument, whichever is lesser.
* at\_least – Accepts one argument; returns either the input or the argument, whichever is greater.
* ceil – Rounds a float to the next highest integer.
* divided\_by – Accepts one argument; divides the input by the argument. If both the input and arguments are integers, returns an integer; otherwise, returns a float.
* floor – Rounds a float down to the next highest integer.
* minus – Accepts one argument; returns the input minus the argument.
* plus – Accepts one argument; returns the input plus the argument.
* round – Rounds a float to the nearest integer.
* times – Accepts one argument; divides the input by the argument. If both the input and arguments are integers, returns an integer; otherwise, returns a float.
* modulo – Accepts one argument; divides the input by the argument and returns the remainder.

## Array filters

### concat

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

### first, last

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
{% endif %}
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

### in\_groups \*

This filter is an implementation of [Array\#in\_groups](https://api.rubyonrails.org/classes/Array.html#method-i-in_groups). It accepts an array, and an integer count, and – optionally – a "fill\_with" option.

{% tabs %}
{% tab title="Code" %}
```javascript
{{ "1,2,3" | split: "," | in_groups: 2 | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
[["1","2"],["3",null]]
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Code" %}
```javascript
{{ "1,2,3" | split: "," | in_groups: 2, fill_with: false | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
[["1","2"],["3"]]
```
{% endtab %}
{% endtabs %}

### in\_groups\_of \*

This filter is an implementation of [Array\#in\_groups\_of](https://api.rubyonrails.org/classes/Array.html#method-i-in_groups_of). It accepts an array, and an integer count, and – optionally – a "fill\_with" option.

This filter is particularly useful when performing work in batches, by making it easy to split an array of potentially large size into smaller pieces of controlled size.

{% tabs %}
{% tab title="Code" %}
```javascript
{{ "1,2,3,4,5" | split: "," | in_groups_of: 2 | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
[["1","2"],["3","4"],["5",null]]
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Code" %}
```javascript
{{ "1,2,3,4,5" | split: "," | in_groups_of: 2, fill_with: false | json }}
```
{% endtab %}

{% tab title="Output" %}
```
[["1","2"],["3","4"],["5"]]
```
{% endtab %}
{% endtabs %}

### join

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

### map

Given an array of objects that contain attributes \(e.g. `name`\) and values \(e.g. `matt`\), we can use `map` to get a new array of values for a specific attribute of the objects in the array. In the below example, we have an array of products, and each product has a title. Using `map` we can create a new array of only product titles.

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

### reverse <a id="sample"></a>

This filter returns a new array, containing the contents of the original in reverse order.

### sample \* <a id="sample"></a>

This filter can be used on any array. Used without any arguments, it returns a single random element from the array. Provide an integer argument to return another array of that size, containing a random subset of the input array.

```javascript
{{ "1,2,3" | split: "," | sample }}
=> "2"

{{ "1,2,3" | split: "," | sample: 2 | join: "," }}
=> "3,1"
```

### size

This filter returns the number of elements in the given array.

### sort

Sorts an array by a given attribute.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign big_spenders = customers | sort: "total_spent" %}

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

### sum \*

Use this on an array of numbers to quickly generate their sum.

```javascript
{{ "[1, 2, 3]" | parse_json | sum }}
=> 6
```

### uniq

Removes any duplicates in an array, resulting in a new array of distinct values.

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

### where

Takes an array of objects, and create a new array with only those that have a given property value.

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

## Hash filters

### compact \*

The compact filter is [a long-standing part of Liquid itself](http://shopify.github.io/liquid/filters/compact/). Mechanic's implementation includes explicit support for hashes. The result: Mechanic's implementation removes nil elements from arrays, and removes hash keys having nil values.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign foo = hash %}
{% assign foo["bar"] = "baz" %}
{% assign foo["qux"] = nil %}
{{ foo | json }}
{{ foo | compact | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
{"bar":"baz","qux":null}
{"bar":"baz"}
```
{% endtab %}
{% endtabs %}

### keys \*

Returns an array of keys found in the supplied hash.

### values \*

Returns an array of values found in the supplied hash.

