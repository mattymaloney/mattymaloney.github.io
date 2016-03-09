---
published: true
layout: post
title: Process for Disabling or Deleting a Linux User Account
---


Search for files and processes owned by the user.

To lock their password, preventing login via password: `passwd -l username`.

To expire the account, disabling authentication of any kind: `chage -E 0 username` or `usermod -e 0 username`.

Archive the home directory: `tar czf /home/username.tgz /home/username`.




See also:

[Linux: Delete / Remove User Account](http://www.cyberciti.biz/faq/linux-remove-user-command/)

