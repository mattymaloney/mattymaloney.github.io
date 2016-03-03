---
published: true
layout: post
title: Show All Port Open for Listening Right Now
---

To find out what ports are open for listening, we have a few options.

Simple answers are available here: [Find Out What Ports Are Listening / Open On My Linux & FreeBSD Server](http://www.cyberciti.biz/faq/how-do-i-find-out-what-ports-are-listeningopen-on-my-linuxfreebsd-server/)

And here: [Get a list of Open Ports in Linux - Super User](http://superuser.com/questions/529830/get-a-list-of-open-ports-in-linux)

From RedHat, including a mention of nmap for this purpose: https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/Security_Guide/s1-server-ports.html


## nmap

```
nmap -sT -O localhost
```

However, nmap isn't always installed. I'm rather sure I've never seen is in a default installation.


## netstat

```
netstat -lnptu
```

`netstat` seems to be installed on all of our boxes, and I almost assume it's a default component of new installations.

```
netstat -tuwanp | grep LISTEN | awk '{print $4}' | grep ':' | cut -d ":" -f 2 | sort -n | uniq
```

This is a tweaked version from a comment on superuser. It does work to produce a sorted list of listening port numbers.


## lsof

I don't like the `lsof` commands much because they return too many results on a busy box. In this case, I'm interested in seeing open/listening ports. I don't need to see every open port and every connection to every open port.





