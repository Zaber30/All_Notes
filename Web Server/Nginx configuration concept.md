# Nginx Configuration Structure

Nginx configuration is based on two main building blocks:

1. Context (Configuration Block)
2. Directive


---

# 1. Context (Configuration Block)

## Definition

A context is a section/block in Nginx configuration that groups related settings.

Syntax:

```nginx
context {

    directive value;

}
```

Example:

```nginx
http {

    server {

        location / {

        }

    }

}
```

Here:

- `http` → Context
- `server` → Context
- `location` → Context


## Common Nginx Contexts

| Context | Purpose |
|---|---|
| main | Global Nginx settings |
| events | Connection handling |
| http | Web server configuration |
| server | Website / virtual host |
| location | URL routing |
| upstream | Backend server group |


---

# 2. Directive

## Definition

A directive is an instruction that tells Nginx what to do.

Syntax:

```nginx
directive parameter;
```


Example:

```nginx
worker_processes 4;
```

Breakdown:

```
worker_processes → Directive
4                → Parameter
```


Examples:

```nginx
listen 80;

server_name example.com;

root /var/www/html;
```


---

# 3. Parameter / Argument

## Definition

A parameter is the value given to a directive.

Example:

```nginx
worker_connections 1024;
```

Breakdown:

```
worker_connections → Directive

1024                → Parameter
```


Example:

```nginx
server_name example.com;
```

Breakdown:

```
server_name → Directive

example.com → Parameter
```


---

# 4. Variable

## Definition

Variables are dynamic values provided by Nginx.

Example:

```nginx
proxy_set_header X-Real-IP $remote_addr;
```


Breakdown:

```
proxy_set_header → Directive

$remote_addr     → Variable
```


Common variables:

| Variable | Meaning |
|---|---|
| $host | Requested domain |
| $remote_addr | Client IP address |
| $request_uri | Requested URL |
| $server_name | Server name |


---

# 5. Module

## Definition

Modules provide Nginx functionality.

Structure:

```
Nginx

|
+-- HTTP Module
|
+-- SSL Module
|
+-- Proxy Module
|
+-- FastCGI Module
|
+-- Load Balancing Module
```


Example:

```nginx
proxy_pass http://backend;
```

`proxy_pass` comes from:

```
Proxy Module
```


---

# Nginx Configuration Hierarchy

```
Nginx Configuration

        |
        v

     Context

        |
        v

    Directives

        |
        v

Parameters / Variables
```


---

# Example Full Configuration

```nginx
http {

    server {

        listen 80;

        server_name example.com;


        location / {

            proxy_pass http://backend;

        }

    }

}
```


Explanation:

```
http
 |
 +-- Context


server
 |
 +-- Context


listen
 |
 +-- Directive


80
 |
 +-- Parameter


location
 |
 +-- Context


proxy_pass
 |
 +-- Directive


http://backend
 |
 +-- Parameter
```


---

# Quick Summary

| Item | Type | Example |
|---|---|---|
| events | Context | events { } |
| http | Context | http { } |
| server | Context | server { } |
| location | Context | location / { } |
| worker_processes | Directive | worker_processes 4; |
| listen | Directive | listen 80; |
| 80 | Parameter | listen 80; |
| $remote_addr | Variable | Client IP |
| SSL / Proxy | Module | Provides features |