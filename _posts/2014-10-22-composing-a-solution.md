---
layout: post
title: Composing Your Solution
---
In the past couple weeks I've been writing a few different modules and publishing them to [npm](https://npmjs.org). When I'm writing new code to solve a problem I often end up coding together some functionality and releasing a singular module however this time around I tried to do things a little differently. Instead I tried to sit down and break out each individual piece of my solution (similar to what's often referred to as the [substack pattern](http://substack.net/many_things)). What pieces could I work on separately that would have value and merit on their own? The result was 3 individual modules with each one being a composition of the last. Breaking out my solution like this did a few things that I think are big wins when programming:

1. I made my problem's surface area smaller by identifying tinier problems (and easier wins)
2. I made useful pieces of my solution available for others to compose into other potential solutions
3. My github activity bar is looking awesome

The problem I set out to solve was how to keep [dat](http://dat-data.com) (a project with the goal of streamlining data transfer between any and all data stores or file formats) in sync with [MongoDB](http://mongodb.org). Doing a single import had been solved by [mongs](https://npmjs.org/package/mongs) but there was no existing way to keep that import up to date with changes in Mongo. Having identified my problem I broke it out into smaller problems.

The first problem was translating [Mongo's oplog](http://docs.mongodb.org/manual/core/replica-set-oplog/) documents into actions that can be performed on any object. Mongo utilizes the oplog to translate changes in the primary to its secondaries keeping them in sync. The syntax of these JSON documents is little peculiar at first but they provide information about document updates, inserts, as well as deletes. Essentially everything you need to make the same changes to a document that lives outside of Mongo as well. I built [oplog-transform](https://npmjs.org/package/oplog-transform) to take oplog documents, turn them into functional expressions, and apply those changes to any JSON object you want.

Now that we can translate oplog docs we just need to hook up to the oplog and tail it to perform these changes in real-time. I created [oplog-transform-tail](https://npmjs.org/package/oplog-transform-tail) to do just that. It's a simple composition of the great [mongo-oplog](https://npmjs.org/package/mongo-oplog) lib as well as oplog-transform. We're already reaping the benefits of breaking these pieces out by being able to build them up in new useful ways.

Finally we can solve the overarching problem we set out for in the beginning, syncing dat with MongoDB, and the result is [dat-oplog](https://npmjs.org/package/dat-oplog). 

The code needed to create dat-oplog was relatively small since we can leverage the previous modules. Each module being responsible for its own functionality keeps the individual code bases small which can reduce bugs and increase transparency into what is happening. Building a solution this way has a lot of advantages and I think it's a worthwhile exercise when tackling any large problem.
