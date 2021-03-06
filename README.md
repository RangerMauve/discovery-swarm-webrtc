# discovery-swarm-webrtc
webrtc-swarm but with a similar API to discovery-swarm

This module provide a `stream` option to replicate across peers and a `join` method to connect to a channel.

```
$ npm install @geut/discovery-swarm-webrtc
```

## Usage

```javascript
const swarm = require('@geut/discovery-swarm-webrtc')

const sw = swarm({
  id: 'id',
  stream: () => feed.replicate()
})

sw.join(signalhub('channel-id', ['http://yourhub.com']))

sw.on('connection', peer => {
  // connected
})
```

## API

#### `const sw = swarm(opts)`

Creates a new Swarm. Options include:

```javascript
{
  id: cuid(), // peer-id for user
  stream: stream, // stream to replicate across peers
}
```

#### `sw.join(hub, [opts])`

Join a channel specified by [hub](https://github.com/mafintosh/signalhub) instance.

The options are for the `webrtc-swarm` instance.

### Events

#### `sw.on('handshaking', function(connection, info) { ... })`

Emitted when you've connected to a peer and are now initializing the connection's session. Info is an object that contains info about the connection.

``` js
{
  id // the remote peer's peer-id.
}
```

#### `sw.on('connection', function(connection, info) { ... })`

Emitted when you have fully connected to another peer. Info is an object that contains info about the connection.

#### `sw.on('connection-closed', function(connection, info) { ... })`

Emitted when you've disconnected from a peer. Info is an object that contains info about the connection.

#### `sw.on('redundant-connection', function(connection, info) { ... })`

Emitted when multiple connections are detected with a peer, and so one is going to be dropped (the `connection` given). Info is an object that contains info about the connection.
