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
    1. [@Daemon](#@daemon)
    1. [Daemon state preserving](#daemon-state-preserving)
    1. [UI state preserving](#ui-state-preserving)


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

Run production (optional)
```bash
$ npm run build
# On UNIX
$ npm run server
# On Windows
$ npm run server-win
# Application will open at random port and URL will prints
```
See [Skeleton](#skeleton) and [Examples](#examples) for more development.

### Skeleton
***
[Migratory Skeleton Application](https://github.com/teamellipsis/migratory-skeleton-app) consist two main application states
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
### @Daemon
***
This decorator provides asynchronous mechanism (`Promise`) to call synchronous function defined in `daemon` (Node.js process).

#### Ex: 1

```js
// components/App.js

import React from 'react';
// Import `Daemon` is not necessary
import Daemon from 'Daemon';

class Example extends React.Component {
    constructor(props) {
        super(props);

        this.getCount().then((count) => {
            // count ...
        }).catch((err) => {
            // err ...
        });
    }

    handleCountChange = (num) => {
        this.setCount(num).then(() => {
            // ...
        }).catch((err) => {
            // err ...
        });
    };

    @Daemon()
    getCount() { }

    @Daemon()
    setCount(count) { }

}
```
```js
// daemon/index.js

this.getCount = () => {
    if (!this.count) this.count = 0; // Set default value for this.count
    return this.count;
};

this.setCount = (count) => {
    this.count = count;
};
```

### Daemon state preserving
***
#### Daemon function defining convention
Function should define with `this` as [Ex: 2](#ex:-2).

#### Ex: 2
```js
this.setCount = function (count) {
    this.count = count;
};
// Or
this.setCount = (count) => {
    this.count = count;
};
```

#### Daemon variable defining convention
It's not like normal variable definition, because if previous variable state already assigned by `Application Migratory Platform` then reassigning the variable will cause to lose variable previous state. Basically each variable has initial state and after that the varible has a value. According to [Ex: 3](#ex:-3) assigning defualt or initial value to a variable, need only once.

> Note: Preserving device dependent (hardware dependent such as IO) variable not supported. Recommended for primitive data types.

#### Ex: 3
```js
this.getCount = () => {
    if (!this.count) this.count = 0; // Set default value for this.count
    return this.count;
};
```

### UI state preserving
***
We use [`react-redux`](https://react-redux.js.org/) to UI state preserving. Migratory application create two `redux` stores. One for application and other one for platform. Application's `redux` stote creates at `pages/_app.js`. While creating application's `redux` store, `preloadedState` may inject if application is not fresh.
Redux actions are located at `actions` directory.
And reducers are located at `reducers` directory according to [Skeleton](#skeleton).
When exit from the application, the `redux` store will be saved.

More about Redux
- [actions](https://redux.js.org/basics/actions#actions)
- [reducers](https://redux.js.org/basics/reducers#reducers)
- [store](https://redux.js.org/basics/store#store)

> Note: `Application Migratory Platform` or `Migratory Application` will not handle any **mutation** and **asynchronicity** by default in any mentioned API above. Without handling them application may lead to race conditions. But `redux` itself handle **mutation** and **asynchronicity**.

[Back](./)
