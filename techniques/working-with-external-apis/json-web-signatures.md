# JSON Web Signatures

From [Wikipedia](https://en.wikipedia.org/wiki/JSON_Web_Signature):

> A JSON Web Signature (abbreviated JWS) is an [IETF](https://en.wikipedia.org/wiki/Internet_Engineering_Task_Force)-proposed standard ([RFC](https://en.wikipedia.org/wiki/RFC_\(identifier\)) [7515](https://datatracker.ietf.org/doc/html/rfc7515)) for [signing](https://en.wikipedia.org/wiki/Digital_signature) arbitrary data.[\[1\]](https://en.wikipedia.org/wiki/JSON_Web_Signature#cite_note-1) This is used as the basis for a variety of web-based technologies including [JSON Web Token](https://en.wikipedia.org/wiki/JSON_Web_Token).

```liquid
{% capture private_key_pem = options.private_key_pem__required_code_multiline %}

{% assign claims = options.payload_json__code_multiline | parse_json | json | base64_url_safe_encode %}
{% assign header = '{"alg":"RS256","typ":"JWT"}' | base64_encode %}
{% assign input = header | append: "." | append: claims %}

{% assign signature = input | rsa_sha256: private_key_pem | base64_url_safe_encode %}

{% log header: header, claims: claims, input: input, signature: signature %}
```
