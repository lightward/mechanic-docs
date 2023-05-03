# Filters

This page contains definitions for all Mechanic-supported Liquid filters.

Filters that are unique to Mechanic (and not available in Shopify or on other platforms) are noted with an asterisk.

{% hint style="info" %}
This page is a developer reference for Liquid filters supported by Mechanic. For an introduction to the concept of Liquid filters, see [Filters](basics/filters.md).
{% endhint %}

{% hint style="warning" %}
Liquid filters should not be confused with [event filters](../events/filters.md), which are used to conditionally ignore incoming events.
{% endhint %}

## Data filters

### base64, decode\_base64 \*

Allows for converting between strings and their base64-encoded representations.

```javascript
{% raw %}
{% assign json_safe_image = image | base64 %}
{% assign original_image = json_safe_image | decode_base64 %}
{% endraw %}
```

### browser \*

This filter converts a browser user agent string into an object that represents the browser itself. (This filter uses data from [ Browserscope](https://github.com/ua-parser/uap-core) to match user agents.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% assign browser = "Mozilla/5.0 (iPhone; CPU iPhone OS 12_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) GSA/79.0.259819395 Mobile/16G77 Safari/604.1" | browser %}
{% endraw %}

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

### csv, parse\_csv \*

Supports converting a two-dimensional array to a CSV string, and back again.

The parse\_csv filter accepts a "headers" option; when set to `true`, this filter will interpret the first line of input as containing headers for the CSV table, and will return an array of hashes whose keys map to items in that header row.

{% tabs %}
{% tab title="csv" %}
```javascript
{% raw %}
{% capture two_dimensional_array_json %}
  [
    [
      "Order Name",
      "Order ID",
      "Order Date"
    ],
    [
      "#1234",
      1234567890,
      "2021/03/23"
    ],
    [
      "#1235",
      1234567891,
      "2021/03/24"
    ]
  ]
{% endcapture %}

{% assign two_dimensional_array = two_dimensional_array_json | parse_json %}

{% assign csv_string = two_dimensional_array | csv %}
{% endraw %}
```
{% endtab %}

{% tab title="parse_csv" %}
```javascript
{% raw %}
{% comment %}
  Note the dashes used in the capture/endcapture tags!
  They make sure that we don't end up with blank lines
  at the beginning and end of our CSV string.
{% endcomment %}
{% capture csv_string -%}
Order Name,Order ID,Order Date
#1234,1234567890,2021/03/23
#1235,1234567891,2021/03/24
{%- endcapture %}

{% assign csv_rows = csv_string | parse_csv %}

{% assign orders = array %}
{% for row in csv_rows %}
  {% comment %}
    Skip the header row
  {% endcomment %}
  {% if forloop.first %}
    {% continue %}
  {% endif %}

  {% assign order = hash %}
  {% assign order["name"] = row[0] %}
  {% comment %}
    We're using `times: 1` to convert our string ID to an integer
  {% endcomment %}
  {% assign order["id"] = row[1] | times: 1 %}
  {% assign order["date"] = row[2] %}

  {% assign orders[orders.size] = order %}
{% endfor %}
{% endraw %}

{{ orders | json }}
```
{% endtab %}

{% tab title="parse_csv with headers" %}
```javascript
{% raw %}
{% comment %}
  Note the dashes used in the capture/endcapture tags!
  They make sure that we don't end up with blank lines
  at the beginning and end of our CSV string.
{% endcomment %}
{% capture csv_string -%}
Order Name,Order ID,Order Date
#1234,1234567890,2021/03/23
#1235,1234567891,2021/03/24
{%- endcapture %}

{% comment %}
  Note: the order ID is a string, in this resulting set of
  hashes, not an integer!
{% endcomment %}
{% assign orders = csv_string | parse_csv: headers: true %}

{{ orders | json }}
{% comment %}
  The result:

  [
    {
      "Order Name": "#1234",
      "Order ID": "1234567890",
      "Order Date": "2021/03/23"
    },
    {
      "Order Name": "#1235",
      "Order ID": "1234567891",
      "Order Date": "2021/03/24"
    }
  ]
{% endcomment %}
{% endraw %}
```
{% endtab %}
{% endtabs %}

### date, parse\_date \*

Mechanic's date filter is based on [Shopify's date filter](https://shopify.dev/docs/themes/liquid/reference/filters/additional-filters#date). Mechanic's implementation has all the functionality of Shopify's. It accepts a date format, using [the same format as Ruby's strftime](http://www.ruby-doc.org/core/Time.html#method-i-strftime). (Sites like [strfti.me](http://www.strfti.me/) offer convenient references for this format.) Under the hood, this filter uses [ActiveSupport::TimeZone#strptime](https://api.rubyonrails.org/classes/ActiveSupport/TimeZone.html#method-i-strptime), and inherits its behavior with regard to missing upper components.

#### Choosing a timezone

Mechanic's date filter supports a `tz` option, which accepts [a timezone name from the TZ database](https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones). If given, the resulting time string will be in the specified timezone. If this option is not provided, the store's local timezone will be used instead.

```javascript
{{ "now" | date: "%Y-%m-%d %H:%M %z" }}
=> "2019-01-01 09:00 +0900"

{{ "now" | date: "%Y-%m-%d %H:%M %z", tz: "UTC" }}
=> "2019-01-01 00:00 +0000"
```

#### Using the current time

This filter also accepts the special value `"now"`, and values offset from now, as in `"now + 5 days"` or `"now - 5 weeks"`. In this way, the filter supports simple date math. Note that durations are calculated using variable duration lengths, given the naturally varying length of specific days, weeks, months, and years, given DST and other calendar variances, all informed by the store's timezone. This math is backed by [ActiveSupport::TimeWithZone#+](https://api.rubyonrails.org/classes/ActiveSupport/TimeWithZone.html#method-i-2B). An example, quoting from that documentation: "a time + 24.hours will advance exactly 24 hours, while a time + 1.day will advance 23-25 hours, depending on the day".

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

In general, all strings passing through Mechanic must be UTF-8, and must ultimately be valid when represented as JSON. However, because gzip'd content may _not_ be UTF-8, and because it may be important to preserve the original encoding, the gunzip filter supports a `force_utf8: false` option. Use this when you're certain the original encoding must be preserved, _if_ you ultimately intend to pass along the string in a JSON-friendly representation. (For example, you might gunzip a value, and then use the base64 filter to represent it safely within JSON.)

```javascript
{{ "testing" | gzip | gunzip }}
=> "testing"

{{ "hello world" | gzip | base64 }}
=> "H4sIABwbfl8AA8tIzcnJVyjPL8pJAQCFEUoNCwAAAA=="

{{ "H4sIANAafl8AA8tIzcnJVyjPL8pJAQCFEUoNCwAAAA==" | decode_base64: force_utf8: false | gunzip }}
=> "hello world"

{% raw %}
{% assign base64_non_utf8_string = "H4sIACP1fV8AAyvPSCxRSMlPLVbILFHITU3MUyjJV0hKVXjUMKc4J7/8UcNcewAYP+lTIwAAAA==" %}
{{ base64_non_utf8_string | decode_base64: force_utf8: false | gunzip: force_utf8: false }}
=> (a string that is not UTF-8, and cannot be exported to JSON as-is)

{% assign base64_non_utf8_string = "H4sIACP1fV8AAyvPSCxRSMlPLVbILFHITU3MUyjJV0hKVXjUMKc4J7/8UcNcewAYP+lTIwAAAA==" %}
{% endraw %}
{{ base64_non_utf8_string | decode_base64: force_utf8: false | gunzip }}
=> "what does it mean to be “slow”?"
```

### graphql\_arguments \*

Useful for preparing key-value pairs of GraphQL query or mutation arguments.

{% hint style="info" %}
Across the documentation and [task library](../../resources/task-library/), you'll frequently see `json` used for serializing argument values. Users have reported some rare cases where this filter is insufficient, and where `graphql_arguments` does the trick instead.
{% endhint %}

{% hint style="info" %}
`graphql_arguments` is typically used for rendering GraphQL values into the final GraphQL query string itself. Instead, consider extracting your values as GraphQL variables. This approach can result in more reusable query code.

To try this using a Shopify action, use the [GraphQL with variables](../../core/actions/integrations/shopify.md#graphql-with-variables) syntax.

To try this using the shopify filter, use the [variables](filters.md#graphql-variables) argument.


{% endhint %}

```liquid
{% raw %}
{% assign inputs = hash %}
{% assign inputs["a_string"] = "yep this is a string" %}
{% assign inputs["a_more_complex_type"] = hash %}
{% assign inputs["a_more_complex_type"]["id"] = "gid://something/Or?other" %}
{% assign inputs["an_array"] = array %}
{% assign inputs["an_array"][0] = 1 %}
{% assign inputs["an_array"][0] = 2 %}

{% action "shopify" %}
  mutation {
    anExample({{ inputs | graphql_arguments }}) {
      result
    }
  }
{% endaction %}
{% endraw %}
```

This results in a [GraphQL Shopify action](../../core/actions/integrations/shopify.md#graphql) containing the following GraphQL:

```graphql
mutation {
  anExample(
    a_string: "yep this is a string"
    a_more_complex_type: { id: "gid://something/Or?other" }
    an_array: [2]
  ) {
    result
  }
}
```

For a more complex example, see [Set product or variant metafields values in bulk](https://github.com/lightward/mechanic-tasks/blob/ce13e354c263f47bb77d80618fc90af5064d10bc/docs/set-product-or-variant-metafields-in-bulk/script.liquid#L127) from the task library.

### img\_url \*

\[todo]

### json, parse\_json \*

Allows converting objects to their JSON representations, and parsing that JSON into hashes.

```liquid
{% assign order_as_json = order | json }}
{% raw %}
{% assign plain_order = order_as_json | parse_json %}
{% endraw %}
```

The parse\_json filter raises an error when invalid JSON. To ignore parse errors, and to return null when an error is encountered, add `silent: true` to the filter's options:

```liquid
{% raw %}
{% assign should_be_nil = "{{" | parse_json: silent: true %}
{% endraw %}
```

### jsonl, parse\_jsonl \*

Allows for rendering an iterable object (i.e. an array) as a series of JSON lines, separated by simple newlines.

```liquid
{{ shop.customers | jsonl }}
```

The parse\_jsonl filter can be used to parse a series of JSON strings, each on their own line, into an array of hashes. Useful when preparing [stub data](../../core/tasks/previews/stub-data.md) for [bulk operations](../graphql/bulk-operations.md).

```liquid
{% raw %}
{% capture jsonl_string %}
  {"id":"gid://shopify/Customer/12345","email":"foo@bar.baz"}
  {"id":"gid://shopify/Customer/67890","email":"bar@baz.qux"}
{% endcapture %}

{% assign json_objects = jsonl_string | parse_jsonl %}
{% endraw %}

{{ json_objects | map: "email" | join: ", " }}
```

The parse\_jsonl filter raises error an error when invalid JSONL is received.

### parse\_xml \*

Use this filter to parse an XML string. (Under the hood, this filter calls [Hash::from\_xml](https://api.rubyonrails.org/classes/Hash.html#method-c-from\_xml).) Useful for processing output from third-party APIs, either by [responding to](https://docs.usemechanic.com/article/431-responding-to-action-results) "http" actions, or by parsing content from [inbound webhooks](https://docs.usemechanic.com/article/439-creating-events-with-webhooks).

```javascript
{% raw %}
{% capture xml_string %}
<foo>
  <bar>baz</bar>
  <bar>
    <qux>quux</qux>
  </bar>
</foo>
{% endcapture %}

{% assign xml = xml_string | parse_xml %}
{% endraw %}

{{ xml | json }}
```

```
{"foo":{"bar":["baz",{"qux":"quux"}]}}
```

### shopify \*

This filter accepts a GraphQL query string, sends it to Shopify, and returns the full response – including `"data"` and `"errors"`.

{% hint style="info" %}
Use [Shopify's GraphiQL query builder](https://shopify.dev/apps/tools/graphiql-admin-api) to quickly and precisely assemble your queries.
{% endhint %}

{% tabs %}
{% tab title="Usage" %}
```liquid
{% raw %}
{% capture query %}
  query {
    shop {
      primaryDomain {
        host
      }
    }
  }
{% endcapture %}

{% assign result = query | shopify %}

{% log result %}
{% endraw %}
```
{% endtab %}

{% tab title="Result" %}
```json
{
  "log": {
    "data": {
      "shop": {
        "primaryDomain": {
          "host": "example.com",
        },
      }
    },
    "extensions": {
      "cost": {
        "requestedQueryCost": 2,
        "actualQueryCost": 2,
        "throttleStatus": {
          "maximumAvailable": 1000.0,
          "currentlyAvailable": 998,
          "restoreRate": 50.0
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### GraphQL variables

This filter also supports GraphQL variables, via an optional named argument called `variables`.

```liquid
{% raw %}
{% capture query %}
  query ProductQuery($id: ID!) {
    product(id: $id) {
      title
    }
  }
{% endcapture %}

{% assign variables = hash %}
{% assign variables["id"] = product_id %}

{% assign result = query | shopify: variables: variables %}

{% log result %}


{% comment %}
  Alternate style, avoiding the `variables: variables` construction:
{% endcomment %}

{% assign query_options = hash %}
{% assign query_options["variables"] = hash %}
{% assign query_options["variables"]["id"] = product_id %}

{% assign result = query | shopify: query_options %}
{% endraw %}
```

{% hint style="info" %}
Variables can be a useful part of making queries reusable within a task, or for working around [Shopify's 50,000 character limit for GraphQL queries](../../faq/query-param-length-is-too-long.md).
{% endhint %}

## String filters

Mechanic supports all of the string filters available in Liquid - [https://shopify.dev/api/liquid/filters/string-filters](https://shopify.dev/api/liquid/filters/string-filters).

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

Use this filter to match a string with a Ruby-compatible regular expression pattern (see [Regexp](https://ruby-doc.org/core/Regexp.html)).

This filter returns the entire matched string (i.e. [MatchData#to\_s](https://ruby-doc.org/core/MatchData.html#method-i-to\_s)). Use the "captures" or "named\_captures" lookups to receive an array or hash of captures, respectively (i.e. [MatchData#captures](https://ruby-doc.org/core/MatchData.html#method-i-captures), [MatchData#named\_captures](https://ruby-doc.org/core/MatchData.html#method-i-named\_captures)).

{% hint style="info" %}
This filter only returns the first match found. To find all available matches in a string, use [scan](filters.md#scan).
{% endhint %}

```javascript
{{ "It's a lovely day!" | match: "(?<=a ).*(?= day)" }}
=> "lovely"

{% raw %}
{% assign match = "It's a lovely day!" | match: "a (bucolic|lovely) day" %}
{{ match.captures }}
=> ["lovely"]

{% assign match = "It's a lovely day!" | match: "a (?<adjective>bucolic|lovely) day" %}
{{ match.named_captures }}
=> {"adjective" => "lovely"}

{% assign match = "It's a lovely day!" | match: "a (?i:LOVELY) day" %}
{% endraw %}
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

### scan \*

Use this filter to find all available matches in a string, using a Ruby-compatible regular expression pattern (see [Regexp](https://ruby-doc.org/core/Regexp.html)).

This filter returns an array of matches, consisting of each matched string (i.e. [MatchData#to\_s](https://ruby-doc.org/core/MatchData.html#method-i-to\_s)). Use the "captures" or "named\_captures" lookups on individual matches to receive an array or hash of captures, respectively (i.e. [MatchData#captures](https://ruby-doc.org/core/MatchData.html#method-i-captures), [MatchData#named\_captures](https://ruby-doc.org/core/MatchData.html#method-i-named\_captures)).

{% hint style="info" %}
This filter returns an array of matches. To only find the first match, use [match](filters.md#match).
{% endhint %}

```javascript
{{ "It's a lovely day!" | scan: "[\w']+" }}
=> ["It's", "a", "lovely", "day"]

{{ "It's a lovely day!" | scan: "(bucolic|lovely|day)" | map: "captures" }}
=> [["lovely"], ["day"]]

{{ "It's a lovely day!" | scan: "(?<punctuation>[[:punct:]])" | map: "named_captures" }}
=> [{"punctuation" => "'"}, {"punctuation" => "!"}]
```

### sha256, hmac\_sha1, hmac\_sha256

Generates hexadecimal SHA digests. For binary output instead, add `binary: true` to the filter's options.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% assign signature = "mechanic" | sha256 %}
sha256: {{ signature }}

{% assign signature = "mechanic" | hmac_sha1: "sincerely" %}
hmac_sha1: {{ signature }}

{% assign signature = "mechanic" | hmac_sha256: "sincerely" %}
hmac_sha256: {{ signature }}

{% comment %}
  Generating an AWS request signature - adapted from
  https://docs.aws.amazon.com/general/latest/gr/sigv4-calculate-signature.html
{% endcomment %}
{% assign kSecret = "wJalrXUtnFEMI/K7MDENG+bPxRfiCYEXAMPLEKEY" %}
{% assign prefixed_kSecret = "AWS4" | append: kSecret %}
{% assign kDate = "20150830" | hmac_sha256: prefixed_kSecret, binary: true %}
{% assign kRegion = "us-east-1" | hmac_sha256: kDate, binary: true %}
{% assign kService = "iam" | hmac_sha256: kRegion, binary: true %}
{% assign kSigning = "aws4_request" | hmac_sha256: kService %}
{% endraw %}
aws: {{ kSigning }}
```
{% endtab %}

{% tab title="Output" %}
```
sha256: 6c8a739536961bcf34dccc343908406d48139344da4754d4cfe43dcf8d662607

hmac_sha1: 0425a4dbbe0588be87fb51b5706c2244401bc73a

hmac_sha256: 4b8e2bcf66f95b21f74f491eacc1459b0c9ea6723355174af52ded391f9326ea

aws: c4afb1cc5771d871763a393e44b703571b55cc28424d1a5e86da6ed3c154a4b9
```
{% endtab %}
{% endtabs %}

### size

Returns the integer length of the input string.

### slice

{% hint style="info" %}
This filter also works for [arrays](filters.md#slice-1)! And there's a different behavior available for [hashes](filters.md#slice-2).
{% endhint %}

This filter accepts an integer offset, and an optional integer length (defaulting to 1). It returns a substring, beginning at the provided index, having the provided length.

Negative offsets begin counting from the end of the string.

```javascript
{{ "12345" | slice: 3 }}
=> "4"

{{ "12345" | slice: 3, 2 }}
=> "45"

{{ "12345" | slice: -3, 2 }}
=> "34"
```

### split

Takes a substring, and uses it to split a string into an array.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% assign quote = "love,is,all,you,need!" | split: ',' %}

{% for word in quote %}
  {{ quote }}
{% endfor %}
{% endraw %}
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
{% raw %}
{% capture message %}
  Hello, friend!
  It's a mighty fine day!
{% endcapture %}
{% endraw %}

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

## Number filters

### currency \*

Formats a number (given as an [integer](basics/types.md#integer-float), [float](basics/types.md#integer-float), or [string](basics/types.md#string)) as currency. Called with no arguments, this filter uses the store's primary currency and default locale.

A three-character ISO currency code may be specified as the first argument; currency support is drawn from the [money](https://github.com/RubyMoney/money/blob/main/config/currency\_iso.json) project. The locale may be overridden as a named option; locale support is drawn from [rails-i18n](https://github.com/svenfuchs/rails-i18n#available-locales).

{% tabs %}
{% tab title="Code" %}
```javascript
{{ "100000.0" | currency }}
{{ 100000.0 | currency: "EUR" }}
{{ 100000 | currency: "EUR", locale: "fr" }}
{{ 100000 | currency: locale: "fr" }}
```
{% endtab %}

{% tab title="Output" %}
```
$100,000.00
€100,000.00
€100 000,00
$100 000,00
```
{% endtab %}
{% endtabs %}

Note that this filter does not automatically append the currency ISO code (e.g. it will not generate output resembling "€100,000.00 EUR"). To add the ISO code manually, use one of these examples:

```javascript
{{ price | currency }} {{ shop.currency }}
{{ price | currency | append: " " | append: shop.currency }}
```

### Math filters

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
{% raw %}
{% assign lunch = "sandwich, apple, soup" | split: ", " %}
{% assign dinner = "pasta, pizza, salad" | split: ", " %}

{% assign meals = lunch | concat: dinner %}

{{ meals | join: ", " }}

{% assign breakfast = "eggs, oatmeal, toast" | split: ", " %}

{% assign meals = breakfast | concat: lunch | concat: dinner %}
{% endraw %}

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
{% raw %}
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
{% endraw %}
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

This filter is an implementation of [Array#in\_groups](https://api.rubyonrails.org/classes/Array.html#method-i-in\_groups). It accepts an array, and an integer count, and – optionally – a "fill\_with" option.

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

This filter is an implementation of [Array#in\_groups\_of](https://api.rubyonrails.org/classes/Array.html#method-i-in\_groups\_of). It accepts an array, and an integer count, and – optionally – a "fill\_with" option.

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

### index\_by \*

This filter accepts the name of an object property or attribute, and returns a hash that whose values are every element in the array, keyed by every element's corresponding property or attribute.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% capture variants_json %}
  [
    {
      "id": 12345,
      "sku": "ONE"
    },
    {
      "id": 67890,
      "sku": "TWO"
    }
  ]
{% endcapture %}

{% assign variants = variants_json | parse_json %}
{% endraw %}

{{ variants | index_by: "sku" | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
{
  "ONE": {
    "id": 12345,
    "sku": "ONE"
  },
  "TWO": {
    "id": 67890,
    "sku": "TWO"
  }
}
```
{% endtab %}
{% endtabs %}

### join

Creates a string from the elements of an array using a character passed as an argument.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% comment %} 
  product.tags = "VIP", "New", "Canada"
{% endcomment %}
{% endraw %}

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

Given an array of objects that contain attributes (e.g. `name`) and values (e.g. `matt`), we can use `map` to get a new array of values for a specific attribute of the objects in the array. In the below example, we have an array of products, and each product has a title. Using `map` we can create a new array of only product titles.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% comment %} 
  product.titles = "Apple", "Orange", "Pepper", "Cheese"
{% endcomment %}

{% assign product_titles = products | map: 'title' %}
{% endraw %}

{{ product_titles }}
```
{% endtab %}

{% tab title="Output" %}
```
AppleOrangePepperCheese
```
{% endtab %}
{% endtabs %}

### push \*

This filter appends any number of arguments onto the provided array, returning a new array, leaving the original unmodified.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% assign count_to_three = "one,two,three" | split: "," %}

{% assign count_to_five = count_to_three | push: "four", "five" %}
{% endraw %}

{{ count_to_five | join: newline }}
```
{% endtab %}

{% tab title="Output" %}
```
one
two
three
four
five
```
{% endtab %}
{% endtabs %}

### reverse

This filter returns a new array, containing the contents of the original in reverse order.

### sample \*

This filter can be used on any array. Used without any arguments, it returns a single random element from the array. Provide an integer argument to return another array of that size, containing a random subset of the input array.

```javascript
{{ "1,2,3" | split: "," | sample }}
=> "2"

{{ "1,2,3" | split: "," | sample: 2 | join: "," }}
=> "3,1"
```

### slice

{% hint style="info" %}
This filter also works for [strings](filters.md#slice)! And there's a different behavior available for [hashes](filters.md#slice-2).
{% endhint %}

This filter accepts an integer offset, and an optional integer length (defaulting to 1). If the length is 1, it returns the single element found at that index of the input array. Otherwise, it returns a slice of the array, beginning at the provided index, having the provided length.

Negative offsets begin counting from the end of the array.

```javascript
{{ "1,2,3,4,5" | split: "," | slice: 3 }}
=> "4"

{{ "1,2,3,4,5" | split: "," | slice: 3, 2 | join: "," }}
=> "4,5"

{{ "1,2,3,4,5" | split: "," | slice: -3, 2 | join: "," }}
=> "3,4"
```

### size

This filter returns the number of elements in the given array.

### sort, sort\_natural, sort\_naturally \*

Sorts an array. All three filters accept a single optional parameter, specifying an attribute to sort by, as in `orders | sort: "number"` or `orders | sort_naturally: "number"`.

* `sort` – performs a standard alphanumeric sort (part of standard Liquid)
* `sort_natural`– sorts by case-insensitive string values (part of standard Liquid)
* `sort_naturally` – uses the human-friendly sort order defined by [naturally](https://github.com/dogweather/naturally) (unique to Mechanic)

{% hint style="info" %}
Make sure to choose the sort algorithm most suitable for the consumers of your data. Machine audiences are typically happier with `sort`; human audiences are typically happier with `sort_naturally`.
{% endhint %}

{% tabs %}
{% tab title="Code" %}
```liquid
{% raw %}
{% assign set = "order #10.b,Order #10.a,Order #2.c,order #2.d" | split: "," %}
{% endraw %}

unsorted:
  {{ set | join: ", " }}
sort:
  {{ set | sort | join: ", " }}
sort_natural:
  {{ set | sort_natural | join: ", " }}
sort_naturally:
  {{ set | sort_naturally | join: ", " }}
```
{% endtab %}

{% tab title="Output" %}
```
unsorted:
  order #10.b, Order #10.a, Order #2.c, order #2.d
sort:
  Order #10.a, Order #2.c, order #10.b, order #2.d
sort_natural:
  Order #10.a, order #10.b, Order #2.c, order #2.d
sort_naturally:
  Order #2.c, Order #10.a, order #2.d, order #10.b
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
{% raw %}
{% assign cars = "Honda Ford Toyota Jeep VW Honda VW" %}
{% endraw %}
{{ cars | split: ' ' | uniq | join: ' ' }}
```
{% endtab %}

{% tab title="Output" %}
```
Honda Ford Toyota Jeep VW
```
{% endtab %}
{% endtabs %}

### unshift \*

This filter prepends any number of arguments onto the provided array, returning a new array, leaving the original unmodified.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% assign count_two_three = "two,three" | split: "," %}

{% assign count_to_three_and_start_at_zero = count_two_three | unshift: "zero", "one" %}
{% endraw %}

{{ count_to_three_and_start_at_zero | join: newline }}
```
{% endtab %}

{% tab title="Output" %}
```
zero
one
two
three
```
{% endtab %}
{% endtabs %}

### where

Takes an array of objects, and create a new array with only those that have a given property value.

{% tabs %}
{% tab title="Code" %}
```javascript
All products:
{% raw %}
{% for product in collection.products %}
- {{ product.title }}
{% endfor %}

{% assign sports_products = collection.products | where: "type", "sports" %}

sports products:
{% for product in sports_products %}
- {{ product.title }}
{% endfor %}
{% endraw %}
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
{% raw %}
{% assign foo = hash %}
{% assign foo["bar"] = "baz" %}
{% assign foo["qux"] = nil %}
{% endraw %}
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

### except \*

This filter accepts one or more string arguments, corresponding to keys that should be left out of the output. The filter returns a new hash, containing all the key/value pairs of the original hash _except_ those keys named as arguments.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% assign foo = hash %}
{% assign foo["bar"] = "bar" %}
{% assign foo["baz"] = "baz" %}
{% assign foo["qux"] = "qux" %}
{% endraw %}

{{ foo | except: "bar", "baz" | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
{"qux":"qux"}
```
{% endtab %}
{% endtabs %}

### keys \*

Returns an array of keys found in the supplied hash.

### slice \*

{% hint style="info" %}
This filter has alternate behavior for [arrays](filters.md#slice-1) and [strings](filters.md#slice)!
{% endhint %}

When applied to a hash, the slice filter accepts one or more string arguments, corresponding to keys that the hash may contain. This filter will then return a new hash, containing only matching key/value pairs from the original hash.

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
{% assign foo = hash %}
{% assign foo["bar"] = "bar" %}
{% assign foo["baz"] = "baz" %}
{% assign foo["qux"] = "qux" %}
{% endraw %}

{{ foo | slice: "bar", "baz" | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
{"bar":"bar","baz":"baz"}
```
{% endtab %}
{% endtabs %}

### values \*

Returns an array of values found in the supplied hash.

## Deprecated filters

We'll keep support for these filters, but we don't recommend using them.

### add\_tag, add\_tags, remove\_tag, remove\_tags

First things first: we don't recommend using Shopify's REST API for tagging. Because this API requires you to specify the _entire_ set of tags all at once, it's very easy to accidentally overwrite the work of another user or system. Instead, we strongly recommend using GraphQL for tagging.

{% content-ref url="../../techniques/tagging-shopify-resources.md" %}
[tagging-shopify-resources.md](../../techniques/tagging-shopify-resources.md)
{% endcontent-ref %}

If you _must_ use REST, you can use these filters to make your life a little easier, and manipulate tag strings and arrays more naturally. (All four of these tag filters are case-sensitive.)

```javascript
{{ "a, b" | add_tag: "c" }}
=> a, b, c

{{ "a, b, e" | add_tags: "c", "d" }}
=> a, b, c, d, e

{{ "a, b, e" | add_tags: "c", "d", sort: false }}
=> a, b, e, c, d

{{ "a, b, c" | remove_tag: "b" }}
=> a, b

{{ "a, b, c" | remove_tags: "a", "c" }}
=> b
```

If supplied an array, these filters will return an array as well:

```javascript
{{ "a,b,e" | split: "," | add_tags: "c", "d" | join: "-" }}
=> a-b-c-d-e

{{ "a,b,c,d,e" | split: "," | remove_tags: "c", "d" | join: "-" }}
=> a-b-e
```
