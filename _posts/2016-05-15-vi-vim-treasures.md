---
published: true
layout: post
title: vi/vim Treasures
---
[vi - How to effectively work with multiple files in Vim? - Stack Overflow](http://stackoverflow.com/questions/53664/how-to-effectively-work-with-multiple-files-in-vim?rq=1)

[All the right moves - Vim Tips Wiki - Wikia](http://vim.wikia.com/wiki/All_the_right_moves)

[vi/vim delete commands and examples | vi vim delete lines to end | alvinalexander.com](http://alvinalexander.com/linux/vi-vim-delete-line-commands-to-end)

---

To clear the last search: `:let @/= ""`. The clears the search string register.

To turn off highlighting until the next search operation (including pressing `n`): `:noh`.

To set a new command to perform a long-winded operation (e.g. to let the `:C` command perform the same work as `:let @/ = ""`: `:command C let @/=""`.

Ref: [highlight - Vim clear last search highlighting - Stack Overflow](http://stackoverflow.com/questions/657447/vim-clear-last-search-highlighting)

---
