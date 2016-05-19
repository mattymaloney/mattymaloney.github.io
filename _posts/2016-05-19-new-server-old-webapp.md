---
published: false
layout: post
title: Setup New LAMP Server for Old Web Application
---
I need to setup a linux/apache/mysql/php server with Apache 2.2.x, mysqld 5.5.x, php 5.3.x. The latest version of each: httpd-2.2.31, mysqld-5.5.27, php-5.3.29. 

```
sudo yum install httpd-2.2.31 mysql55-server php-5.3.29 php-cli-5.3.29 php-common-5.3.29 php-devel-5.3.29 php-gd-5.3.29 php-mbstring-5.3.29 php-mcrypt-5.3.29 php-mysql-5.3.29 php-pdo-5.3.29 php-soap-5.3.29 php-xml-5.3.29 php-pear

sudo service httpd start
sudo chkconfig httpd on

sudo service mysqld start
sudo chkconfig mysqld on

```

