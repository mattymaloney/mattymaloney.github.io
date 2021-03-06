---
published: true
layout: post
title: nc for Network Copy
---


I came across a brilliantly simple device today, which I shall use often to perform copy and paste and other sharing operations between 2 linux hosts.

See [Copy/paste to Micro Core as a Virtualbox guest?](http://forum.tinycorelinux.net/index.php/topic,11306.msg59248.html#msg59248).

The posted example: 

```
nc -l -p 1234 > /tmp/text # on the guest

nc ip.of.guest 1234 << "EOF" # on the host
CFLAGS=""
...
EOF
# press ctrl-c
```

But according to `man nc`, the `-l` and `-p` flags can not be used together. That command should be `nc -l 1234`.

So, this doesn't work for me everywhere I want it to work because the public servers are all firewalled with restricted ports open. I'd need to open up an additional port that I can use for nc. So, I guess I'll just stick with `scp` and `rsync` for now.
