---
published: true
layout: post
title: "Setting Up Apache, PHP, WordPress, phpMyAdmin on and AWS VPC with EC2 Linux and RDS MariaDB, Take 2"
---

Two resources that were very valuable in getting this process started:

* [Tutorial: Installing a LAMP Web Server on Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html)
* [Tutorial: Hosting a WordPress Blog with Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/hosting-wordpress.html)


## Region and Availability Zone

Pick a region and availability zone. For a VPC, I'm pretty sure that all the boxes do not all need to be in the same availability zone, but as I understand so far, data transfer is free between boxes in a VPC when they are all in the same region and availability zone.

I'm picking:

	region: us-east-1
    availability zone: us-east-1c


## Create DB in new or existing MariaDB RDS instance

If you already have a MariaDB RDS instance you want to reuse, simply add a new database to it.

Create an new MariaDB RDS instance, if necessary.

- **DB Instance Class**: db.t2.micro
- **Multi-AZ**: no
- **Storage**: SSD
- **VPC**: let AWS create a new VPC
- **Publicly Accessible**: no
- **Availability Zone**: any, but use the same one later for EC2 instances
- **VPC Security Groups**: create new or reuse a group that allows 3306 traffic from your VPC private network
- **DB options**: leave empty, except port 3306 and copy tags to snapshots
- **Backup Retention**: 21 days

I'm using the _t2_ instance class because I want it to be available only to clients in the VPC. I haven't looked around enough yet to know how to make sure other instance types belong to my VPC and are only accessible to the VPC. UPDATE: I think I understand this better, and can set this security via Security Groups. I haven't tried this yet, as I'm still using the _t2_ instance classes.


## Examine the VPC

I don't understand everything that's been created with the VPC that AWS creates in this process.

- I don't understand why we need subnets, 4 of them. **UPDATE:** I see now that each subnet is in a different availability zone. Between the 4 of them, they encompass 4 of the 5 _us-east-*_ availability zones.
- I don't understand what our RDS instance calls a "Subnet Group", which I don't see anywhere in the AWS VPC dashboard. **UPDATE:** I do see Subnet Groups in the RDS dashboard. Still not sure what it means or what's its purpose.
- I don't understand why the RDS instance says it's on all 4 subnets that were created. Why divide the network into subnets only to put the service directly on all 4 subnets? **UPDATE:** see above. I think this is so that EC2 instances in any of the subnets or availability zones can communicate with the RDS instance without incurring bandwidth/transfer fees. ??


## Create httpd baseline instance

We will use this instance as a template for the phpMyAdmin instance as well as the 

- **OS/AMI**: Amazon Linux (ami-08111162)
- **Instance Type**: t2.nano (this is a necessary, but not-oft-used vm)
- **VPC**: the VPC that was create with the RDS instance
- **Availability Zone**: same that was chosen when creating the RDS instance
- **Enable termination protection**: yes
- Security Group: Select or create one with the following settings:
	- port 22 from office and home IPs.
    - ports 80 and 443 from all.

Name the instance, e.g. _lap-baseline_. In this case, _lap_ stands for Linux, Apache, PHP.

Associate an Elastic IP with the instance. I had to make this association in the VPC Console's "Elastic IPs" screen rather than the EC2 Console's Elastic IPs screen.

## Update OS and Install Services

Run updates and install apache/php as well as the php mysql native driver, the php mbstring extension, and the mysql client cli. Start httpd and set it to start automatically when the server boots.

```
sudo yum update
sudo yum install httpd24 mod24_ssl php56 php56-mysqlnd php56-mbstring mysql56
sudo service httpd start
sudo chkconfig httpd on
```

At this point, we should be able to see the server’s default apache “noindex” page over http using the public dns hostname shown in the ECs Management Console.


## Create OS permissions and apply to /var/www

Set file permissions on the /var/www folder to allow the ec2-user and apache users to access and change the content.

Outside of wordpress installations, do we need the apache service to have modify access to the /var/www folder?

Create the www group and add the ec2-user and apache users to it.

    sudo groupadd www
    sudo usermod -a -G www ec2-user
    sudo usermod -a -G www apache
    exit

Reconnect and check permissions.

	groups
    # should show "ec2-user wheel www"
 
Set better ownership and permissions on the /var/www directory, sub-directories, and files.

    sudo chown -R apache:www /var/www
    sudo chmod 2775 /var/www
    find /var/www -type d -exec sudo chmod 2775 {} \;
    find /var/www -type f -exec sudo chmod 0664 {} \;
    sudo service httpd restart


## Test Apache/PHP

Test and inspect the Apache/PHP installation.

	echo "<?php phpinfo(); ?>" > /var/www/html/pi.php
    
Then, in web browser, go to http://public.dns.hostname/pi.php to verify that Apache and PHP are working and have the proper packages and modules installed and enabled.


## Slim-down Apache's per-process footprint

	sudo mv conf.d conf.d.disabled
    sudo mv conf.moduled.d conf.modules.d.disabled
    
Then, add the following files:

`conf.d/10-php56.conf`:

```
#
# Allow php to handle Multiviews
#
AddType text/html .php

#
# Add index.php to the list of files that will be served as directory
# indexes.
#
DirectoryIndex index.php

#
# Cause the PHP interpreter to handle files with a .php extension.
#
<FilesMatch \.php$>
SetHandler application/x-httpd-php
</FilesMatch>

#
# Apache specific PHP configuration options
# those can be override in each configured vhost
#
php_value session.save_handler "files"
php_value session.save_path    "/var/lib/php/5.6/session"
php_value soap.wsdl_cache_dir  "/var/lib/php/5.6/wsdlcache"
```

`conf.d/50-server-status.conf.disabled`:

```
LoadModule info_module modules/mod_info.so
LoadModule status_module modules/mod_status.so

<Location "/server-status">
  SetHandler server-status
  Require ip [office ip block(s)]
  Require ip [other authorized ip block(s)]
  Require local
</Location>

<Location "/server-info">
  SetHandler server-info
  Require ip [office ip block(s)]
  Require ip [other authorized ip block(s)]
  Require local
</Location>
```

`conf.modules.d/00-required.conf`:

```
#
# prefork MPM: Implements a non-threaded, pre-forking web server
# See: http://httpd.apache.org/docs/2.4/mod/prefork.html
#
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so

#
# mod_unixd to support the User and Group directives.
# - to run Apache as an alternative user and/or group.
#
LoadModule unixd_module modules/mod_unixd.so

#
# mod_authz_core to support the Require directive.
# mod_authz_host to allow `ip` and `local` in Require directive.
# - to restrict and allow access to given directories and/or locations.
#
LoadModule authz_core_module modules/mod_authz_core.so
LoadModule authz_host_module modules/mod_authz_host.so

#
# mod_mime required for AddType and AddHandler directives
#
LoadModule mime_module modules/mod_mime.so

#
# mod_dir required for DirectoryIndex directive.
#
LoadModule dir_module modules/mod_dir.so

#
# PHP as a prefork module
#
LoadModule php5_module modules/libphp-5.6.so
```

As far as I can tell, these are the only modules we need... so far. We'll see what else specifically is needed by phpMyAdmin and WordPress.


## Setup Apache SSL

Setup SSL, even without the final hostname or certificates. For now, use a self-signed certificate. The purpose of the setup is not to finalize it, but rather to setup the proper structure. The actual certificate files will need to be added later.

Set up ssl for apache.
Tight ciphersuite.
Force http --> https.
Enable hsts.
Test at ssllabs.com.

```
sudo mkdir /etc/httpd/ssl
cd /etc/httpd/ssl

```


## Create an AMI for this baseline

Use AWS console to create an AMI from this instance. Set the name of the ami and its snapshot to _lap-baseline_.


## Create phpMyAdmin EC2 instance

Choosing:

- **OS/AMI**: _lap-baseline_ created previously.
- **Instance Type**: t2.nano (since this is a necessary, but not-oft-used vm)
- **VPC**: the VPC that was create with the RDS instance
- **Availability Zone**: same that was chosen when creating the RDS instance
- **Enable termination protection**: yes
- Security Group: Select or create one with the following settings:
	- port 22 from office and home IPs.
    - ports 80 and 443 from all.

## 