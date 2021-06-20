---
layout: default
title: SignalR
has_children: true
permalink: docs/signalr
---

# SignalR
 <https://www.npmjs.com/package/@microsoft/signalr>

## Installation

```ts
npm install @microsoft/signalr
# or
yarn add @microsoft/signalr
```

## Usage
### Create a connection

```ts
// create the connection instance
 let connection = new signalR.HubConnectionBuilder()
    .withUrl("https://example.com/signalr/chat")
    .build();
```
- Starts the connection.
```ts
connection.start();
```
- Stops the connection.
```ts
connection.stop();
```
### Call hub methods from client
Using ie the javascript client I can either use
```ts
connection.invoke("SendMessage", user, message)
```
or
```ts
connection.send("Send", message);
```
You need to read the source code to see the difference between send and invoke.
- `Send` returns a promise that is resolved when the client has sent the invocation to the server, or an error occurred. The server may still be handling the invocation when the promise resolves.
- `Invoke` returns a promise that is resolved when the server has finished invoking the method (or an error occurred). In addition, the `Invoke` promise can receive a result from the server method, if the server returns a result.

The code can be found [here](https://github.com/aspnet/SignalR/blob/7e832eeb27b25be51dade7ccfe557af6c8d98cfa/clients/ts/signalr/src/HubConnection.ts#L187)

### Call client methods from hub
> As a best practice, call the **start** method on the  `HubConnection` after `on`. Doing so ensures your handlers are registered before any messages are received.

To receive messages from the hub, define a method using the `on` method of the [HubConnection](https://docs.microsoft.com/en-us/javascript/api/@aspnet/signalr/hubconnection?view=signalr-js-latest).
- The name of the JavaScript client method. In the following example, the method name is `ReceiveMessage`.
- Arguments the hub passes to the method. In the following example, the argument value is `message`.
```ts
connection.on("ReceiveMessage", (user, message) => {
    // do something
});
```
### Error handling and logging.
```ts
connection.start().catch(err => console.error(err));
```
Set up client-side log tracing by passing a logger and type of event to log when the connection is made. Messages are logged with the specified log level and higher. Available log levels are as follows:
- `signalR.LogLevel.Error`: Error messages. Logs `Error` messages only.
- `signalR.LogLevel.Warning`: Warning messages about potential errors. Logs `Warning`, and `Error` messages.
- `signalR.LogLevel.Information`: Status messages without errors. Logs `Information`, `Warning`, and `Error` messages.
- `signalR.LogLevel.Trace`: Trace messages. Logs everything, including data transported between hub and client.

Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level. Messages are logged to the browser console.
### Reconnect clients
After to 3.0, the JavaScript client for SignalR automatically reconnect with using `withAutomaticReconnect`. You don't must write code that will reconnect your client manually.

## For example
1. Create a connection
```ts
// Connect, using the token we got.
  let connection = new signalR.HubConnectionBuilder()
    .withUrl(connectionHub, options),
      {
        transport:
          signalR.HttpTransportType.WebSockets ||
          signalR.HttpTransportType.LongPolling,
        accessTokenFactory: () => globalState.token,
      },
    )
    .configureLogging(signalR.LogLevel.Warning)
    .withAutomaticReconnect() //will automatically try to reconnect
    .build();
```

## Some common mistakes
