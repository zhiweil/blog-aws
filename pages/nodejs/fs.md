---
title: File System Module
tags: [NodeJS]
keywords: NodeJS
last_updated: July 16, 2019
summary: "NodeJS modules"
sidebar: mydoc_sidebar
permalink: nodejs_fs.html
folder: nodejs
---
## Create and Write
Write File function takes three parameters:
* path
* content to write to file
* callback 

```js
const fs = require('fs');

fs.writeFile("newfile.txt", "this is the content to be written to newfile", (err) => {
    if (err) {
        console.error("Cannot create newfile.txt: " + err);
    } else {
        console.info("file newfile.txt has been created successfully.");
    }
});
```

## Read
Read file function takes three parameters:
* path
* encoding, read as binary if this is ignored.
* callback

```js
const fs = require('fs');

fs.readFile("newfile.txt", "utf8", (err, file) => {
    if (err) {
        console.log(err);
    } else {
        console.log(file);
    }
})
```

## Rename
Rename file function takes three arguments. 
* path
* new file name
* callback

```js
const fs = require('fs');

fs.rename("newfile.txt", "newfile1.txt", (err) => {
    if (err) {
        console.log(err);
    } else {
        console.log("file has been renamed successfully.");
    }
})
```

## Append
Append file function takes three arguments.
* path
* content to be appened
* callback

```js
const fs = require('fs');

fs.appendFile("newfile1.txt", "\nthis is a new line appended to file", (err) => {
    if (err) {
        console.log(err);
    } else {
        console.log("file has been appended successfully.");
    }
})
```

## Delete
Delete file function takes two arguments.
* path
* callback

```js
const fs = require('fs');

fs.unlink("newfile1.txt", (err) => {
    if (err) {
        console.log(err);
    } else {
        console.log("file has been deleted successfully.");
    }
})
```

## Create folder
Create folder function takes two arguments.
* path
* callback

```js
const fs = require('fs');

fs.mkdir("tutorial", (err) => {
    if (err) {
        console.log(err);
    } else {
        console.log("folder has been created successfully.");
    }
})
```

## Delete folder
Delete folder function takes two arguments.
* path
* callback

**NOTE: this function will fail if the folder is not empty.**

```js
const fs = require('fs');

fs.rmdir("tutorial", (err) => {
    if (err) {
        console.log(err);
    } else {
        console.log("folder has been deleted successfully.");
    }
})
```

## Read folder
Read folder takes two arguments.
* path
* callback

```js
const fs = require('fs');

fs.readdir("./tutorial", (err, files) => {
    if (err) {
        console.log(err);
    } else {
        for (let file of files) {
            fs.unlink("./tutorial/" + file, (err) => {
                if (err) {
                    console.log(err);
                } else {
                    console.log("file " + file + " has been deleted.");
                }
            });
        }
    }
})
```

## Readable and writable stream
While read or write large files, the read file function might fail due to insufficient buffer size, 
readable and writable streams allow to read and write file in chunks, which is much more memory efficient. 

```js
const fs = require('fs');

const readStream = fs.createReadStream("newfile.txt", "utf8");
const writeStream = fs.createWriteStream("copy.txt");

readStream.on("data", (chunk) => {
        writeStream.write(chunk);
    });
```

## Pipe
Streams can be chained to achieve complex operations. 

```js
const fs = require('fs');
const zlib = require('zlib');

const readStream = fs.createReadStream("newfile.txt", "utf8");
const writeStream = fs.createWriteStream("copy.txt.gz");
const gzip = zlib.createGzip();

readStream.pipe(gzip).pipe(writeStream)
```

Or the operations above can be reversed by:
```js
const fs = require('fs');
const zlib = require('zlib');

const readStream = fs.createReadStream("copy.txt.gz");
const writeStream = fs.createWriteStream("copy1.txt");
const gunzip = zlib.createGunzip();

readStream.pipe(gunzip).pipe(writeStream)
```