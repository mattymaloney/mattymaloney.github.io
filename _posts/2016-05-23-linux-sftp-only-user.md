---
published: true
layout: post
title: "Setup an sftp-only User in Linux"
---
How to setup a user to access the server via sftp only.

---

Add this to the end of the `/etc/ssh/sshd_config` file:

```
Match User wp-test
  PasswordAuthentication yes
  ChrootDirectory %h
  X11Forwarding no
  AllowTcpForwarding no
  ForceCommand internal-sftp
```
