# 1. What is an API?

**Answer:**  
An **API (Application Programming Interface)** is a set of rules that allows two software applications to communicate with each other.

**Example:**

- Mobile App → API → Database
- Frontend → API → Backend

---

# 2. What is REST API?

**Answer:**  
A REST API is an API that follows the REST architectural principles and uses HTTP methods to perform CRUD operations.

Example:

```
GET /users
POST /users
PUT /users/5
DELETE /users/5
```

---

# 3. What does REST stand for?

**Answer:**

Representational State Transfer

---

# 4. What are HTTP methods?

|Method|Purpose|
|---|---|
|GET|Read data|
|POST|Create data|
|PUT|Update entire resource|
|PATCH|Update partial resource|
|DELETE|Delete resource|

---

# 5. Difference between PUT and PATCH?

**PUT**

- Replaces the whole resource.

Example

```
PUT /users/1

{
  "name":"John",
  "email":"john@gmail.com"
}
```

Entire object is replaced.

---

**PATCH**

Updates only specified fields.

```
PATCH /users/1

{
  "email":"new@gmail.com"
}
```

Only email changes.

---

# 6. What is CRUD?

|Operation|HTTP|
|---|---|
|Create|POST|
|Read|GET|
|Update|PUT/PATCH|
|Delete|DELETE|

---

# 7. What is an Endpoint?

**Answer**

A URL through which an API can be accessed.

Example

```
GET /api/users
```

---

# 8. What is a Resource?

A resource is an object managed by an API.

Examples

- User
- Product
- Order
- Student

---

# 9. What is JSON?

JSON stands for

**JavaScript Object Notation**

It is the most common format for sending data between client and server.

Example

```
{
  "id":1,
  "name":"John"
}
```

---

# 10. Why is JSON preferred?

- Lightweight
- Easy to read
- Easy to parse
- Language independent

---

# 11. What is Stateless API?

Every request is independent.

The server does not remember previous requests.

Example

```
Request 1
↓

Server

Request 2

Server doesn't remember Request 1
```

---

# 12. What is Client-Server Architecture?

- Client sends request.
- Server processes request.
- Server returns response.

---

# 13. What is HTTP Status Code?

Status codes indicate the result of an HTTP request.

---

# 14. Common HTTP Status Codes

|Code|Meaning|
|---|---|
|200|OK|
|201|Created|
|204|No Content|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|405|Method Not Allowed|
|409|Conflict|
|422|Validation Error|
|500|Internal Server Error|
|503|Service Unavailable|

---

# 15. Difference between 401 and 403?

**401 Unauthorized**

- Authentication required or invalid credentials.

**403 Forbidden**

- User is authenticated but lacks permission.

---

# 16. Difference between 200 and 201?

**200 OK**

- Successful request.

**201 Created**

- Resource successfully created.

---

# 17. Difference between 404 and 400?

**400**  
Bad request syntax or invalid input.

**404**  
Requested resource does not exist.

---

# 18. What is Authentication?

Authentication verifies **who the user is**.

Examples

- Password
- JWT
- OAuth
- API Key

---

# 19. What is Authorization?

Authorization determines **what the user is allowed to do** after authentication.

---

# 20. Difference between Authentication and Authorization?

Authentication

> Who are you?

Authorization

> What can you do?

---

# 21. What is an API Key?

A unique key sent with requests to identify the client application.

Example

```
x-api-key: abcd1234
```

---

# 22. What is JWT?

JWT stands for

**JSON Web Token**

It is a token used for stateless authentication.

---

# 23. JWT Structure

```
Header
.
Payload
.
Signature
```

---

# 24. What is Bearer Token?

A token sent in the Authorization header.

```
Authorization: Bearer eyJhbGc...
```

---

# 25. What is OAuth?

OAuth allows users to log in using another service without sharing passwords.

Examples

- Login with Google
- Login with Facebook
- Login with GitHub

---

# 26. What is API Versioning?

Managing multiple versions of an API.

Examples

```
/api/v1/users

/api/v2/users
```

---

# 27. Why API Versioning?

- Backward compatibility
- Add new features
- Avoid breaking existing clients

---

# 28. What is Idempotency?

An operation is **idempotent** if performing it multiple times has the same effect as performing it once.

Examples:

- GET → Idempotent
- PUT → Idempotent
- DELETE → Idempotent (deleting an already deleted resource still results in the resource being absent)

POST is generally **not** idempotent.

---

# 29. Which HTTP methods are idempotent?

- GET
- PUT
- DELETE
- HEAD
- OPTIONS

POST is usually not.

---

# 30. What is CORS?

CORS stands for

**Cross-Origin Resource Sharing**

It allows or restricts requests from different origins (domain, protocol, or port).

---

# 31. What causes a CORS error?

A browser blocks a cross-origin request because the server doesn't allow it through the appropriate CORS headers.

---

# 32. What is Rate Limiting?

Restricting the number of requests a client can make in a given time.

Example

```
100 requests/minute
```

---

# 33. Why use Rate Limiting?

- Prevent abuse
- Prevent DDoS
- Reduce server load

---

# 34. What is Pagination?

Returning data in smaller chunks instead of all at once.

Example

```
GET /users?page=2
```

---

# 35. Why use Pagination?

- Faster responses
- Lower memory usage
- Better user experience

---

# 36. What is API Documentation?

Documentation that explains how to use an API, including endpoints, parameters, request/response formats, and authentication.

Common tools:

- Swagger (OpenAPI)
- Postman

---

# 37. What is Postman?

A tool used to test and debug APIs by sending HTTP requests and inspecting responses.

---

# 38. What is Swagger/OpenAPI?

A standard for documenting REST APIs that provides interactive documentation and can generate client/server code.

---

# 39. What is Content-Type?

The `Content-Type` header tells the server what format the request body is in.

Example:

```
Content-Type: application/json
```

---

# 40. What is the Accept header?

The `Accept` header tells the server which response format the client prefers.

Example:

```
Accept: application/json
```

---

# 41. Difference between Query Parameters and Path Parameters?

**Path Parameter**

```
GET /users/10
```

`10` identifies a specific user.

**Query Parameter**

```
GET /users?page=2&sort=name
```

Used for filtering, sorting, or pagination.

---

# 42. What is the difference between REST and SOAP?

|REST|SOAP|
|---|---|
|Uses HTTP|Can use multiple protocols|
|Usually JSON|XML only|
|Lightweight|Heavier|
|Faster|Slower|
|Easier to use|More complex|

---

# 43. What is the difference between REST and GraphQL?

|REST|GraphQL|
|---|---|
|Multiple endpoints|Single endpoint|
|Fixed response structure|Client chooses fields|
|May over-fetch data|Reduces over-fetching|

---

# 44. What is an HTTP Header?

Headers carry additional information about the request or response.

Examples:

```
Authorization
Content-Type
Accept
User-Agent
```