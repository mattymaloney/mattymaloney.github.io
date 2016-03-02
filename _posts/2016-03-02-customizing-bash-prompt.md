---
published: true
layout: post
title: Customizing the bash Prompt
---

I always want to customize the bash prompt, to add some space between commands and to colorize the prompt to more easily recognize where I am and who I am. However, since I don't do this often enough to remember how to do it, and I'm always reluctant to take the time to look it up, I mostly get by with the default bash prompts.

Now, to remedy that, I will finally take some notes on my preferred prompt configuration.

Well, in the end, I used [.bashrc generator: create your .bashrc PS1 with a drag and drop interface](http://bashrcgenerator.com/) and [Easy Bash Prompt Generator](http://ezprompt.net/) to generate the prompt. That did make it much easier than previous experiences. However, my prompt code is totally bloated by `tput sgr0` calls and much more. Nevertheless, it's functional, and I'll use it for now.

Here is the code that I'll use to ~/.bashrc for Linux servers:

```
export PS1="\n\[$(tput sgr0)\]\[\033[38;5;3m\]\u\[$(tput sgr0)\]\[\033[38;5;15m\]@\[$(tput sgr0)\]\[\033[38;5;3m\]\H\[$(tput sgr0)\]\[\033[38;5;15m\] [\[$(tput sgr0)\]\[\033[38;5;5m\]short-name\[$(tput sgr0)\]\[\033[38;5;15m\]]\n\[$(tput bold)\]\[$(tput sgr0)\]\[\033[38;5;2m\]\w\[$(tput sgr0)\]\[$(tput sgr0)\]\[\033[38;5;15m\]\[$(tput bold)\]\[$(tput sgr0)\]\[\033[38;5;4m\]\[$(tput sgr0)\]\[\033[38;5;15m\]\n\\$ \[$(tput sgr0)\]"
```

---

Someday, also consider the [fish shell](https://fishshell.com/).


