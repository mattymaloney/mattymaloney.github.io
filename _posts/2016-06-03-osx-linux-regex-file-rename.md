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

Then, removing the `echo` condom:

```
for f in _.*; do mv "$f" "${f/_/2019-06-05._}"; done
```

---

Well, apparently, I can't use "real" regular expressions in that `mv $f ${f//}` syntax, or at least I couldn't figure out how to do `\n` substitutions with that syntax. So, using `sed`, based on [osx - Regex to batch rename files in OS X Terminal - Stack Overflow](http://stackoverflow.com/questions/10664542/regex-to-batch-rename-files-in-os-x-terminal/10664565#10664565), this worked for a more complex renaming operation:

```
for f in 2017*; do sudo mv "$f" `echo $f | sed "s/^(....)(..)(..)/\1-\2-\3/g" -r`; done
```

Side note -- the trouble using the `mv $f ${f//}` syntax was actually with a CentOS box. I haven't tried more complex expressions on Mac OS X yet.
