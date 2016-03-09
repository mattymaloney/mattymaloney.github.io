---
published: true
layout: post
title: Process for Disabling or Deleting a Linux User Account
---


Search for files and processes owned by the user.

To lock their password, preventing login via password: `passwd -l username`.

To expire the account, disabling authentication of any kind: `chage -E 0 username` or `usermod -e 0 username`.

Archive the home directory, crontab, and mail: `tar czf /home/username.tgz /home/username /var/spool/cron/username /var/spool/mail/username`.

Check for running processes owned by user: `ps -fp $(pgrep -u username)`.

If user's processes need to be killed: `killall -KILL -u username`.

Delete the user account and home directory: `userdel -r username`.

---

To see what's configured to be created for new users and deleted when running the `userdel` command, see `/etc/login.defs`.

---

See also:

[Linux: Delete / Remove User Account](http://www.cyberciti.biz/faq/linux-remove-user-command/)

