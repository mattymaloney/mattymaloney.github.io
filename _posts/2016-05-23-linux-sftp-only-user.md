---
published: true
layout: post
title: "Setup an sftp-only User in Linux"
---
How to setup a user to access the server via sftp only. 

---

For allowing a user to login via password, see http://serverfault.com/questions/154957/set-up-sftp-to-use-password-but-ssh-not-to-use-password. In some cases, we want the `ec2-user` to be able to login via keypair only, while allowing, e.g. the content-upload wordpress user, to login via password.

Create the user, assign a password, set the user's default group to `www` (same as the apache user), and mount the html directory into the user's home directory (so that it can be accessed without traversing up and then down into the `/var/www` directory).

Only execute the `sudo passwd` line if we want the user to login via password rather than via keypair.

```
sudo adduser username
sudo passwd username
sudo usermod -g www username
sudo mkdir /home/username/www
sudo chown username:username /home/username/www
sudo chown root:root /home/username/
sudo chmod 755 /home/username
sudo mount --bind /var/www/html /home/username/www
```

These are the lines that need to be added to the end of `/etc/ssh/sshd_config`. I don't properly understand the `X11Forwarding` and `AllowTcpForwarding` lines, but every example I see has them, so I'm going with it.

If the user should be allowed to login via password, then `PasswordAuthentication` should be `yes`. Otherwise, that line can be removed or set to `no`.

The `ChrootDirectory` needs to be owned by and only writable by root. `%h` evaluates to the user's home directory as defined in `/etc/passwd`.

```
Match User username
  PasswordAuthentication yes
  ChrootDirectory %h
  X11Forwarding no
  AllowTcpForwarding no
  ForceCommand internal-sftp
```

Restart sshd:

```
sudo service sshd restart
```

Some helpful insight into `sshd` and especially `ChrootDirectory`:

* [Setting Up an sftp Site on Amazon Web Services EC2, and a Guest Account | Software Projects](https://rmtheis.wordpress.com/2011/07/03/setting-up-an-sftp-site-on-amazon-web-services-ec2-creating-an-account-to-share-with-a-third-party-and-restricting-that-account-to-allow-only-sftp/)
* [How to add user with SFTP/ FTP access to '/var/www/html/website_abc' folder on Amazon EC2 Centos? - Server Fault](http://serverfault.com/questions/392601/how-to-add-user-with-sftp-ftp-access-to-var-www-html-website-abc-folder-on-a)
* [ubuntu - Set up sftp to use password but ssh not to use password - Server Fault](http://serverfault.com/questions/154957/set-up-sftp-to-use-password-but-ssh-not-to-use-password)
* [How to add pointers to filesystems outside the chrooted environment - using chrooted SFTP via OpenSSH - Super User](http://superuser.com/questions/247125/how-to-add-pointers-to-filesystems-outside-the-chrooted-environment-using-chro)
* [amazon ec2 - Chrooted user logged out immediately after login - Server Fault](http://serverfault.com/questions/643396/chrooted-user-logged-out-immediately-after-login)
* [ssh - bad ownership or modes for chroot directory component - Server Fault](http://serverfault.com/questions/584986/bad-ownership-or-modes-for-chroot-directory-component)
* [sshd_config(5)](https://www.freebsd.org/cgi/man.cgi?query=sshd_config&sektion=5)


