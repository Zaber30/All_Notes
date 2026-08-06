If the domain of the requester is different from the domain of the receiver then by default the browser will reject the request.The browser will block the request

A **preflight request** is an automatic **HTTP `OPTIONS` request** that a browser sends **before** the actual request when making certain cross-origin requests (known as **CORS** requests).

Its purpose is to ask the server:

> "If I send this request, will you allow it?"

### Why does it happen?

Browsers enforce the **Same-Origin Policy**, which restricts web pages from making requests to a different domain, protocol, or port.

For requests that might be risky (for example, using methods other than `GET`/`POST`, sending custom headers, or certain content types), the browser first sends a preflight request.

---

### Example

Suppose your frontend is running at:

```
https://app.example.com
```

and it wants to send:

```
PUT https://api.example.com/users/123
Authorization: Bearer token
Content-Type: application/json
```

Before sending the `PUT`, the browser sends:

```
OPTIONS /users/123 HTTP/1.1
Origin: https://app.example.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: Authorization, Content-Type
```

The server might respond:

```
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: Authorization, Content-Type
Access-Control-Max-Age: 86400
```

If the response allows it, the browser then sends the actual request:

```
PUT /users/123
Authorization: Bearer token
Content-Type: application/json
```

---

### When is a preflight request sent?

A preflight request is sent if **any** of the following are true:

- The HTTP method is **not** one of:
    - `GET`
    - `HEAD`
    - `POST`
- The request includes **custom headers**, such as:
    - `Authorization`
    - `X-API-Key`
    - `X-Custom-Header`
- The `Content-Type` is **not** one of these "simple" types:
    - `application/x-www-form-urlencoded`
    - `multipart/form-data`
    - `text/plain`

For example:

|Request|Preflight?|
|---|---|
|`GET /users`|❌ No|
|`POST` with `application/json`|✅ Yes|
|`POST` with `text/plain`|❌ No|
|`PUT`|✅ Yes|
|`DELETE`|✅ Yes|
|`GET` with `Authorization` header|✅ Yes|

---

### What headers are involved?

**Browser sends:**

- `Origin`
- `Access-Control-Request-Method`
- `Access-Control-Request-Headers`

**Server responds with:**

- `Access-Control-Allow-Origin`
- `Access-Control-Allow-Methods`
- `Access-Control-Allow-Headers`
- `Access-Control-Max-Age` (optional, lets the browser cache the preflight result)

---

### Why is it useful?

It prevents a browser from sending potentially sensitive cross-origin requests to a server that hasn't explicitly opted in to handling them.

For example:

1. Browser asks:
    
    > "Can I send a `DELETE` request with an `Authorization` header?"
    
2. Server replies:
    
    > "Yes, I allow `DELETE` from `https://app.example.com`."
    
3. Browser proceeds with the actual request.

If the server doesn't allow it, the browser blocks the request before it's sent.

---

### Visual flow

```
Frontend                    Browser                    API Server
    |                           |                           |
    | fetch(...)                |                           |
    |-------------------------->|                           |
    |                           | OPTIONS (preflight)       |
    |                           |-------------------------->|
    |                           |                           |
    |                           | 204 + Allow-* headers     |
    |                           |<--------------------------|
    |                           |                           |
    |                           | Actual PUT/POST/DELETE    |
    |                           |-------------------------->|
    |                           |                           |
    |                           | Response                  |
    |                           |<--------------------------|
    |<--------------------------|                           |
```

The important thing to remember is that **your JavaScript code does not explicitly send the preflight request**. The **browser** decides whether one is required based on the request's method, headers, and content type, and handles it automatically as part of the CORS protocol.