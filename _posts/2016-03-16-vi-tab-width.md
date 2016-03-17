---
published: true
layout: post
title: "Setting the Tab Width in `vim`"
---

There seem to be 3 settings that affect tab width.

`tabstop`, `softtabstop`, and `shiftwidth`

When indenting or dedenting using `<<` or `>>`, `shiftwidth` determines how far to or from the left margin the line will move.

To change `shiftwidth`, use `:set shiftwidth=n`, where `n` is the number of spaces you want `>>` and `<<` to move the line.

See also: [css - How do I change tab size in Vim? - Stack Overflow](http://stackoverflow.com/questions/2054627/how-do-i-change-tab-size-in-vim)
