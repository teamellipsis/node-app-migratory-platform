---
layout: doc
title: Migratory application
description:  
---

### Migratory application
1. [Application development](#application-development)
    1. [Getting started](#getting-started)
    1. [Skeleton](#skeleton)
    1. [Examples](#examples)
    1. [Build](#build)
1. [API reference](#api-reference)

## Application development
### Getting started
***
Migratory application can be developed using well known web technologies such as [React.js](https://reactjs.org/), [Redux](https://redux.js.org/), [React Redux](https://react-redux.js.org/), [Node.js](https://nodejs.org/en/).

> Prerequisites: [Node.js 8.x LTS](https://nodejs.org/docs/latest-v8.x/api/)
> 
> Note: [Application Migratory Platform](https://teamellipsis.github.io/node-app-migratory-platform) only supports Node.js 8.x, if `Migaratory application` developed on higher than Node.js 8.x, then it will cause to crash the `Migratory application` at runtime.

Clone the minimal migratory app
```bash
$ git clone https://github.com/teamellipsis/migratory-skeleton-app
$ cd migratory-skeleton-app

# Or clone to different directory name
$ git clone https://github.com/teamellipsis/migratory-skeleton-app <your-project-name>
$ cd <your-project-name>
```

Change remote (optional)
```bash
# Remove current origin
$ git remote remove origin
# Add your project repository as origin
$ git remote add origin <your-project-repo-link>
# Check origin
$ git remote -v
```

Run in development
```bash
$ npm install
$ npm run dev-server
# Open in browser http://localhost:3000
```

If port 3000 busy(Address already in use) run in different port as below (optional)
```bash
# On UNIX
$ PORT=3001 npm run dev-server
# On Windows
$ set PORT=3001 && npm run dev-server
```
See [Skeleton](#skeleton) and [Examples](#examples) for more development.

### Skeleton
***
Skeleton app consist two main application states
- [UI state](#ui-state)
- [Daemon state](#daemon-state)

#### UI state
UI state preserved by using [`react-redux`](https://react-redux.js.org/).

#### Daemon state
Daemon state preserved by using [Node.js `vm` module](https://nodejs.org/docs/latest-v8.x/api/vm.html).

### Examples
***
#### [Demo Todo Application](https://github.com/teamellipsis/demo-migratory-redux-app)
This example application provides only UI state preserving with [`react-redux`](https://react-redux.js.org/). Not provides Daemon state preserving. But UI can call Daemon to get native access.

#### [Demo Chess Game](https://github.com/teamellipsis/migratory-chess-app)
This chess game developed only using Daemon state preserving. But UI state preserving also posible. 

### Build
***
> Note: Make sure remove previous `build` directory before create new build.
```bash
# Without specifing app name. This will use package.json -> name as the app name.
$ npm run release
# Or specify app name as first argument.
$ npm run release -- "App Name"
```
Build file will put into `build` directory.

## API reference
***

[Back](./)
