---
published: true
layout: post
title: "sshd_config's PermitRootLogin Directive"
---

Finally clearing up some confusion about the `/etc/ssh/sshd_config` file's `PermitRootLogin` directive.

Potential values (or at least the ones I think I understand):

`PermitRootLogin yes` - the root user can connect via ssh by either password or authorized_keys.

`PermitRootLogin without-password` - the root user can connect via ssh by authorized_keys only.

`PermitRootLogin no` - the root user can not connect via ssh by neither password or authorized_keys.
