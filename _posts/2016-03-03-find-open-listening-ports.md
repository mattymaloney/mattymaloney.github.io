---
published: true
layout: post
title: Show All Port Open for Listening Right Now
---

To find out what ports are open for listening, we have a few options.

## nmap

```
nmap -sT -O localhost
```

However, nmap isn't always installed. I'm rather sure I've never seen is in a default installation.


## netstat

