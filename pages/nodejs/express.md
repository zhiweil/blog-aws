---
title: Express Framework
tags: [NodeJS]
keywords: NodeJS
last_updated: July 17, 2019
summary: "NodeJS Express framework"
sidebar: mydoc_sidebar
permalink: nodejs_express.html
folder: nodejs
---

## Setup
```bash
npm install express
```

```js
const express = require('express');
const app = express();

app.get("/", (req, res)=> {
    res.send("Hello");
});

app.listen(3000);
```

## GET
Use parameters for compulsory fields, and use query strings for optional ones.

### Parameters
```js
const express = require('express');
const app = express();

app.get("/", (req, res)=> {
    res.send("Hello");
});

app.get("/example/:name/:age", (req, res) => {
    res.send(req.params.name + ": " + req.params.age);
});

app.listen(3000);
```

### Query
```js
const express = require('express');
const app = express();

app.get("/", (req, res)=> {
    res.send("Hello");
});

app.get("/example", (req, res) => {
    res.send(req.query.name + ": " + req.query.age);
});

app.listen(3000);
```

### Serve static files
* app.use() create a middleware to serve static resources from specified folder.
* sendFile() function serves static resources.
```js
const express = require('express');
const path = require('path');
const app = express();

app.use("/public", express.static(path.join(__dirname, "static")));
app.get("/", (req, res)=> {
    res.sendFile(path.join(__dirname, "static", "index.html"));
});

app.listen(3000);
```

## POST
Body parser will parse URL encoded POST body and put fields under Request.body
```bash
npm install body-parser
```

```js
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const app = express();

app.use("/public", express.static(path.join(__dirname, "static")));
app.use(bodyParser.urlencoded({extended: false}));
app.post("/", (req, res)=> {
    console.info(req.body);
    res.send("Successfully parsed request body.");
});

app.listen(3000);
```

## JSON
For JSON request body, should use JSON body parser. 

```js
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const app = express();

app.use("/public", express.static(path.join(__dirname, "static")));
app.use(bodyParser.json());
app.post("/", (req, res)=> {
    // now the req.body is a JSON object
    console.info(req.body);
    res.json({success: true});
});

app.listen(3000);
```

## Input validation by joi
```bash
npm install joi
```

The joi module can be used to validate post input.

```js
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const joi = require("joi");
const app = express();

app.use("/public", express.static(path.join(__dirname, "static")));
app.use(bodyParser.json());
app.post("/", (req, res)=> {
    console.info(req.body);
    const schema = joi.object().keys({
        email: joi.string().trim().email().required(),
        password: joi.string().min(5).max(10).required()
    });

    joi.validate(req.body, schema, (err, result) => {
        if (err) {
            res.send(err);
        } else {
            res.send("post validation passed.")
        }
    });
});

app.listen(3000);
```

## EJS tempalte
**EJS - Embedded Javascript Template**

Create a folder "views" under the projet root, this folder holds all the HTML templates for Express. 
```bash
npm install ejs
```

NodeJS code:
```js
const express = require('express');
const ejs = require("ejs");
const app = express();

app.set("view engine", "ejs");
app.get("/:userQuery", (req, res)=> {
    // the file extension ejs is not needed here
    res.render("index", {data: {userQuery: req.params.userQuery,
        searchResults: ["book1", "book2", "book3"]}});
});

app.listen(3000);
```

EJS template:

* The variables between "<%" and "%>" will be evaluated by EJS and express.
* The = sign is used to output an variable.


```html
<html>
    <body>
        <h1>Home page</h1>
        <h2><%= data.userQuery %></h2>
        <ul>
            <% data.searchResults.forEach(result => { %>
                <li><%= result %></li>
            <% }); %>
        </ul>
    </body>
</html>
```

## Middleware
The the call to app.user() call does not specify path argument, the middleware is applied to all express HTTP handlers.

```js
const express = require('express');
const ejs = require("ejs");
const app = express();

app.use("/someurl", (req, res, next) => {
    console.info(req.url, req.method);
    next();
});
app.set("view engine", "ejs");
app.get("/:userQuery", (req, res)=> {
    // the file extension ejs is not needed here
    res.render("index", {data: {userQuery: req.params.userQuery,
        searchResults: ["book1", "book2", "book3"]}});
});

app.listen(3000);
```

## Router
Create a folder "routes" under project root. The folder holds routes for express.

In app.js

```js

```

In routes/people.js

```js
const express = require('express');
const route = express.Router();

route.get("/", (req, res) => {
    console.info("/ being hit");
});

module.exports = route;
```