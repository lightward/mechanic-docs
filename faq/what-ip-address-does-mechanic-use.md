# What IP address does Mechanic use?

Mechanic uses dynamically-assigned IP addresses, and does not support static or fixed IP addresses for outgoing or inbound requests.

The HTTP action supports HTTPS, HTTP, and SOCKS5 proxies, allowing you to set up your own proxy server with a static IP address, passing your Mechanic requests through it. Using this strategy, you can control the client IP addresses used for your outbound requests. This can be useful for API vendors that have IP address restrictions. Learn more: [Actions / HTTP / Using a proxy](../core/actions/http.md#using-a-proxy).
