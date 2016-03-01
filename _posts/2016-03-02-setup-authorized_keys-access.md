---
published: true
layout: post
title: Instructions for Setting Up ssh Access via authorized_keys
---

Setting up authorized_keys access to boxes has always been a point of confusion. I manage to get it done, but it never feels clean. I probably don't protect the private key properly, and several times, I've generated the key pair on the server rather than the client.

It's time finally to write out my simple instructions for getting this done efficiently.

On the server:

Make sure that the user account you're connecting to has an `~/.ssh` directory with `700` permissions and an `authorized_keys` file with `600` permissions. The home directory itself, should also not be writable by any other users, so max permissions for `~` are `755`.

```
```

One the client:

```
```
