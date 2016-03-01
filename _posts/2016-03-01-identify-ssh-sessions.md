---
published: true
layout: post
title: How to Identify Active SSH Sessions
---

To discover active ssh sessions on a box,

`sudo netstat -tnpa | grep ESTABLISHED.*sshd`

and

`w`

---

See also: [terminal - List all connected SSH sessions? - Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/92560/list-all-connected-ssh-sessions)
