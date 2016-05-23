---
published: true
layout: post
title: "Create \"lap-wp-baseline\" by Adding WordPress to \"lap-baseline\" on an AWS VPC with EC2 Linux and RDS MariaDB, Take 3"
---



## Create WordPress EC2 instance

Choosing:

- **OS/AMI**: _lap-baseline_ created previously.
- **Instance Type**: t2.nano (since this is just a test)
- **VPC**: the VPC that was create with the RDS instance
- **Availability Zone**: same that was chosen when creating the RDS instance
- **Enable termination protection**: yes
- **Security Group**: Select or create one with the following settings:
	- port 22 from office and home IPs.
    - ports 80 and 443 from all.

## Permission to Access RDS Instance

Make sure that this EC2 instance's security group is added to the permitted ingress traffic for the RDS instance's security group.

Verify connection to the database using wordpress's username and password.


## Update Apache `ServerName`

Edit `/etc/httpd/conf/httpd.conf` to update the `ServerName` directive.

Restart Apache.

```
sudo service httpd restart
```


## Cleanup `conf.modules.d`

Delete any files in `/etc/httpd/conf.modules.d` that were added by `yum` during updates.

e.g. `sudo rm -rf /etc/httpd/conf.modules.d/10-php*`


## Install WordPress

Download and extract wordpress.

```
cd
wget --ca-certificate /etc/pki/tls/certs/ca-bundle.crt https://wordpress.org/latest.tar.gz
tar xzf latest.tar.gz
rm latest.tar.gz
```

Edit wordpress wp-config.php, setting up db connection details and replacing the keys/salts section with data from https://api.wordpress.org/secret-key/1.1/salt/.

```
cd wordpress
cp wp-config-sample.php wp-config.php
vi wp-config.php
```

Move extracted wordpress into `/var/www`, set apache:www ownership and appropriate permissions, and point `DocumentRoot` at `/var/www/wordpress`. Also, create and edit `/etc/httpd/conf.d/vhosts.conf`, creating a `VirtualHost` for wordpress.

```
sudo rm -rf /var/www/html
sudo mv ~/wordpress /var/www/html
sudo chown -R apache:www /var/www
sudo chmod 2775 /var/www
find /var/www -type d -exec sudo chmod 2775 {} \;
find /var/www -type f -exec sudo chmod 0664 {} \;
```


## Create wordpress sftp user

UPDATE: see also _\_posts/2016-05-23-linux-sftp-only-user.md_ for an updated and more generic version of this process.

For allowing only the wordpress content-control user to login via password, see http://serverfault.com/questions/154957/set-up-sftp-to-use-password-but-ssh-not-to-use-password. In our case, we want the `ec2-user` to be able to login via keypair only, while allowing the content-upload wordpress user to login via password.

Create the user, assign a password, set the user's default group to `www` (same as the apache user), and mount the wordpress html directory into the user's home directory (so that it can be accessed without traversing out to the /var/www directory).

```
sudo adduser wp-test
sudo passwd wp-test
sudo usermod -g www wp-test
sudo mkdir /home/wp-test/www
sudo chown wp-test:wp-test /home/wp-test/www
sudo chown root:root /home/wp-test/
sudo chmod 755 /home/wp-test
sudo mount --bind /var/www/html /home/wp-test/www
```

These are the lines that need to be added to the end of `/etc/ssh/sshd_config`.

```
Match User wp-test
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
