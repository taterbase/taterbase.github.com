---
layout: post
title: Tips for Working with Go and crypto/ssh
---

I've been working on an internal tool at [Spacemonkey](http://spacemonkey.com) that requires [SSH](http://en.wikipedia.org/wiki/Secure_Shell) functionality to some of our servers. We're big fans of [Go](http://golang.org) and still being new to the language I wanted to take this is an opportunity to become more familiar with some of its excellent tooling. Go has plenty of great built in libraries but there are a few they keep external as [SubRepositories](https://github.com/golang/go/wiki/SubRepositories). [crypto/ssh](http://godoc.org/golang.org/x/crypto/ssh) is one such package that makes navigating SSH networking a breeze.

While building this internal tool there were a few areas of the library that I found confusing, difficult, and downright impossible. After much code spelunking and some help from my coworkers I was able to achieve my goals and my hope is to pass on some solutions and tips that could have saved me a bunch of hassle. With that, let's get started.

## Passphrase Protected Private Keys

Our internal tool has its own private key, public key, and certificate. In order to SSH to our servers we need these in our keyring to successfully authenticate.

## SSH Certificates

[SSH certificates](https://blog.habets.se/2011/07/OpenSSH-certificates) are a signed public key that can allow you to ssh into hosts without them needing you to already exist on its `authorized_keys` list.

## Forwarding Your Agent

Our internal tool has to make a couple server hops to complete its task. Forwarding your agent (your keyring) is somewhat new functionality in crypto/ssh but is somewhat opaque about how to use it.


