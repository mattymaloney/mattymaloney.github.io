---
published: true
layout: post
title: Bulk Regular Expression File Rename in Mac OS X
---

The mac os x `rename` command doesn't seem to support regular expressions. Based on [osx - mac os x terminal batch rename - Stack Overflow](http://stackoverflow.com/questions/24102974/mac-os-x-terminal-batch-rename/24103055#24103055), I used this command to rename files.

First, to see what it was going to do:

```
for f in _.*; do echo mv "$f" "${f/_/2019-06-05._}"; done
```

Then:

```
for f in _.*; do mv "$f" "${f/_/2019-06-05._}"; done
```
