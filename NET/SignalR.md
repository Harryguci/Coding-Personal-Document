# SignalR - Introduction

## What is SignalR

ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.

SignalR provide a simple API for creating server-to-client **remote procedure calls** (RPC) that call Javascript functions in client browsers (and other client platforms) from server-side.
SignalR also includes API for connection management (for instance, connect and disconnect events), and grouping connections.

[![SignalR](https://learn.microsoft.com/en-us/aspnet/signalr/overview/getting-started/introduction-to-signalr/_static/image1.png)]

## SignalR and WebSocket

SignalR using the new websocket transport where available and falls back to older transports where necessary.

## Transport and Fall back

- SignalR is an abstraction over some of the transports that are required to do real-time work between server and client. SignalR first attempts to establish a Websocket connection if possible. Websocket is the optimal transport for SignalR because it has:
  - The most efficient use of server memory
  - The lowest latency
  - The most underlying features, such as full duplex communications between client and server.
  - The most stringer requirements
- If these requirements are not met, SignalR attempts to use other transports to make its connections

## HTML5 transports

- These transports depend on support for HTML5. If the client browser does not support the HTML5 standard, older transport will be used
  - **WebSocket** (if both the server and client browser indicate they can support Websocket).
  - **Server Sent Events**

## Comet transport

- The following transports are base on the comet web application model, in which a browser or other client maintains a long-held HTTP request, which the server can use to push data to the client without the client specifically requesting it.
  - **Forever Frame** (only Internet Explorer)
  - **Ajax long polling**, Long polling is not create a persistent connection, but instead polls server with a request that says open util server responds, at which point the connection closes, and a new connection is requested immediately. That may introduce some latency while the connection resets.

## Transport Selection Process

Steps

1. If the browser is **Internet Explorer 8** or earlier, Long Polling is used
2. If JSONP is configured (that is, the `jsonp` was set to `true` when the connection is started), Long Polling is used.
3. If the cross-domain connection is being made (that is, if the SignalR endpoint is not in the same domain as the hosting page), then the WebSocket will be used if the following criteria are met
   - The client browser supports CORS
   - The client browser supports WebSocket
   - The server supports WebSocket
   - If any of these criteria are not met, Long Polling is used.
4. JSONP is not configured and the client browser and server support WebSocket, WebSocket is used.
5. If either the client browser or the server are not support WebSocket, **Server Sent Events** is used
6. If Server Sent Events is not available, **Forever Frame** is attempted
7. If Forever Frame fails, Long Polling is used

## Monitoring Transport

## Specifying a Transport

```
connection.start({ transport: 'longPolling' });
```

```
connection.start({ transport: ['webSockets','longPolling'] });
```

The string constants for specifying transport are defined as follows:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## Connections and Hubs

- The SignalR API contains two models for communication between the client and server: **Persistent Connection** and **Hubs**
- A connection represents a simple endpoint for sending to single-recipient, grouped, or board-cast messages. The Persistent Connection API (PersistentConnection class in .NET) gives the developer direct access to the low-level communication protocol that SignalR exposes. Using the connections communication model will be familiar to developers who used connection-based APIs such as Windows Communication Foundation
- A Hub is a more high-level pipeline built upon the connection API that allows your client and server to call methods on each other directly.

## Architecture Diagram

- The following diagram shows the relationship between Persistent Connections, Hubs and the underlying technologies used for transport.

[![Architecture Diagram](https://learn.microsoft.com/en-us/aspnet/signalr/overview/getting-started/introduction-to-signalr/_static/image5.png)]

## How Hub Work

- When the server-side code calls a method on the client, a packet is sent across the active transport that contains the name and parameters of the method to be called (when an object is sent as a method parameter, this object is serialized using JSON). The client then matches the method name to methods defined in client-side code. If there is a match, the client method will be executed using the deserialized parameter data.
- The method call can be monitored by using tools like Fiddler. Example: The method call is sent from a Hub called `MoveShapeHub`, and the method being invoked is called `updateShape`

[![Fiddler Logs](https://learn.microsoft.com/en-us/aspnet/signalr/overview/getting-started/introduction-to-signalr/_static/image6.png)]
