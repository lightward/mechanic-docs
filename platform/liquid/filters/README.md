# Mechanic filters

This page defines all of the [**Liquid filters**](../basics/filters.md) that are available in Mechanic Liquid. Mechanic supports many of our own filters, as well as an array of filters drawn from Shopify Liquid.

{% hint style="info" %}
Note that not all Shopify Liquid filters are supported by Mechanic. If a filter is supported by Shopify but it's not included on this page, it's not available in Mechanic Liquid.
{% endhint %}

{% hint style="warning" %}
Liquid filters should not be confused with [event filters](../../events/filters.md), which are used to conditionally ignore incoming events.
{% endhint %}

## Data filters

### browser

This filter converts a browser user agent string into an object that represents the browser itself. Data from[ Browserscope](https://github.com/ua-parser/uap-core) is used to match user agents.

{% tabs %}
{% tab title="Code" %}

```liquid
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

### csv, parse\_csv

Supports converting a two-dimensional array to a CSV string, and back again.

#### Arguments

* `delimiter` (optional): A single character that separates the fields in the CSV. Defaults to a comma.
* `include_bom` (optional): A boolean value that, when set to true, includes a Byte Order Mark (BOM) at the beginning of the CSV string to enhance compatibility with Microsoft Excel. Defaults to false.

{% tabs %}
{% tab title="Default" %}

```liquid
{{ data | csv }}
```

{% endtab %}

{% tab title="Custom Delimiter" %}

```javascript
{{ data | csv: ";" }}
```

{% endtab %}

{% tab title="Include BOM" %}

```liquid
{{ data | csv: include_bom: true }}
```

{% endtab %}

{% tab title="Custom Delimiter and BOM" %}

```liquid
{{ data | csv: ";", include_bom: true }}
```

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Example Usage csv and parse_csv filters" %}

```liquid
{% if event.preview %}
  {% capture shop_json %}
    {
      "products": [
        {
          "id": 1234567890,
          "title": "Short sleeve t-shirt"
        }
      ],
      "name": "My Test Shop",
      "customer_email": "shop@example.com"
    }
  {% endcapture %}

  {% assign shop = shop_json | parse_json %}
{% endif %}

{% assign rows = array %}

{% assign header = array %}
{% assign header[0] = "Product ID" %}
{% assign header[1] = "Product Title" %}

{% assign rows[0] = header %}

{% for product in shop.products %}
  {% assign row = array %}
  {% assign row[0] = product.id %}
  {% assign row[1] = product.title %}

  {% assign rows[rows.size] = row %}
{% endfor %}

{%- comment -%}
  Generate CSV with custom delimiter and include BOM for Excel compatibility
{%- endcomment -%}
{% assign csv = rows | csv: ";", include_bom: true %}

{% action "email" %}
  {
    "to": {{ options.email_recipient__email_required | json }},
    "subject": {{ "Product ID export for " | append: shop.name | json }},
    "body": "Please see attached. :)",
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }},
    "attachments": {
      "export.csv": {{ csv | json }}
    }
  }
{% endaction %}

{% action "echo" csv %}

{%- capture csv_string_default_delimiter -%}
Order Name,Order ID,Order Date
#1234,1234567890,2021/03/23
#1235,1234567891,2021/03/24
{%- endcapture %}

{%- capture csv_string_custom_delimiter -%}
Order Name;Order ID;Order Date
#1234;1234567890;2021/03/23
#1235;1234567891;2021/03/24
{%- endcapture %}

{% comment %}
  Parse CSV with default delimiter (comma)
{% endcomment %}
{% assign orders_with_default_delimiter = csv_string_default_delimiter | parse_csv: headers: true %}
{% action "echo" orders_with_default_delimiter %}

{% comment %}
  Parse CSV with custom delimiter (semicolon)
{% endcomment %}
{% assign orders_with_custom_delimiter = csv_string_custom_delimiter | parse_csv: headers: true, delimiter: ";" %}
{% action "echo" orders_with_custom_delimiter %}

{% comment %}
  Parse CSV without headers, default delimiter
{% endcomment %}
{% assign csv_rows_default_delimiter = csv_string_default_delimiter | parse_csv %}

{% assign orders_without_headers_default_delimiter = array %}
{% for row in csv_rows_default_delimiter %}
  {% if forloop.first %}
    {% continue %}
  {% endif %}

  {% assign order = hash %}
  {% assign order["name"] = row[0] %}
  {% assign order["id"] = row[1] | times: 1 %}
  {% assign order["date"] = row[2] %}

  {% assign orders_without_headers_default_delimiter[orders_without_headers_default_delimiter.size] = order %}
{% endfor %}

{% action "echo" orders_without_headers_default_delimiter %}

{% comment %}
  Parse CSV without headers, custom delimiter
{% endcomment %}
{% assign csv_rows_custom_delimiter = csv_string_custom_delimiter | parse_csv: delimiter: ";" %}

{% assign orders_without_headers_custom_delimiter = array %}
{% for row in csv_rows_custom_delimiter %}
  {% if forloop.first %}
    {% continue %}
  {% endif %}

  {% assign order = hash %}
  {% assign order["name"] = row[0] %}
  {% assign order["id"] = row[1] | times: 1 %}
  {% assign order["date"] = row[2] %}

  {% assign orders_without_headers_custom_delimiter[orders_without_headers_custom_delimiter.size] = order %}
{% endfor %}

{% action "echo" orders_without_headers_custom_delimiter %}
```

{% endtab %}
{% endtabs %}

### date, parse\_date

Mechanic's date filter is based on [Shopify's date filter](https://shopify.dev/docs/themes/liquid/reference/filters/additional-filters#date), and has several important extensions.

{% code title="Quick reference" %}

```liquid
{{ "now"
    | date: [format,]

      [tz: "America/Chicago",]

      [beginning_of_year: true,]
      [end_of_year: true,]

      [beginning_of_quarter: true,]
      [end_of_quarter: true,]

      [beginning_of_month: true,]
      [end_of_month: true,]

      [beginning_of_week: "monday",]
      [end_of_week: "sunday",]

      [beginning_of_day: true,]
      [end_of_day: true,]
      [middle_of_day: true,]

      [advance: "1 day 10 minutes",]
}}
```

{% endcode %}

#### Format

Date formats may be given per[ Ruby's strftime](http://www.ruby-doc.org/core/Time.html#method-i-strftime). For an interactive format-building tool, see [strfti.me](https://www.strfti.me/).

Unlike Shopify Liquid, Mechanic's date filter does not require a format argument. If one is not given, Mechanic defaults to formatting the date per ISO8601.

```liquid
{{ "2000-01-01" | date: "%Y-%m-%d %H:%M %z" }}
=> "2000-01-01 00:00 -0600"

{{ "2000-01-01" | date }}
=> "2000-01-01T00:00:00-06:00"

{{ "2000-01-01" | date: tz: "UTC" }}
=> "2000-01-01T06:00:00Z"
```

#### Using the current time

This filter accepts the special value `"now"`. This may optionally be combined with a single date calculation, as in `"now + 5 days"` or `"now - 5 weeks"`. For specifics on date calculation, see notes below for the `advance` option.

#### Additional options

The date filter also accepts these following options, evaluated in the following order:

1. `tz` — [A timezone name from the TZ database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
   * If given, the resulting time string will be in the specified timezone.
   * If this option is not provided, the time is assumed to be in the store's local timezone, as configured at the Shopify level.
   * All date calculations are performed with respect to the current timezone, with consideration for DST and other calendar variances.
2. `beginning_of_year: true` or `end_of_year: true`
3. `beginning_of_quarter: true` or `end_of_quarter: true`
4. `beginning_of_month: true` or `end_of_month: true`
5. `beginning_of_week: weekday` or `end_of_month: weekday`
   * `weekday` must be a string naming the first day of the week for the intended usage, e.g. `"sunday"` or `"monday"`
6. `beginning_of_day: true` or `middle_of_day: true` or `end_of_day: true`
7. `advance: "1 year 6 months"`
   * Supports any combination of years, months, weeks, days, hours, minutes, seconds
   * Supports positive and negative values
   * Durations are calculated in the order given, left to right
   * Seconds, minutes, and hours are all implemented as constant intervals
   * Days, weeks, months, and years are all variable-length, appropriate for the current time in the current timezone (see `tz`). For example, `{{ "2023-01-31" | date: "%F", advance: "1 year 1 month" }}` returns `2024-02-29`.
   * Commas and signs may be used for clarity. Pluralization is optional. All of the examples below are equivalent:
     * `{{ "now" | date: advance: "-3 hours, +2 minutes, -1 second" }}`
     * `{{ "now" | date: advance: "-3 hours, 2 minutes, -1 second" }}`
     * `{{ "now" | date: advance: "-3 hour 2 minute -1 second" }}`
     * `{{ "now" | date: advance: "-3 hours 2 minutes -1 seconds" }}`

#### Parsing dates

Use parse\_date to parse a date string, when its exact format is known. This filter is useful for strings that contain an ambiguous date value, like `"01/01/01"`.

Under the hood, parse\_date uses [ActiveSupport::TimeZone#strptime](https://api.rubyonrails.org/classes/ActiveSupport/TimeZone.html#method-i-strptime), and inherits its behavior with regard to missing upper components.

This filter returns an ISO8601 string, representing the parsed date value in the store's local timezone. If the supplied date string cannot be parsed successfully, the filter will return nil.

```liquid
{{ "01-01-20" | parse_date: "%m-%d-%y" }}
=> "2020-01-01T00:00:00+11:00"

{{ "01-01-20" | parse_date: "%m-%d-%y" | date: "%Y-%m-%d" }}
=> "2020-01-01"

{{ "ab-cd-ef" | parse_date: "%m-%d-%y" }}
=> nil
```

### gzip, gunzip

These filters allow you to compress and decompress strings, using gzip compression.

In general, all strings passing through Mechanic must be UTF-8, and must ultimately be valid when represented as JSON. However, because gzip'd content may _not_ be UTF-8, and because it may be important to preserve the original encoding, the gunzip filter supports a `force_utf8: false` option. Use this when you're certain the original encoding must be preserved, _if_ you ultimately intend to pass along the string in a JSON-friendly representation. (For example, you might gunzip a value, and then use the base64 filter to represent it safely within JSON.)

```liquid
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

### graphql\_arguments

Useful for preparing key-value pairs of GraphQL query or mutation arguments.

{% hint style="info" %}
Across the documentation and [task library](../../../resources/task-library/), you'll frequently see `json` used for serializing argument values. Users have reported some rare cases where this filter is insufficient, and where `graphql_arguments` does the trick instead.
{% endhint %}

{% hint style="info" %}
`graphql_arguments` is typically used for rendering GraphQL values into the final GraphQL query string itself. Instead, consider extracting your values as GraphQL variables. This approach can result in more reusable query code.

To try this using a Shopify action, use the [GraphQL with variables](../../../core/actions/shopify.md#graphql-with-variables) syntax.

To try this using the shopify filter, use the [variables](./#graphql-variables) argument.
{% endhint %}

```liquid
{% assign inputs = hash %}
{% assign inputs["a_string"] = "yep this is a string" %}
{% assign inputs["a_more_complex_type"] = hash %}
{% assign inputs["a_more_complex_type"]["id"] = "gid://something/Or?other" %}
{% assign inputs["an_array"] = array %}
{% assign inputs["an_array"][0] = 1 %}
{% assign inputs["an_array"][1] = 2 %}

{% action "shopify" %}
  mutation {
    anExample({{ inputs | graphql_arguments }}) {
      result
    }
  }
{% endaction %}
```

This results in a [GraphQL Shopify action](../../../core/actions/shopify.md#graphql) containing the following GraphQL:

```graphql
mutation {
  anExample(
    a_string: "yep this is a string"
    a_more_complex_type: { id: "gid://something/Or?other" }
    an_array: [1, 2]
  ) {
    result
  }
}
```

For a more complex example, see [Set product or variant metafields values in bulk](https://github.com/lightward/mechanic-tasks/blob/ce13e354c263f47bb77d80618fc90af5064d10bc/docs/set-product-or-variant-metafields-in-bulk/script.liquid#L127) from the task library.

### json, parse\_json

Allows converting objects to their JSON representations, and parsing that JSON into hashes.

```liquid
{% assign order_as_json = order | json %}
{% assign plain_order = order_as_json | parse_json %}
```

The parse\_json filter raises an error when invalid JSON. To ignore parse errors, and to return null when an error is encountered, add `silent: true` to the filter's options:

```liquid
{% assign should_be_nil = "{{" | parse_json: silent: true %}
```

### jsonl, parse\_jsonl

Allows for rendering an iterable object (i.e. an array) as a series of JSON lines, separated by simple newlines.

```liquid
{{ shop.customers | jsonl }}
```

The parse\_jsonl filter can be used to parse a series of JSON strings, each on their own line, into an array of hashes. Useful when preparing [stub data](../../../core/tasks/previews/stub-data.md) for [bulk operations](../../graphql/bulk-operations.md).

```liquid
{% capture jsonl_string %}
  {"id":"gid://shopify/Customer/12345","email":"foo@bar.baz"}
  {"id":"gid://shopify/Customer/67890","email":"bar@baz.qux"}
{% endcapture %}

{% assign json_objects = jsonl_string | parse_jsonl %}

{{ json_objects | map: "email" | join: ", " }}
```

The parse\_jsonl filter raises an error when invalid JSONL is received.

### parse\_xml

Use this filter to parse an XML string. (Under the hood, this filter calls [Hash::from\_xml](https://api.rubyonrails.org/classes/Hash.html#method-c-from_xml).) Useful for processing output from third-party APIs, either by [responding to](https://docs.usemechanic.com/article/431-responding-to-action-results) "http" actions, or by parsing content from [inbound webhooks](https://docs.usemechanic.com/article/439-creating-events-with-webhooks).

```liquid
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

```json
{"foo":{"bar":["baz",{"qux":"quux"}]}}
```

### shopify

This filter accepts a GraphQL query string, sends it to Shopify, and returns the full response – including `"data"` and `"errors"`.

{% hint style="info" %}
Use [Shopify's GraphiQL query builder](https://shopify.dev/apps/tools/graphiql-admin-api) to quickly and precisely assemble your queries.
{% endhint %}

{% tabs %}
{% tab title="Usage" %}

```liquid
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

{% hint style="info" %}
Variables can be a useful part of making queries reusable within a task, or for working around [Shopify's 50,000 character limit for GraphQL queries](../../../faq/query-param-length-is-too-long.md).
{% endhint %}

```liquid
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
```

### Shopify Data filters

In addition to our own filters, Mechanic supports the following data filter from Shopify Liquid:

* [default](https://shopify.dev/docs/api/liquid/filters/default)

***

## String filters

### e164

This filter accepts a phone number – country code is required! – and outputs it in [standard E.164 format](https://en.wikipedia.org/wiki/E.164). If the number does not appear valid, the filter returns `nil`.

```liquid
{{ "1 (312) 456-7890" | e164 }}
=> "13124567890"

{{ "+43 670 1234567890" | e164 }}
=> "436701234567890"

{{ "000" | e164 | json }}
=> "null"
```

### match

Use this filter to match a string with a Ruby-compatible regular expression pattern (see [Regexp](https://ruby-doc.org/core/Regexp.html)).

This filter returns the entire matched string (i.e. [MatchData#to\_s](https://ruby-doc.org/core/MatchData.html#method-i-to_s)). Use the "captures" or "named\_captures" lookups to receive an array or hash of captures, respectively (i.e. [MatchData#captures](https://ruby-doc.org/core/MatchData.html#method-i-captures), [MatchData#named\_captures](https://ruby-doc.org/core/MatchData.html#method-i-named_captures)).

{% hint style="info" %}
This filter only returns the first match found. To find all available matches in a string, use [scan](./#scan).
{% endhint %}

```liquid
{{ "It's a lovely day!" | match: "(?<=a ).*(?= day)" }}
=> "lovely"

{% assign match = "It's a lovely day!" | match: "a (bucolic|lovely) day" %}
{{ match.captures }}
=> ["lovely"]

{% assign match = "It's a lovely day!" | match: "a (?<adjective>bucolic|lovely) day" %}
{{ match.named_captures }}
=> {"adjective" => "lovely"}

{% assign match = "It's a lovely day!" | match: "a (?i:LOVELY) day" %}
{{ match }}
=> "a lovely day"
```

### hmac\_sha512

Works like [hmac\_sha256 from Shopify Liquid](https://shopify.dev/docs/api/liquid/filters/hmac_sha256), but uses SHA-512 instead.

### rsa\_sha256, rsa\_sha512

Accepts string input, given an RSA PEM key string as a filter option.

{% hint style="info" %}
This filter is useful for generating [JSON Web Signatures](../../../techniques/working-with-external-apis/json-web-signatures.md)!
{% endhint %}

```liquid
{{ input | rsa_sha256: private_key_pem }}
{{ input | rsa_sha256: private_key_pem, binary: true | base64_encode }}
```

```liquid
{{ input | rsa_sha512: private_key_pem }}
{{ input | rsa_sha512: private_key_pem, binary: true | base64_encode }}
```

### scan

Use this filter to find all available matches in a string, using a Ruby-compatible regular expression pattern (see [Regexp](https://ruby-doc.org/core/Regexp.html)).

This filter returns an array of matches, consisting of each matched string (i.e. [MatchData#to\_s](https://ruby-doc.org/core/MatchData.html#method-i-to_s)). Use the "captures" or "named\_captures" lookups on individual matches to receive an array or hash of captures, respectively (i.e. [MatchData#captures](https://ruby-doc.org/core/MatchData.html#method-i-captures), [MatchData#named\_captures](https://ruby-doc.org/core/MatchData.html#method-i-named_captures)).

{% hint style="info" %}
This filter returns an array of matches. To only find the first match, use [match](./#match).
{% endhint %}

```liquid
{{ "It's a lovely day!" | scan: "[\w']+" }}
=> ["It's", "a", "lovely", "day"]

{{ "It's a lovely day!" | scan: "(bucolic|lovely|day)" | map: "captures" }}
=> [["lovely"], ["day"]]

{{ "It's a lovely day!" | scan: "(?<punctuation>[[:punct:]])" | map: "named_captures" }}
=> [{"punctuation" => "'"}, {"punctuation" => "!"}]
```

### sha512

Works like [sha256 from Shopify Liquid](https://shopify.dev/docs/api/liquid/filters/sha256), but uses SHA-512 instead.

### unindent

Use this filter on strings to remove indentation from strings.

{% tabs %}
{% tab title="Code" %}

```liquid
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

### Shopify String filters

In addition to our own filters, Mechanic supports the following string filters from Shopify Liquid:

* [append](https://shopify.dev/docs/api/liquid/filters/append)
* [base64\_decode](https://shopify.dev/docs/api/liquid/filters/base64_decode)
* [base64\_encode](https://shopify.dev/docs/api/liquid/filters/base64_encode)
* [base64\_url\_safe\_decode](https://shopify.dev/docs/api/liquid/filters/base64_url_safe_decode)
* [base64\_url\_safe\_encode](https://shopify.dev/docs/api/liquid/filters/base64_url_safe_encode)
* [capitalize](https://shopify.dev/docs/api/liquid/filters/capitalize)
* [downcase](https://shopify.dev/docs/api/liquid/filters/downcase)
* [escape](https://shopify.dev/docs/api/liquid/filters/escape)
* [escape\_once](https://shopify.dev/docs/api/liquid/filters/escape_once)
* [handleize](https://shopify.dev/docs/api/liquid/filters/handleize) / [handle](https://shopify.dev/docs/api/liquid/filters/handleize)
* [hmac\_sha1](https://shopify.dev/docs/api/liquid/filters/hmac_sha1)
* [hmac\_sha256](https://shopify.dev/docs/api/liquid/filters/hmac_sha256)
* [lstrip](https://shopify.dev/docs/api/liquid/filters/lstrip)
* [md5](https://shopify.dev/docs/api/liquid/filters/md5)
* [newline\_to\_br](https://shopify.dev/docs/api/liquid/filters/newline_to_br)
* [pluralize](https://shopify.dev/docs/api/liquid/filters/pluralize)
* [prepend](https://shopify.dev/docs/api/liquid/filters/prepend)
* [remove](https://shopify.dev/docs/api/liquid/filters/remove)
* [remove\_first](https://shopify.dev/docs/api/liquid/filters/remove_first)
* [remove\_last](https://shopify.dev/docs/api/liquid/filters/remove_last)
* [replace](https://shopify.dev/docs/api/liquid/filters/replace)
* [replace\_first](https://shopify.dev/docs/api/liquid/filters/replace_first)
* [replace\_last](https://shopify.dev/docs/api/liquid/filters/replace_last)
* [rstrip](https://shopify.dev/docs/api/liquid/filters/rstrip)
* [sha1](https://shopify.dev/docs/api/liquid/filters/sha1)
* [sha256](https://shopify.dev/docs/api/liquid/filters/sha256)
* [slice](https://shopify.dev/docs/api/liquid/filters/slice)
* [split](https://shopify.dev/docs/api/liquid/filters/split)
* [strip](https://shopify.dev/docs/api/liquid/filters/strip)
* [strip\_html](https://shopify.dev/docs/api/liquid/filters/strip_html)
* [strip\_newlines](https://shopify.dev/docs/api/liquid/filters/strip_newlines)
* [truncate](https://shopify.dev/docs/api/liquid/filters/truncate)
* [truncatewords](https://shopify.dev/docs/api/liquid/filters/truncatewords)
* [upcase](https://shopify.dev/docs/api/liquid/filters/upcase)
* [url\_decode](https://shopify.dev/docs/api/liquid/filters/url_decode)
* [url\_encode](https://shopify.dev/docs/api/liquid/filters/url_encode)

***

## Math filters

### currency

Formats a number (given as an [integer](../basics/types.md#integer-float), [float](../basics/types.md#integer-float), or [string](../basics/types.md#string)) as currency. Called with no arguments, this filter uses the store's primary currency and default locale.

A three-character ISO currency code may be specified as the first argument; currency support is drawn from the [money](https://github.com/RubyMoney/money/blob/main/config/currency_iso.json) project. The locale may be overridden as a named option; locale support is drawn from [rails-i18n](https://github.com/svenfuchs/rails-i18n#available-locales).

{% tabs %}
{% tab title="Code" %}

```liquid
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

```liquid
{{ price | currency }} {{ shop.currency }}
{{ price | currency | append: " " | append: shop.currency }}
```

### Shopify Math filters

In addition to our own filters, Mechanic supports the following math filters from Shopify Liquid:

* [abs](https://shopify.dev/docs/api/liquid/filters/abs)
* [at\_least](https://shopify.dev/docs/api/liquid/filters/at_least)
* [at\_most](https://shopify.dev/docs/api/liquid/filters/at_most)
* [ceil](https://shopify.dev/docs/api/liquid/filters/ceil)
* [divided\_by](https://shopify.dev/docs/api/liquid/filters/divided_by)
* [floor](https://shopify.dev/docs/api/liquid/filters/floor)
* [minus](https://shopify.dev/docs/api/liquid/filters/minus)
* [modulo](https://shopify.dev/docs/api/liquid/filters/modulo)
* [plus](https://shopify.dev/docs/api/liquid/filters/plus)
* [round](https://shopify.dev/docs/api/liquid/filters/round)
* [times](https://shopify.dev/docs/api/liquid/filters/times)

***

## Array filters

### in\_groups

This filter is an implementation of [Array#in\_groups](https://api.rubyonrails.org/classes/Array.html#method-i-in_groups). It accepts an array, and an integer count, and – optionally – a "fill\_with" option.

{% tabs %}
{% tab title="Code" %}

```liquid
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

```liquid
{{ "1,2,3" | split: "," | in_groups: 2, fill_with: false | json }}
```

{% endtab %}

{% tab title="Output" %}

```javascript
[["1","2"],["3"]]
```

{% endtab %}
{% endtabs %}

### in\_groups\_of

This filter is an implementation of [Array#in\_groups\_of](https://api.rubyonrails.org/classes/Array.html#method-i-in_groups_of). It accepts an array, and an integer count, and – optionally – a "fill\_with" option.

This filter is particularly useful when performing work in batches, by making it easy to split an array of potentially large size into smaller pieces of controlled size.

{% tabs %}
{% tab title="Code" %}

```liquid
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

```liquid
{{ "1,2,3,4,5" | split: "," | in_groups_of: 2, fill_with: false | json }}
```

{% endtab %}

{% tab title="Output" %}

```
[["1","2"],["3","4"],["5"]]
```

{% endtab %}
{% endtabs %}

### index\_by

This filter accepts the name of an object property or attribute, and returns a hash that whose values are every element in the array, keyed by every element's corresponding property or attribute.

{% tabs %}
{% tab title="Code" %}

```liquid
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

### push

This filter appends any number of arguments onto the provided array, returning a new array, leaving the original unmodified.

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign count_to_three = "one,two,three" | split: "," %}

{% assign count_to_five = count_to_three | push: "four", "five" %}

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

### except

This filter accepts one or more arguments, corresponding to values that should be left out of the output. The filter returns a new array, containing all the elements of the original array _except_ those values.

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign items = array | push: "bar", "baz", "qux" %}
{{ items | except: "bar", "baz" | json }}
```

{% endtab %}

{% tab title="Output" %}

```javascript
["qux"]
```

{% endtab %}
{% endtabs %}

### sample

This filter can be used on any array. Used without any arguments, it returns a single random element from the array. Provide an integer argument to return another array of that size, containing a random subset of the input array.

```liquid
{{ "1,2,3" | split: "," | sample }}
=> "2"

{{ "1,2,3" | split: "," | sample: 2 | join: "," }}
=> "3,1"
```

### slice

When applied to an array, this filter accepts an integer offset, and an optional integer length (defaulting to 1). If the length is 1, it returns the single element found at that index of the input array. Otherwise, it returns a slice of the array, beginning at the provided index, having the provided length.

Negative offsets begin counting from the end of the array.

```liquid
{{ "1,2,3,4,5" | split: "," | slice: 3 }}
=> "4"

{{ "1,2,3,4,5" | split: "," | slice: 3, 2 | join: "," }}
=> "4,5"

{{ "1,2,3,4,5" | split: "," | slice: -3, 2 | join: "," }}
=> "3,4"
```

### sort\_naturally

Sorts an array uses the human-friendly sort order defined by [naturally](https://github.com/dogweather/naturally). Accepts a single optional parameter, specifying an attribute to sort.

{% hint style="info" %}
This filter complements Shopify Liquid's [sort](https://shopify.dev/docs/api/liquid/filters/sort) and [sort\_natural](https://shopify.dev/docs/api/liquid/filters/sort_natural) filters. Choose your sort filter intentionally: machine audiences are typically happier with "sort", and human audiences are typically happier with "sort\_naturally".
{% endhint %}

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign set = "order #10.b,Order #10.a,Order #2.c,order #2.d" | split: "," %}

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

### sum

Returns the sum of all elements in an array.

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign primes = array | push: 2, 3, 5, 7, 11 %}
{{ primes | sum }}
```

{% endtab %}

{% tab title="Output" %}

```
28
```

{% endtab %}
{% endtabs %}

Note that Mechanic's implementation of this filter predates [Shopify's sum filter](https://shopify.dev/docs/api/liquid/filters/sum), and so there are two notable differences:

1. In Mechanic, the `sum` filter requires that every element of the array be a number.
2. In Mechanic, the sum filter cannot operate on an array of objects. To achieve similar behavior to Shopify, you can combine `sum` with the `map` filter:

{% tabs %}
{% tab title="Code" %}

```liquid
Original quantities: {{ cart.items | map: "quantity" | join: "," }}
Sum of quantities:   {{ cart.items | map: "quantity" | sum }}
```

{% endtab %}

{% tab title="Output" %}

```
Original quantities: 1,1,2,1,1
Sum of quantities:   6
```

{% endtab %}
{% endtabs %}

### unshift

This filter prepends any number of arguments onto the provided array, returning a new array, leaving the original unmodified.

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign count_two_three = "two,three" | split: "," %}

{% assign count_to_three_and_start_at_zero = count_two_three | unshift: "zero", "one" %}

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

### Shopify Array filters

In addition to our own filters, Mechanic supports the following array filters from Shopify Liquid:

* [compact](https://shopify.dev/docs/api/liquid/filters/compact)
* [concat](https://shopify.dev/docs/api/liquid/filters/concat)
* [first](https://shopify.dev/docs/api/liquid/filters/first)
* [join](https://shopify.dev/docs/api/liquid/filters/join)
* [last](https://shopify.dev/docs/api/liquid/filters/last)
* [map](https://shopify.dev/docs/api/liquid/filters/map)
* [reverse](https://shopify.dev/docs/api/liquid/filters/reverse)
* [size](https://shopify.dev/docs/api/liquid/filters/size)
* [sort](https://shopify.dev/docs/api/liquid/filters/sort)
* [sort\_natural](https://shopify.dev/docs/api/liquid/filters/sort_natural)
* [uniq](https://shopify.dev/docs/api/liquid/filters/uniq)
* [where](https://shopify.dev/docs/api/liquid/filters/where)

***

## Hash filters

### compact

When applied to a hash, this filter returns a new hash which omits all keys having _nil_ values.

{% tabs %}
{% tab title="Code" %}

```liquid
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

Note that the filter only operates on the top-level keys, it does not affect keys in lower levels of a nested hash. If you need to omit _nil_ keys from a nested hash, you must apply the compact filter to the nested hash directly.

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign foo = hash %}
{% assign foo["bar"] = hash %}
{% assign foo["bar"]["baz"] = "baz" %}
{% assign foo["bar"]["quz"] = nil %}
{{ foo | compact | json }}

{% assign foo["bar"] = foo["bar"] | compact %}
{{ foo | json }}
```

{% endtab %}

{% tab title="Output" %}

```javascript
{"bar":{"baz":"baz","quz":null}}
{"bar":{"baz":"baz"}}
```

{% endtab %}
{% endtabs %}

### except

This filter accepts one or more string arguments, corresponding to keys that should be left out of the output. The filter returns a new hash, containing all the key/value pairs of the original hash _except_ those keys named as arguments.

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign foo = hash %}
{% assign foo["bar"] = "bar" %}
{% assign foo["baz"] = "baz" %}
{% assign foo["qux"] = "qux" %}

{{ foo | except: "bar", "baz" | json }}
```

{% endtab %}

{% tab title="Output" %}

```javascript
{"qux":"qux"}
```

{% endtab %}
{% endtabs %}

### keys

Returns an array of keys found in the supplied hash.

### slice

When applied to a hash, the slice filter accepts one or more string arguments, corresponding to keys that the hash may contain. This filter will then return a new hash, containing only matching key/value pairs from the original hash.

{% tabs %}
{% tab title="Code" %}

```liquid
{% assign foo = hash %}
{% assign foo["bar"] = "bar" %}
{% assign foo["baz"] = "baz" %}
{% assign foo["qux"] = "qux" %}

{{ foo | slice: "bar", "baz" | json }}
```

{% endtab %}

{% tab title="Output" %}

```javascript
{"bar":"bar","baz":"baz"}
```

{% endtab %}
{% endtabs %}

### values

Returns an array of values found in the supplied hash.
