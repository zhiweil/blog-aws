---
title: Package Management
tags: [NodeJS]
keywords: NodeJS
last_updated: July 17, 2019
summary: "NodeJS package management"
sidebar: mydoc_sidebar
permalink: nodejs_package.html
folder: nodejs
---

## package.json
By running the following command, package.json is generated.
```bash
npm init --yes
```

```json
{
  "name": "nodejs",
  "version": "1.0.0",
  "description": "Tutorials for Node JS projects",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Zhiwei Liu",
  "license": "ISC"
}
```

## Install package
```bash
npm install lodash
```
The command above create a new folder "node_modules" under the project root, which holds all 
the dependencies of the project.

The package.json is updated with installed module:

```json
{
  "name": "nodejs",
  "version": "1.0.0",
  "description": "Tutorials for Node JS projects",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Zhiwei Liu",
  "license": "ISC",
  "dependencies": {
    "lodash": "^4.17.14"
  }
}
```

To remove a module from project:
```bash
npm uninstall lodash
```

## Semantic versioning
* patch - bug fixes
* minor - minor updates, should be non-broken
* major - feature release, might contain broken changes
* ^ - this carent character means that major version should not be udpated, but if there is a new version of minor or patch, then apply it automatically.
* ~ - this tilda character means that only the patch updates are applied automatically.
* without any special character - this means that the exact version is applied.