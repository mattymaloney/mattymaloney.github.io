---
published: true
layout: post
title: "Finding Files *NOT* Matching a Pattern in Bash"
---

In a seemingly endless quest to improve my sysadmin skills to a functional level, I had to learn to search for files without a given extension, in this case `.conf`.

```
find /path/to/files -type f ! -name "*.conf"
```

This finds all files in the given path (not recursively) that do *NOT* have the `.conf` extension, and *ARE* files.

The `!` is used before the argument(s?) that you want to negate. e.g., moving the `!` in front of the `-type f` argument (`find . ! -type f -name "*.conf"`) will find all entries in the current directory, that are *NOT* files, but *DO* have the `.conf` extension.

To delete all the files in the current directory *EXCEPT* those with the `.conf` extension:

```
find . -type f ! -name "*.conf" -delete
```
