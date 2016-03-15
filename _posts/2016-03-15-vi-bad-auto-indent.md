---
published: true
layout: post
title: "Avoiding `vi`'s Bad Auto-Indenting"
---

On seemingly random servers, `vi` has this awfully annoying way of auto-indenting text that I paste. It's particularly bad when I'm editing Apache/httpd conf files, or at leat that's what's annoying me right now.

Choosing to no longer be lazy, I finally searched for an explanation of this odd auto-indent behavior. I found this simple explanation: [Toggle auto-indenting for code paste - Vim Tips Wiki - Wikia](http://vim.wikia.com/wiki/Toggle_auto-indenting_for_code_paste).

To turn off `vi`'s "smart" indentation of pasted input, use:

```
:set paste
```

To turn on `vi`'s auto-indent, use:

```
:set nopaste
```

