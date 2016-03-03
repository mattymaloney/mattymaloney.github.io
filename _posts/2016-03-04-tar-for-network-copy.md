---
published: true
layout: post
title: "Using `tar` for Copying Across the Network"
---

I like this suggestion for using tar to copy over the network. I think it'd even be safe to add compression to this command.

http://serverfault.com/a/18142/121200 from [linux - How to copy a large number of files quickly between two servers - Server Fault](http://serverfault.com/questions/18125/how-to-copy-a-large-number-of-files-quickly-between-two-servers)

e.g.

```
tar -c /path/to/dir | ssh remote_server 'tar -xvf - -C /absolute/path/to/remotedir'
```

instead of 

```
rsync -avW -e ssh /path/to/dir/ remote_server:/path/to/remotedir
```
