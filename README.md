## Demo and Screenshots

Add screenshots or recordings only from an authorized lab environment. Redact credentials, cookies, access tokens, private messages, personal information, and other sensitive data.

### Live traffic capture proof

The screenshot below shows the live Burp Suite capture in an authorized testing workflow. The request history is logging active Instagram traffic after the pinning check is bypassed, and the response pane reveals real-time HTTP metadata and JSON payload content.

```text
Burp Suite: Live Traffic Capture
--------------------------------
Host: i.instagram.com
Method: GET
Path: /api/v1/...
Status: HTTP/2 200 OK
Content-Type: text/javascript

Request/Response History:
- Request headers visible in Burp Suite
- Response body returned as JSON-like app payload
- Active app flow under inspection in the proxy
- Real-time traffic observed from the Android test device
```

This is proof of live traffic capture from a controlled lab setup: active application requests are being intercepted and displayed in real time, including the request path, HTTP status, response type, and decrypted traffic content.

- `[Screenshot placeholder: Frida session using the verified launch command]`
- `[Screenshot placeholder: sanitized ig_traffic.log output]`
- `[Screenshot placeholder: Tigon plaintext request and response capture]`
- `[Demo placeholder: authorized Android security-testing workflow]`
