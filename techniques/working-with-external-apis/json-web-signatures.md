# JSON Web Signatures

From [Wikipedia](https://en.wikipedia.org/wiki/JSON\_Web\_Signature):

> A JSON Web Signature (abbreviated JWS) is an [IETF](https://en.wikipedia.org/wiki/Internet\_Engineering\_Task\_Force)-proposed standard ([RFC](https://en.wikipedia.org/wiki/RFC\_\(identifier\)) [7515](https://datatracker.ietf.org/doc/html/rfc7515)) for [signing](https://en.wikipedia.org/wiki/Digital\_signature) arbitrary data.[\[1\]](https://en.wikipedia.org/wiki/JSON\_Web\_Signature#cite\_note-1) This is used as the basis for a variety of web-based technologies including [JSON Web Token](https://en.wikipedia.org/wiki/JSON\_Web\_Token).

```liquid
{% raw %}
{% capture private_key_pem = options.private_key_pem__required_code_multiline %}

{% assign claims = options.payload_json__code_multiline | parse_json | json | base64_url_safe_encode %}
{% assign header = '{"alg":"RS256","typ":"JWT"}' | base64_encode %}
{% assign input = header | append: "." | append: claims %}

{% assign signature = input | rsa_sha256: private_key_pem | base64_url_safe_encode %}

{% log header: header, claims: claims, input: input, signature: signature %}
{% endraw %}
```
