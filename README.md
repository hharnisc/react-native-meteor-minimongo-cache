# Quick Start

NOTE: this is an experiment, subject to change.

This works with the Meteor `todos` example project.

I'm using a pure [JavaScript DDP](https://github.com/hharnisc/node-ddp-client) client to talk to Meteor (via [DDP](http://en.wikipedia.org/wiki/Distributed_Data_Protocol)).

**However** - instead of maintaining meteor collections with JavaScript objects it's using [@petehunt's](https://github.com/petehunt) [minimongo-cache](https://github.com/petehunt/minimongo-cache) to store them. This branch of DDPClient exposes minimongo-cache via `collections`

```javascript
var ddpClient = new DDPClient({url: 'ws://127.0.0.1:3000/websocket'});

ddpClient.collections // minimongo-cache instance
```

Here's an example using minimongo-cache

```javascript
var ddpClient = new DDPClient({url: 'ws://127.0.0.1:3000/websocket'});
var subComplete = () => {
  var todoItems = ddpClient.collections.observe(() => ddpClient.collections.lists.find({}));
  todoItems.subscribe((results) => this.updateRows(results));
}

ddpClient.connect(() => ddpClient.subscribe('publicLists', undefined, subComplete));
```

Here's the same functionality with the `master` branch of the node-ddp-client for reference

```javascript
var ddpClient = new DDPClient({url: 'ws://127.0.0.1:3000/websocket'});

ddpClient.connect(() => ddpClient.subscribe('publicLists'));

// observe the lists collection
var observer = ddpClient.observe("lists");
observer.added = () => this.updateRows(_.cloneDeep(_.values(ddpClient.collections.lists)));
observer.changed = () => this.updateRows(_.cloneDeep(_.values(ddpClient.collections.lists)));
observer.removed = () => this.updateRows(_.cloneDeep(_.values(ddpClient.collections.lists)));
```

If you'd like to start playing around with the `minimongo-cache` branch of the DDPClient take a look at the package.json file. It's pointed at the latest commit of that branch.

### Install NPM Modules

<https://docs.npmjs.com/getting-started/installing-node>

```
$ npm install
```
![Screen Shot](https://raw.githubusercontent.com/hharnisc/react-native-meteor/master/ScreenShot.png)
