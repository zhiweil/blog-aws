---
title: HTTP Module
tags: [NodeJS]
keywords: NodeJS
last_updated: July 17, 2019
summary: "NodeJS HTTP module"
sidebar: mydoc_sidebar
permalink: nodejs_http.html
folder: nodejs
---
## Create Server
```js
const http = require('http');

const server = http.createServer((req, res) => {
    if (req.url === "/") {
        res.write("Hello");
        res.end();
    } else {
        res.write("access from non-root url");
        res.end();
    }
});

server.listen('3000');
```

## Serve static files
### HTML
```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    var writeStream = fs.createReadStream("./static/index.html");
    res.writeHead(200, {'Content-Type': 'text/html'});
    writeStream.pipe(res);
}).listen(3000);
```

### Image
```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    var writeStream = fs.createReadStream("./static/index.png");
    res.writeHead(200, {'Content-Type': 'image/png'});
    writeStream.pipe(res);
}).listen(3000);
```

### JSON
```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    var writeStream = fs.createReadStream("./static/index.json");
    res.writeHead(200, {'Content-Type': 'application/json'});
    writeStream.pipe(res);
}).listen(3000);
```