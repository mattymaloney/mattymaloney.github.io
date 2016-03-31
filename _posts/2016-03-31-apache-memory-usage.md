---
published: true
layout: post
title: "Determining Apache (Average) Per-Process Memory Usage"
---



```
ps -ylC httpd --sort:rss | awk '{sum+=$8; ++n} END {print "Tot="sum"("n")";print "Avg="sum"/"n"="sum/n/1024"MB"}'
```

but this has the problem of counting 1 more process than actually exists, and it counts the root process as well -- I'm really looking for the size (or average size) or child processes. The parent process needs to be examined separately.

... or simply `ps -ylC httpd` for a glance.

---

See also:

* [Average Apache Process Memory Usage - Goettner.net](https://www.goettner.net/2012/average-apache-process-memory-usage/)
* [memory usage - Finding Average size of single Apache process for setting MaxClients - Server Fault](http://serverfault.com/questions/353828/finding-average-size-of-single-apache-process-for-setting-maxclients)
* [Finding Apache’s Memory Usage | Pseudo Random Bytes](http://arr.gr/blog/2007/05/finding-apaches-memory-usage/)
* [Apache | Karl Rixon](http://www.karlrixon.co.uk/category/writing/apache/)
