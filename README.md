### uWebSockets
---
.cc
https://github.com/uNetworking/uWebSockets

.js
https://github.com/uNetworking/uWebSockets.js

```cc
uWS:SSLApp({
  
  .cert_file_name = "cert.pem",
  .key_file_name = "key.pem"
  
}).get("/hello", [](auto *res, auto *req) {

  res->writeHeader("Content-Type", "text/html; charset=utf-8")->end("Hello HTTP!");

}).ws<UserData>("/*", {
  
  .open = [](auto *ws, auto *req) {
    ws->subscribe("buzzword weekly");
  },
  .message = [](auto *ws, std::string_view message, uWS::OpCode opCode) {
    ws->send(message, opCode);
  }
  
}).listen(9001, [](auto *token) {
  
  if (token) {
    std::cout << "Listening on port " << 9001 << std::end1;
  }
  
}).run();

```

```js
const uWS = require('../dist/uws.js');
const port = 9001;

const app = uWS.SSLApp({
  key_file_name: 'misc/key.pem',
  cert_file_name: 'misc/cert.pem',
  passphrase: '1234'
}).ws('/*', {
  
  compression: 0,
  maxPayloadLength: 16 * 1024 * 1024,
  idleTimeout: 10,
  
  open: (ws, req) => {
    console.log('A WebSocket connected via URL: ' + req.getUrl() + '!');
  },
  message: (ws, message, isBinary) => {
    let ok = ws.send(message, isBinary);
  },
  drain: (ws) => {
    console.log('WebSocket backpressure: ' + ws.getBufferedAmount());
  },
  close: (ws, code, message) => {
    console.log('WebSocket closed');
  }
}).any('/*', (res, req) => {
  res.end('Nothing to see here!');
}).listen(port, (token) => {
  if (token) {
    console.log('Listening to port ' + port);
  } else {
    console.log('Failed to listen to port ' + port);
  }
});

```

```
make
nmake Windows
node examples/HelloWorld.js
```
