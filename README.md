# Node.js Compatibility
There is some issues with the code from Peter, and with the new improvements on node.js 0.6 this lib __don't work__ correctly.Work fine only with node.js 0.4

Please be sure of use node.js 0.4 (recommended 0.4.12) before use this lib.

## hook.io-ws
Hook listening messages on *WebSocket* client connection.

Based on [Peter Griess](https://github.com/pgriess/node-websocket-client) work. [WebSocket Nodejs Client](https://github.com/pgriess/node-websocket-client)

## Contributors
 - [Marak Squires](http://github.com/marak)

## Requirements
 - [nodejs](http://nodejs.org)
 - [npm](http://npmjs.org)
 - [hook.io](http://hook.io)


## Installation
    npm install hook.io-ws

## Usage
    hookio-ws

## Development Example
Now you can require the `hook.io-ws` module and instance a new Hook that try to connect to `url` using *WebSockets*

```javascript

#!/usr/bin/env node

var WebSocketHook = require('hook.io-ws').HookSocket;

var mtgoxTest = new WebSocketHook({
  name: 'mtgox',
  url:'ws://websocket.mtgox.com/mtgox',
  debug:false
});

mtgoxTest.start();

```

Or you can use the Hook.io Magic

```javascript

/*
 * Hook class for interacting with mtgox.com
 */
var Hook = require('hook.io').Hook,
    util = require('util');
    
var MtgoxHook = exports.MtgoxHook = function(options){
  Hook.call(this, options);
  var self = this;
  self.on('hook::ready', function(){
    /// Spawn a new hook as type have "ws"
    self.spawn([{ 
      "name": "mtgox-ws-hook",
      "type": "ws", // Doing this. Hook.io will look for hook.io-ws 
      "debug": "false",
      "url": "ws://websocket.mtgox.com/mtgox"
    }], function(err){
      // spawn ready
      self.on('*::websocket::message',function(message){
        
        if(message.op === 'subscribe') {
          self.emit('subscribe', message);
        } else {
          self.emit(message.private, message);
        }
        
      });
    });
  });
};
```

## Author
 - [Daniel Aristizabal](http://github.com/cronopio)
