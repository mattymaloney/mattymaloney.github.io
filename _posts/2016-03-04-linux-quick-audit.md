---
published: true
layout: post
title: "Linux Quick-Audit Commands Collections"
---


I'm just trying to collect some commands I'm using for some manual auditing/assessing for our linux boxes.

---

To see which users have (or might have) legit login shells:

```
cat /etc/passwd | grep -Ev -e '/s?bin/(nologin|sync|shutdown|halt)$'
```

I should further awk or cut this to show only the username, maybe along with whatever shell is defined.

---

To see which ports are open for listening:

```
sudo netstat -tuwanp | grep LISTEN | awk '{print $4}' | grep ':' | cut -d ':' -f 2,4 | cut -d ':' -f 2 | sort -n | uniq
```

Add `| paste -sd ',' -` on the end to get a comma-separated list.

---

To see which users have a valid password:

```
sudo cat /etc/shadow | grep -Ev -e '^\w+:[\*!]'
```

If the password hash contains only `!` and/or `*` characters, then that user can not log in, and that user has likely never had a password.

If the password hash begins with one or more `!` characters but is followed by a legit crypt hash, then the user account is locked, but does have a password assigned, and could theoretically login if the account were unlocked.

I gathered these details mostly from [\[all variants\] exclamation mark vs asterisk in /etc/shadow](http://ubuntuforums.org/showthread.php?t=2026413).

---

To see all the non-comment and non-blank lines of `/etc/ssh/sshd_config` and other files:

```
sudo grep -v -e ^# -e ^$ /etc/ssh/sshd_config
```
