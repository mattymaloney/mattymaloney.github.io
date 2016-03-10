---
published: true
layout: post
title: Searching for Symlinks
---

I've several times wanted to search a directory tree for all the symlinks, but I never bothered to figure out how to do it. Today, I really needed a comprehensive search, and this is what I found.

http://stackoverflow.com/a/21385834/1298086

```
find ./ -type l -print0 | xargs -0 ls -plah
```

which lists the indivualized symlinks with their target.


---

From the same SO question, another answer provides some more interesting and educational insights into using `find` to find symlinks.

http://stackoverflow.com/a/21438684/1298086

--- 