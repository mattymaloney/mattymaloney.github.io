---
published: true
layout: post
title: "Linux Quick-Audit Commands Collections"
---

I'm just trying to collect some commands I'm using for some manual auditing/assessing for our linux boxes.

---

To see which users have (or might have) legit login shells:

```
cat /etc/passwd | egrep -v -e '/s?bin/(nologin|sync|shutdown|halt)$'
```

I should further awk or cut this to show only the username, maybe along with whatever shell is defined.

---

To see which ports are open for listening:

```
netstat -tuwanp | grep LISTEN | awk '{print $4}' | grep ':' | cut -d ':' -f 2,4 | cut -d ':' -f 2 | uniq | sort -n
```

Add `| paste -sd ',' -` on the end to get a comma-separated list.

---

