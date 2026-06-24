# Socket vs WebSocket (Networking Notes)

---

# 1. Socket

## Definition

A **socket** is a communication endpoint that allows two applications to exchange data over a network.

A socket is created by the operating system and used by applications for networking.

---

## Socket Structure

A socket is identified by:

```
IP Address + Port Number + Protocol
```

Example:

```
192.168.1.10 : 5000 TCP
```

---

# Socket Types

## 1. TCP Socket

```
SOCK_STREAM
```

Features:

- Connection-oriented
- Reliable
- Ordered data delivery
- Error checking

Example:

```
Client Socket      |      |   TCP Connection      |      |Server Socket
```

Used for:

- HTTP
- HTTPS
- SSH
- FTP

---

## 2. UDP Socket

```
SOCK_DGRAM
```

Features:

- Connectionless
- Faster
- No guarantee of delivery
- Lower latency

Used for:

- DNS
- Online games
- Video calls

---

# Socket Communication Flow

TCP socket:

```
Server:socket()   ↓bind()   ↓listen()   ↓accept()   ↓send()/recv()Client:socket()   ↓connect()   ↓send()/recv()
```

---

# 2. WebSocket

## Definition

A **WebSocket** is a communication protocol that provides a **persistent, two-way communication channel** between client and server.

It is mainly used for real-time applications.

---

# WebSocket Working

First connection:

```
Client  | HTTP Request  |Server
```

Then upgrade:

```
HTTP Connection        |        ↓WebSocket Connection
```

After upgrade:

```
Client  <================> Server(send anytime)(receive anytime)
```

---

# WebSocket Features

- Full duplex communication
- Persistent connection
- Real-time data transfer
- Low latency

---

# WebSocket Uses

Examples: this

## Chat applications

```
User A  |WebSocket  |Server  |WebSocket  |User B
```

---

## Live applications:

- Chat
- Notifications
- Online games
- Live sports score
- Stock price updates
- Collaborative editing

---

# Socket vs WebSocket Comparison

|Feature|Socket|WebSocket|
|---|---|---|
|Type|Programming interface|Communication protocol|
|Layer|Transport level API|Application layer|
|Uses TCP/UDP|Yes|Mostly TCP|
|Connection|Depends on implementation|Persistent|
|Communication|Usually request/response|Two-way anytime|
|Built for browser|No|Yes|
|Real-time|Possible|Designed for it|
|Data format|Raw bytes|Messages|
|Common use|Servers, networking apps|Chat, live apps|

---

# Relationship

Important:

```
Application      |      ↓WebSocket      |      ↓TCP Socket      |      ↓IP      |      ↓Network
```

WebSocket uses sockets underneath.

---

# Example Difference

## Normal Socket

```
Client:connect()send datareceive dataclose()
```

Connection ends.

---

## WebSocket

```
Client:connect()Connection stays opensend messagereceive messagesend messagereceive messageclose()
```

---

# Code Example

## TCP Socket (Python)

Server:

```
import socketserver = socket.socket()server.bind(("localhost",5000))server.listen()client,address = server.accept()client.send(b"Hello")
```

---

## WebSocket (Python)

```
import websocketsasync def server(ws):    await ws.send("Hello")websockets.serve(server,"localhost",5000)
```

---

# Memory Trick

```
Socket = RoadTCP/UDP = Vehicle typeWebSocket = Special vehicle designed for continuous communication
```

---

# Final Summary

- **Socket** → general network communication endpoint
- **TCP/UDP socket** → low-level communication using transport protocols
- **WebSocket** → real-time communication protocol built on TCP sockets
- WebSocket is mainly used when server needs to push data instantly to clients

Example:

```
Browser + WebSocket → Chat appServer + TCP Socket → WebSocket server internally
```