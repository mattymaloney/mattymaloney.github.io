---
layout: post
title: Using `bash` to Move Files and Directories to the Trash in Mac OS X
published: true
---

* [osx - How to move files to trash from command line? - Ask Different](http://apple.stackexchange.com/questions/50844/how-to-move-files-to-trash-from-command-line)
* [morgant/tools-osx: A small collection of command line tools for Mac OS X, incl.: clipcat, dict, eject, ql, swuser, trash & with.](https://github.com/morgant/tools-osx)


e.g.:

```
osascript -e 'tell app "Finder" to move the POSIX file "/Users/matty/ssl/www.ccr.az.2016-06-06.bad.tar.gz" to trash'
```
