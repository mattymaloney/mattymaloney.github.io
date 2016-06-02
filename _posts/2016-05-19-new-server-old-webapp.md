---
published: false
layout: post
title: Setup New LAMP Server for Old Web Application
---
I need to setup a linux/apache/php server with Apache 2.2.x, mysqld 5.5.x, php 5.3.x. The latest version of each: httpd-2.2.31, mysqld-5.5.27, php-5.3.29. 

While keeping everything else old, we're migrating the mysql service to RDS as MariaDB.

```
sudo yum install httpd-2.2.31 mod_ssl-2.2.31 php-5.3.29 php-cli-5.3.29 php-common-5.3.29 php-devel-5.3.29 php-gd-5.3.29 php-mbstring-5.3.29 php-mcrypt-5.3.29 php-mysql-5.3.29 php-pdo-5.3.29 php-soap-5.3.29 php-xml-5.3.29 php-pear

sudo service httpd start
sudo chkconfig httpd on

sudo service mysqld start
sudo chkconfig mysqld on
```

Modify httpd.conf to point to a DocumentRoot on our temp storage device.

Transfer the backup/archive of the app from the old server to this new test server, restore the archive, move the public_html into the new apache document root, set permissions for apache.

```
# move the archived/restored public_html folder into /extra/www and set permissions.
mv /extra/[archive-folder]/public_html /extra/www
cd /extra/www
sudo chown -R apache:www .
sudo chmod 2775 .
find . -type d -exec sudo chmod 2775 {} \;
find . -type f -exec sudo chmod 0664 {} \;
```
