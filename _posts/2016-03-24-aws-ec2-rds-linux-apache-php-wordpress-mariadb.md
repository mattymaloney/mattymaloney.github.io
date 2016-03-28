---
published: true
layout: post
title: "Setting Up Apache, PHP, WordPress on AWS EC2 Linux and RDS MariaDB"
---



Several takes on looking for the ideal setup of Apache/PHP with MariaDB on RDS.

The simplest explanation of my setup choices are described in these documents:

* [Tutorial: Installing a LAMP Web Server on Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html)
* [Tutorial: Hosting a WordPress Blog with Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/hosting-wordpress.html)

---

## 1

Create an Amazon Linux instance. Doesn't matter what size, but I used t2.micro because I'm only using this for testing and eventually to create an ami that I can reuse.

The security group should allow ssh, http, and https only.

I'm allowing ssh from only the office and my home ip addresses.

For this dev server that may temporarily contain content imported from production, I may also limit access via http and https to our office and my home ip addresses.


## 2 - Install services and 

Run updates and install apache/php as well as the php mysql native driver, the php mbstring extension, and the mysql client cli. Start httpd and set it to start automatically when the server boots.

```
sudo yum update -y
sudo yum install -y httpd24 php56 php56-mysqlnd php56-mbstring mysql56
sudo service httpd start
sudo chkconfig httpd on
```

At this point, we should be able to see the server's default apache "noindex" page over http using the public dns hostname shown in the ECs Management Console.


## 3 - Set DocumentRoot permissions

Set file permissions on the `/var/www` folder to allow the `ec2-user` and `apache` users to access and change the content.

Outside of wordpress installations, do we need the apache service to have modify access to the `/var/www` folder?

Create the `www` group and add the `ec2-user` and `apache` users to it.

```
sudo groupadd www
sudo usermod -a -G www ec2-user apache
exit
```

Reconnect and check permissions.

```
groups
# should show "ec2-user wheel www"
```

Set better ownership and permissions on the `/var/www` directory, sub-directories, and files.

```
sudo chown -R apache:www /var/www
sudo chmod 2775 /var/www
find /var/www -type d -exec sudo chmod 2775 {} \;
find /var/www -type f -exec sudo chmod 0664 {} \;
sudo service httpd restart
```

## 4 - Test Apache/PHP

Test and inspect the Apache/PHP installation.

```
echo "<?php phpinfo(); ?>" > /var/www/html/pi.php
```

Then, in web browser, go to http://public.dns.hostname/pi.php to verify that Apache and PHP are working and have the proper packages and modules installed and enabled.


## 5 - Apache SSL

Set up ssl for apache.
Tight ciphersuite.
Force http --> https.
Enable hsts.
Test at ssllabs.com.


## 6 - Create DB in new or existing MariaSB instance

If you already have a MariaDB RDS instance you want to reuse, simply add a new database to it.

Create an new MariaDB RDS instance, if necessary.

I'm using the t2 instance class because I want it to be available only to clients in the VPC. I haven't looked around enough yet to know how to make sure other instance types belong to my VPC and are only accessible to the VPC.

```
Multi-AZ: no
Storage: SSD
VPC: the same VPC to which the EC2 instance belongs
Publicly Accessible: no
Availability Zone: same zone to which the EC2 instance belongs
VPC Security Groups: create or reuse a group that allows 3306 traffic from your VPC private network.
DB Name: why not create first db while creating the rds instance.
Backup Retention: 21 days
```


## 7 - Install mysql client cli and php mysql native driver.

```
sudo yum install php56-mysqlnd mysql56
sudo service httpd restart
```

Check phpinfo again to verify `mysqlnd` installation.

## 8 - Create wordpress user and grant permissions.

Use mysql cli to connect to rds with root id. Create new user for this wordpress installation and assign privileges.

```
mysql -h db.instance.endpoint.rds.amazonaws.com -u root -p
```

In mysql, create the database (if not done while creating the rds instance), create the user (if not reusing a pre-existing user), and grant permissions for that user on the database. I'm granting permissions for the user from all hosts, as I'll rely on VPC security to limit which hosts can contact the db server.

```
create database `db_name`;
create user 'username' identified by 'awesome-password';
grant all privileges on `db_name`.* to 'username'@'%';
flush privileges;
exit;
```


## 9 - Install WordPress

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
cd ..
mv wordpress /var/www/
sudo chown -R apache:www /var/www
sudo chmod 2775 /var/www
find /var/www -type d -exec sudo chmod 2775 {} \;
find /var/www -type f -exec sudo chmod 0664 {} \;
```

Open `/etc/httpd/conf/httpd.conf` and make the edits for the default `DocumentRoot` and it's related `<Directory>` container, both of which should use `/var/www/wordpress`.

Now create/edit `vhosts.conf`. We don't need ot specify `DocumentRoot` because we want to use Apache's default document root which we've already set to `/var/www/wordpress`. This port-80 vhost only exists to forward the user to the port-443 vhost. The `vhosts.conf` file should look something like this:

```
<VirtualHost *:80>
  ServerName aws-test.sensiblebiz.com
  #DocumentRoot "/var/www/wordpress"

  # Because we're using the server_name variable below, be doubly sure that
  # apache will use the VirtualHost's ServerName value.
  UseCanonicalName On

  # Enable the Rewrite capabilities
  RewriteEngine On

  # Make sure the connection is not already HTTPS
  RewriteCond %{HTTPS} off

  # Redirect users from their original location, to the same location
  # but using HTTPS.
  # i.e.  http://www.example.com/foo/ to https://www.example.com/foo/
  # The leading slash is made optional so that this will work either
  # in httpd.conf or .htaccess context
  RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [R=301,QSA,L]
  #RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,QSA,L]
</VirtualHost>
```
sudo service httpd restart
```

Do wordpress installation script. In browser, go to http://public.dns.hostname/.

## 10 - Install phpMyAdmin

phpMyAdmin requires `mbstring`. Install that along with phpMyAdmin.

```
cd /var/www
sudo yum install php56-mbstring
wget --no-check-certificate https://files.phpmyadmin.net/phpMyAdmin/4.6.0/phpMyAdmin-4.6.0-english.tar.gz
tar xzf phpMyAdmin-4.6.0-english.tar.gz
rm phpMyAdmin-4.6.0-english.tar.gz
mv phpMyAdmin-4.6.0-english phpMyAdmin
sudo chown -R apache:www /var/www
sudo chmod 2775 /var/www
find /var/www -type d -exec sudo chmod 2775 {} \;
find /var/www -type f -exec sudo chmod 0664 {} \;
```

Put ssl certificate, key, and bundle somewhere, maybe in `/etc/pki/tls`, and enable the cert in the port-443 virtual host.

Setup port-80 and port-443 virtual hosts for phpMyAdmin. The port-80 virtual host will force traffic to the port-443 virtual host.

The phpMyAdmin port-80 virtual host should be placed after the `*:80` virtual host that we're using for the wordpress site, and it should look something like this:

```
<VirtualHost *:80>
  ServerName phpmyadmin.example.com
  DocumentRoot "/var/www/phpMyAdmin"

  ErrorLog logs/phpmyadmin-error_log
  #TransferLog logs/phpmyadmin-access_log
  LogLevel warn

  # Because we're using the server_name variable below, be doubly sure that
  # apache will use the VirtualHost's ServerName value.
  UseCanonicalName On

  # Enable the Rewrite capabilities
  RewriteEngine On

  # Make sure the connection is not already HTTPS
  RewriteCond %{HTTPS} off

  # Redirect users from their original location, to the same location
  # but using HTTPS.
  # i.e.  http://www.example.com/foo/ to https://www.example.com/foo/
  # The leading slash is made optional so that this will work either
  # in httpd.conf or .htaccess context
  RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [R=301,QSA,L]
  #RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,QSA,L]
</VirtualHost>
```

The port-443 virtual host for phpMyAdmin should come after the `*:443` virtual host that we're using for the wordpress site, and it should look something like this:

```
<VirtualHost *:443>
  ServerName phpmyadmin.example.com
  DocumentRoot "/var/www/phpMyAdmin"

  ErrorLog logs/phpmyadmin-ssl_error_log
  #TransferLog logs/phpmyadmin-ssl_access_log
  LogLevel warn

  SSLEngine on

  SSLProtocol all -SSLv2 -SSLv3

  SSLCipherSuite ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA

  SSLHonorCipherOrder on

  SSLCertificateFile /etc/pki/tls/certs/phpmyadmin.example.com.crt
  SSLCertificateKeyFile /etc/pki/tls/private/phpmyadmin.example.com.key
  SSLCertificateChainFile /etc/pki/tls/certs/bundle.crt

  #CustomLog logs/phpmyadmin-ssl_request_log \
  #          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
</VirtualHost>
```

Setup phpMyAdmin configuration, including configuration db and user.

Log into phpMyAdmin as root, and run this sql, after updating the password, outside of any DB context.

```
GRANT SELECT, INSERT, UPDATE, DELETE ON `phpmyadmin`.* TO 'pma_config'@'%'  IDENTIFIED BY 'pmapass';
```

Once again, we're allowing access for this user from any host, since we're relying on the vpc, and not allowing any mysql traffic from outside the vpc. In the future, if we do need to allow mysql traffic from outside the vpc, we'll do so by allowing traffic from specific IP addresses.

After updating the configuration name ('verbose'), rds endpoint ('host'), time zone ('SessionTimeZone'), the password ('password'), and adding/changing some random characters in the blowfish secret, put this file in `/var/www/phpMyAdmin`.

```
<?php
/*
 * Generated configuration file
 * Generated by: phpMyAdmin 4.6.0 setup script
 * Date: Sun, 27 Mar 2016 17:57:44 -0600
 */

/* Servers configuration */
$i = 0;

/* Server: ccb_dev [1] */
$i++;
$cfg['Servers'][$i]['verbose'] = 'rds-endpoint';
$cfg['Servers'][$i]['host'] = 'rds.endpoint.region.rds.amazonaws.com';
$cfg['Servers'][$i]['port'] = '';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['user'] = '';
$cfg['Servers'][$i]['password'] = '';
$cfg['Servers'][$i]['SessionTimeZone'] = 'America/Denver';
$cfg['Servers'][$i]['pmadb'] = 'phpmyadmin';
$cfg['Servers'][$i]['controluser'] = 'pma_config';
$cfg['Servers'][$i]['controlpass'] = 'pmapassword';
$cfg['Servers'][$i]['bookmarktable'] = 'pma__bookmark';
$cfg['Servers'][$i]['relation'] = 'pma__relation';
$cfg['Servers'][$i]['userconfig'] = 'pma__userconfig';
$cfg['Servers'][$i]['users'] = 'pma__users';
$cfg['Servers'][$i]['usergroups'] = 'pma__usergroups';
$cfg['Servers'][$i]['navigationhiding'] = 'pma__navigationhiding';
$cfg['Servers'][$i]['table_info'] = 'pma__table_info';
$cfg['Servers'][$i]['column_info'] = 'pma__column_info';
$cfg['Servers'][$i]['history'] = 'pma__history';
$cfg['Servers'][$i]['recent'] = 'pma__recent';
$cfg['Servers'][$i]['favorite'] = 'pma__favorite';
$cfg['Servers'][$i]['table_uiprefs'] = 'pma__table_uiprefs';
$cfg['Servers'][$i]['tracking'] = 'pma__tracking';
$cfg['Servers'][$i]['table_coords'] = 'pma__table_coords';
$cfg['Servers'][$i]['pdf_pages'] = 'pma__pdf_pages';
$cfg['Servers'][$i]['savedsearches'] = 'pma__savedsearches';
$cfg['Servers'][$i]['central_columns'] = 'pma__central_columns';
$cfg['Servers'][$i]['designer_settings'] = 'pma__designer_settings';
$cfg['Servers'][$i]['export_templates'] = 'pma__export_templates';
$cfg['Servers'][$i]['MaxTableUiprefs'] = 300;
$cfg['Servers'][$i]['tracking_version_auto_create'] = true;

/* End of servers configuration */

$cfg['blowfish_secret'] = '56f870090a8aa8.80789340';
$cfg['DefaultLang'] = 'en';
$cfg['ServerDefault'] = 1;
$cfg['UploadDir'] = '';
$cfg['SaveDir'] = '';
?>
```

Verify now that you can connect to the database using wordpress's username and password.

All done with phpMyAdmin.

##

