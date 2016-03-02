---
published: true
layout: post
title: Disable SSH Login for Root and Other Users
---



To disable direct ssh logins for a user, especially root, here's a basic run-down.

[How do I disable SSH login for the root user? - Media Temple](https://mediatemple.net/community/products/dv/204643810/how-do-i-disable-ssh-login-for-the-root-user)

---

Also, since this action can disable the root user's ssh access, you'll need to use `sudo` to become root. This post cleared up some of my own confusion about the differences between alternative ways to become root via `sudo`. It expecially cleared up for me which password is expected for each type of invocation for `sudo` or `su` and what the new shell environment will contain.

[su VS sudo su VS sudo -u -i - blog by @johnkpaul](http://johnkpaul.tumblr.com/post/19841381351/su-vs-sudo-su-vs-sudo-u-i)

---

To better understand which users on the box are allowed to connect via ssh, try [linux - List of all users that can connect via SSH - Stack Overflow](http://stackoverflow.com/questions/15802179/list-of-all-users-that-can-connect-via-ssh). This question was answered, sort of, almost, but it really wasn't answered sufficiently. 

More/better insights here (at least for debian): [linux - Which users are allowed to log in via SSH by default? - Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/36804/which-users-are-allowed-to-log-in-via-ssh-by-default). I think the centos configuration is much the same.

The /etc/shadow file has been confusing to me in the past. I understand it better now, after reading [Understanding /etc/shadow file](http://www.cyberciti.biz/faq/understanding-etcshadow-file/).

---

Some more tidbits with a useful variation on explanation of their purpose:

[AWS EC2: Steps to enable root access with your local key - Inspired by: https://forums.aws.amazon.com/thread.jspa?threadID=86876 · GitHub](https://gist.github.com/tracend/4016872)


