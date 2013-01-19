---
layout: post
title: Passwordless SSH Logins on Ubuntu (12.04)
---

I recently had to set up a remote server and it's been a while since doing so. One of my favorite things about ssh login is setting it up to work without a password. Since it's been so long since I last set something like this up I began to scour the internet for the right incantations to implement such behavior. To my surprise the info is out there but **damn** is it scattered. So today I write this not only for others to have a quick reference, but also myself that I may have a nice succinct description for setting up something that is quite simple. Let's begin.

##The Setup

Congrats, you've just installed Ubuntu Server([Ubuntu 12.04](https://wiki.ubuntu.com/PrecisePangolin/ReleaseNotes/UbuntuServer) is the latest LTS[^1] and that is what this guide is written against) and now you're ready to set up login. Depending on how you arrived at your installation whether it is an image installed by a cloud provider or you installed it on your own system, you may or may not have SSH already set up. Obviously if you have SSH credentials from your cloud admin that means everything is already set up but for the sake of simplicity let's say you're at your commandline. This is how you can check.

```
ps -A | grep ssh
```

What just happened? Well we told the server to list out current running processes (that's the ```ps``` command) and indicated that we would like to see all processes including those without terminals with the ```-A``` flag. That by itself (```ps -A```) is enough to see all of the processes running on your system but we just care about ssh. So we then "pipe" that content into grep, saying that we would only like to see the processes that have the word ssh in them.
