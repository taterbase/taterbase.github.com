---
layout: post
title: How to Enable a MongoDB Replica Set on Travis
---
[Travis](https://travis-ci.org) is a fantastic resource for developers building 
open source projects since they provide free build servers to run your tests automatically.
Every time you push new code to your repos on [Github](https://github.com) Travis can pull down
the changes, run the tests, and update you when they fail. It also works with just about every 
imaginable language and provides quite a few databases out of the box. 

When setting your project up for Travis a basic config can often work pretty well but sometimes you 
need something a little more custom. I like to use Travis for most of my projects so while I was building 
[oplog-transform-tail](http://npm.im/oplog-transform-tail) (a node module that keeps your MongoDB data in sync with a secondary data store in real time)
I went ahead and created a somewhat standard Travis config file for a node project using MongoDB. 

That looks something like this:

```yaml
#.travis.yml
language: node_js
node_js:
  - "0.11"
  - "0.10"
services:
  - mongodb
```

This config just says that we're using node, we want to test on version `0.10` and `0.11`, 
and under `services` we say we want access to a MongoDB database. The problem with this
setup is [oplog-transform-tail](http://npm.im/oplog-transform-tail) requires access to
[MongoDB's oplog](http://docs.mongodb.org/manual/core/replica-set-oplog/). By default
MongoDB starts as a single server with **no** replica set and as such does not utilize the oplog.

To turn this feature on we just need to guide Travis a little bit and tell it to set our MongoDB
up as a [replica set](http://docs.mongodb.org/manual/core/replication/). Luckily Travis has a few [lifecycle entrypoints](http://docs.travis-ci.com/user/build-lifecycle/) that we can
take advantage of to do just that. Our use case just needs to happen before the `script` event (when the tests get run) 
so we'll use the `before_script` hook to make the changes ensuring our database is set up properly for testing.

Here's our new config for Travis:

```yaml
#.travis.yml
language: node_js
node_js:
  - "0.11"
  - "0.10"
services:
  - mongodb
# make mongodb a replica set
before_script:
  - echo "replSet = myReplSetName" | sudo tee -a /etc/mongodb.conf
  - sudo service mongodb restart
  - sleep 20
  - mongo --eval 'rs.initiate()'
  - sleep 15
```

Let's talk about what this does.

1. `echo "replSet = myReplSetName" | sudo tee -a /etc/mongodb.conf`
Here we append a string to the MongoDB config file, telling it set the `replSet` name to myReplSetName. This is all that's required for basic replica set initialization.

2. `sudo service mongodb restart`
Now we restart MongoDB so it can use the new config

3. `sleep 20`
Sometimes the database can take a while to reboot, we sleep 20 seconds here to make sure it's ready for the next step

4. `mongo --eval 'rs.initiate()'`
Here we tell MongoDB to evaluate our command, in this case `rs.initiate()` which tells MongoDB to start up the replica set functionality.

5. `sleep 15`
Here we sleep for another 15 seconds to give MongoDB a chance to properly set up the replica set.

And there you have it, now the MongoDB instance is running as a single server replica set and will write operations to the oplog allowing the tests a chance to run properly.
