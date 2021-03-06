---
published: true
layout: post
title: "HTTP-Only LAP EC2 Instance for Elastic Load Balancing"
---


Two resources that were very valuable in getting this process started:

* [Tutorial: Installing a LAMP Web Server on Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html)
* [Tutorial: Hosting a WordPress Blog with Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/hosting-wordpress.html)


This one for hints on timezone settings for php and for the server:

* [Amazon EC2 – Installing Apache and PHP boilerplate – SpiderSoft](http://www.spidersoft.com.au/2013/apache-php-config-boilerplate/)


And some suggestions on which apache modules can be stripped. But in the end, I disabled all modules and reenabled modules as needed.

* [Strip Down Apache to Improve Performance & Memory Efficiency](http://haydenjames.io/strip-apache-improve-performance-memory-efficiency/)


## Region and Availability Zone

Pick a region and availability zone. For a VPC, I'm pretty sure that all the boxes do not all need to be in the same availability zone, but as I understand so far, data transfer is free between boxes in a VPC when they are all in the same region and availability zone.

I'm picking:

	region: us-west-1
    availability zone: us-west-1a


## The VPC

Use a new subnet in the existing VPC.


## Create httpd baseline instance

We will use this instance as a template for the phpMyAdmin instance as well as the 

- **OS/AMI**: Amazon Linux (ami-08111162)
- **Instance Type**: t2.nano (experimental vm)
- **VPC**: the VPC with the RDS instance
- **Availability Zone**: same AZ as the RDS instance
- **Enable termination protection**: yes
- **Security Group**: Select or create one with the following settings:
	- port 22 from office and home IPs.
    - ports 80 from within the VPC.

Name the instance, e.g. _lap80. In this case, _lap_ stands for Linux, Apache, PHP.

Associate an Elastic IP with the instance. I had to make this association in the VPC Console's "Elastic IPs" screen rather than the EC2 Console's Elastic IPs screen.

## Update OS and Install Services

Run updates and install apache/php as well as the php mysql native driver, the php mbstring extension, and the mysql client cli. Start httpd and set it to start automatically when the server boots.

```
sudo yum update
sudo yum install \
  httpd24 \
  php56 php56-mysqlnd php56-mbstring php56-pecl-memcached \
  php56-pecl-redis php-pear php56-opcache \
  mysql56
sudo service httpd start
sudo chkconfig httpd on
```

**Apache components**: httpd24 (no mod24_ssl)

**PHP Components**: php56 php56-mysqlnd php56-mbstring php56-redis php56-memcached-pecl php-pear

**MYSQL Client**: mysql56

At this point, we should be able to see the server’s default apache “noindex” page over http using the public dns hostname shown in the ECs Management Console.


## Create OS permissions and apply to /var/www

Set file permissions on the /var/www folder to allow the ec2-user and apache users to access and change the content.

Outside of wordpress installations, do we need the apache service to have modify access to the /var/www folder?

Create the www group and add the ec2-user and apache users to it.

```
sudo groupadd www
sudo usermod -a -G www ec2-user
sudo usermod -a -G www apache
exit
```

Reconnect and check permissions.

```
groups
# should show "ec2-user wheel www"
```

Set better ownership and permissions on the /var/www directory, sub-directories, and files.

```
sudo chown -R apache:www /var/www
sudo chmod 2775 /var/www
find /var/www -type d -exec sudo chmod 2775 {} \;
find /var/www -type f -exec sudo chmod 0664 {} \;
sudo service httpd restart
```


## Test Apache/PHP

Test and inspect the Apache/PHP installation.

```
echo "<?php phpinfo(); ?>" > /var/www/html/pi.php
```

Then, in web browser, go to http://public.dns.hostname/pi.php to verify that Apache and PHP are working and have the proper packages and modules installed and enabled.


## Cleanup `httpd.conf`

```
ServerRoot "/etc/httpd"
Listen 80

#
# Load required moduled before doing anything else.
#
Include conf.modules.d/*.conf

User apache
Group apache

#
# Deny access to the entire filesystem.
#
<Directory />
  AllowOverride none
  Require all denied
</Directory>

#
# Relax access to content within /var/www.
#
<Directory "/var/www">
  AllowOverride None
  Require all granted
</Directory>

#
# DirectoryIndex: sets the file that Apache will serve if
# a directory is requested.
#
<IfModule dir_module>
  DirectoryIndex index.html
</IfModule>

#
# Deny access to .ht* files
#
<Files ".ht*">
  Require all denied
</Files>


ServerName test.example.com
DocumentRoot "/var/www/html"


#
# Further relax access to the default document root:
#
<Directory "/var/www/html">
  Options -Indexes +FollowSymLinks
  AllowOverride All
  Require all granted
</Directory>

LogLevel warn
ErrorLog "logs/error_log"

#<IfModule log_config_module>
#  LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
#  LogFormat "%h %l %u %t \"%r\" %>s %b" common
#
#  <IfModule logio_module>
#    # You need to enable mod_logio.c to use %I and %O
#    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
#  </IfModule>
#
#  #CustomLog "logs/access_log" common
#  CustomLog "logs/access_log" combined
#</IfModule>

TypesConfig /etc/mime.types

#
# Compress text-based content.
# Apache already sends the `Vary:Accept-Encoding` header
# for any gzipped, responses, so we don't need to add that.
#
AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
SetInputFilter DEFLATE

#
# Specify a default charset for all content served; 
# this enables interpretation of all content as UTF-8
# by default.  To use the default browser choice (ISO-8859-1),
# or to allow the META tags in HTML content to override this
# choice, comment out this directive:
#
AddDefaultCharset UTF-8

<IfModule mime_magic_module>
  #
  # The mod_mime_magic module allows the server to use
  # various hints from the contents of the file itself to
  # determine its type.  The MIMEMagicFile directive tells
  # the module where the hint definitions are located.
  #
  MIMEMagicFile conf/magic
</IfModule>

#
# Customizable error responses come in three flavors:
# 1) plain text 2) local redirects 3) external redirects
#
# Some examples:
#ErrorDocument 500 "The server made a boo boo."
#ErrorDocument 404 /missing.html
#ErrorDocument 404 "/cgi-bin/missing_handler.pl"
#ErrorDocument 402 http://www.example.com/subscription_info.html
#

#
# EnableMMAP and EnableSendfile: On systems that support it,
# memory-mapping or the sendfile syscall may be used to deliver
# files.  This usually improves server performance, but must
# be turned off when serving from networked-mounted
# filesystems or if support for these functions is otherwise
# broken on your system.
# Defaults if commented: EnableMMAP On, EnableSendfile Off
#
#EnableMMAP off
EnableSendfile on

#
# Make apache and php version details less obvious.
#
ServerTokens Prod
ServerSignature Off

# Supplemental configuration
#
# Load config files in the "/etc/httpd/conf.d" directory,
# if any.
IncludeOptional conf.d/*.conf

#
# Include our own configurations additions.
# We're using `IncludeOptoinal` instead of `Include` so that
# the files can easily be renamed or deleted in order to
# disable that functionality.
#
# Since we're not using vhosts anymore, and therefore the
# functionality in these `conf.include/*` files is no longer
# being repeated in different vhosts, we should probably retire
# the include files and rather include those directives
# directly in this conf file.
#
#Include conf.include/forbidden-locations.conf
#Include conf.include/force-http2https.conf
IncludeOptional conf.include/forbidden-locations.conf
IncludeOptional conf.include/force-http2https.conf


#--#
#--# Add common types not included from the call to
#--# `TypesConfig /etc/mime.types`.
#--#

#
# Support compressed file types.
#
# For these, I'm not sure if they should also/instead be
# specified as `AddEncoding` directives.
#
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz

#
# Support webfont files with the proper mime types.
#
AddType application/vnd.ms-fontobject .eot
AddType application/x-font-ttf .ttf
AddType application/x-font-woff .woff

#
# Deny access to these locations
# 
<LocationMatch "/(\.git|ddl|log|cache)/">
  Require all denied
</LocationMatch>
```


## Slim-down Apache's per-process footprint

```
sudo mv conf.modules.d conf.modules.d.disabled
sudo mkdir conf.modules.d
```

Then create a single module-loading conf file: `conf.modules.d/00-required.conf`.

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

#
# socache_memcache_module to support memcached
# socache_shmcb_module to support shmcb for tls
#
LoadModule socache_memcache_module modules/mod_socache_memcache.so
LoadModule socache_shmcb_module modules/mod_socache_shmcb.so

#
# filter_module to enable input and output filtering
# deflate_module to enable the gzip/deflate filters
#
LoadModule filter_module modules/mod_filter.so
LoadModule deflate_module modules/mod_deflate.so

#
# More...
#
LoadModule log_config_module modules/mod_log_config.so
LoadModule rewrite_module modules/mod_rewrite.so
LoadModule alias_module modules/mod_alias.so
```

As far as I can tell, these are the only modules we need... so far. We'll see what else specifically is needed by phpMyAdmin and WordPress.

The `log_config_module` can probably be removed as well since we're not using any custom log lines.


## Clean out other unnecessary configuration data

```
sudo mv conf.d conf.d.disabled
sudo mkdir conf.d
```

Then, add the following files to `conf.d`:

### `conf.d/10-php56.conf`:

```
#
# Add index.php as a directory index.
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

### `conf.d/50-server-status.conf.disabled`:

Remove ".disabled" from this filename and restart apache at moments when you do want to see apache server-status and server-info.

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


## Timezone

Set timezone value in `php.ini`. In my case, I set it to `America/Denver` because that's generally where we want to "think" our servers are, without regard for where they actually are. The new `php.ini` line:

```
date.timezone = America/Denver
```

Set server time zone as well.

```
sudo rm -rf /etc/localtime
sudo ln -s /usr/share/zoneinfo/America/Denver /etc/localtime
```

## php.ini - Default -> Production

Add/change these values in php.ini

```
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
display_errors = Off
display_startup_errors = Off
track_errors = Off
mysqlnd.collect_memory_statistics = Off
```


## Setup HSTS in Apache, maybe

I don't see an absolutely definitive guide for HSTS, so I haven't enabled it yet. I need to look at this some more. There may even be some privacy concerns caused by servers using HSTS.

* [How to configure HTTP Strict Transport Security (HSTS) on Apache & NGINX | IT Igloo](http://itigloo.com/security/how-to-configure-http-strict-transport-security-hsts-on-apache-nginx/)
* [HTTP Strict Transport Security for Apache, NGINX and Lighttpd - Raymii.org](https://raymii.org/s/tutorials/HTTP_Strict_Transport_Security_for_Apache_NGINX_and_Lighttpd.html)
* [HTTP Strict Transport Security - OWASP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security)
* [HSTS (HTTP Strict Transport Security) for Apache/Nginx](http://linux-audit.com/configure-hsts-http-strict-transport-security-apache-nginx/)

This seems to be a more "definitive" guide to HSTS, or at least `includeSubDomains` and `preload` are explained in terms that I understand.

[HTTP Strict Transport Security](https://https.cio.gov/hsts/)

I think we'll add 

```
Header always set Strict-Transport-Security "max-age=31536000"
```

as the simplest implementation to our https virtual host. If we get around to submitting a site to google's/chrome's preload list, we can add `includeSubDomains` and `preload` to the header.


## Cleanup


```
mv /var/www/cgi-bin /var/www/cgi-bin.disabled
mv /var/www/icons /var/www/icons.disabled
mv /var/www/noindex /var/www/noindex.disabled
```

Sometime later, delete the `/var/www/` `.disabled` directories.


## More...

### More... cleanup

Plan on deleting the `.disabled` directories from `/etc/httpd/conf`.

### More... op cache and key/value caching

Not sure yet what exactly to install for Apache or configure for PHP to have reasonable caching alternatives pre-installed and available in this AMI.


### More... HTTP/2

Investigate and understand configuration of spdy/http2 and/or quic, or something else?

chrome://net-internals/#http2

[chrome net-internals spdy http2 - Google Search](https://www.google.co.jp/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=chrome+net-internals+spdy+http2)


## Create an AMI for this baseline

Use AWS console to create an AMI from this instance. Set the name of the ami and its snapshot to _lap-baseline_.


## When launching a new instance from this AMI...

* Remember there is a `pi.php` script in `/var/www/html` which shows `phpinfo()` output. Don't forget to delete it or at least give it a secret 

* First command after launch: `sudo yum update`.
* Then change the friendly hostname in .bash_profile PS1.

