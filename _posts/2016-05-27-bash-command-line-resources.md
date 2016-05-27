---
published: true
layout: post
title: "bash Command-Line Resources"
---
## [Shortcuts to move faster in Bash command line - teohm.dev](http://teohm.com/blog/2012/01/04/shortcuts-to-move-faster-in-bash-command-line/)

### Basic moves

Move back one character. Ctrl + b
Move forward one character. Ctrl + f
Delete current character. Ctrl + d
Delete previous character. Backspace
Undo. Ctrl + -


### Moving faster

Move to the start of line. Ctrl + a
Move to the end of line. Ctrl + e
Move forward a word. Meta + f (a word contains alphabets and digits, no symbols)
Move backward a word. Meta + b
Clear the screen. Ctrl + l
What is Meta? Meta is your Alt key, normally. For Mac OSX user, you need to enable it yourself. Open Terminal > Preferences > Settings > Keyboard, and enable Use option as meta key. Meta key, by convention, is used for operations on word.

### Cut and paste (‘Kill and yank’ for old schoolers)

Cut from cursor to the end of line. Ctrl + k
Cut from cursor to the end of word. Meta + d
Cut from cursor to the start of word. Meta + Backspace
Cut from cursor to previous whitespace. Ctrl + w
Paste the last cut text. Ctrl + y
Loop through and paste previously cut text. Meta + y (use it after Ctrl + y)
Loop through and paste the last argument of previous commands. Meta + .


### Search the command history

Search as you type. Ctrl + r and type the search term; Repeat Ctrl + r to loop through results.
Search the last remembered search term. Ctrl + r twice.
End the search at current history entry. Ctrl + j
Cancel the search and restore original line. Ctrl + g

---

## [Bash Features - Command Line Editing](http://web.mit.edu/gnu/doc/html/features_7.html)

This is a more complete discussion of available movement options.

---
