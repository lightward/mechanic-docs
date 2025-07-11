# AWS request signatures

{% hint style="info" %}
The "hmac\_sha256" filter comes from Shopify Liquid, but the "binary" option is unique to Mechanic Liquid.
{% endhint %}

```liquid
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

aws: {{ kSigning }}
```
