## aframe-broadcast-component

> Work in progress. Supports A-Frame 0.2.0.

> Check out the successor to this component, the [A-Frame Firebase Component](https://github.com/ngokevin/aframe-firebase-component).

A component to send and consume entity data over WebSockets for simple
multiuser [A-Frame](https://aframe.io).

The provided server simply relays all broadcasted data through WebSockets to
the rest of the clients.

When the `broadcast` component is attached to an entity, it will emit all
specified component data, the entity ID, and the parent's ID to the WebSocket
server once every 10ms (will be adjustable later).

When another client receives that data, it uses it to create an element with
the ID if it doesn't exist, and then sync the component data to the entity with
`setAttribute`.

This is primarily used for simply seeing other users walk and look around in
the same scene. It probably would not work well with game logic or physics.
Needs more experimenting.

### Properties

| Property | Description                                          | Default Value          |
| -------- | -----------                                          | -------------          |
| send     | List of comma-delimited component names to broadcast | position, rotation     |
| url      | WebSocket server URL                                 | http://localhost:12000 |

### Usage

#### Server

There is a simple Node `socket.io` server in `server/`. It's about 10 LOC.

```
PORT=12000 node index.js
```

If you are serving the client over HTTPS, you will need SSL on your webserver.
I just serve the client over HTTP and avoid that need.

#### Browser Installation

Install and use by directly including the [browser files](dist):

```html
<head>
  <title>My A-Frame Scene</title>
  <script src="https://aframe.io/releases/0.9.0/aframe.min.js"></script>
  <script src="https://rawgit.com/ngokevin/aframe-broadcast-component/master/dist/aframe-broadcast-component.min.js"></script>
</head>

<body>
  <a-scene broadcast="http://myserver.com:12000">
    <a-entity broadcast="send: geometry, material, position, rotation"
              camera look-controls wasd-controls
              geometry="primitive: box"
              material="color: #222"
              position="0 1.8 5"></a-entity>
  </a-scene>
</body>
```

#### NPM Installation

Install via NPM:

```bash
npm install aframe-broadcast-component
```

Then register and use.

```js
require('aframe');
require('aframe-broadcast-component');
```
