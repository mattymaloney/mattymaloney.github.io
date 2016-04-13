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
- Security Group: Select or create one with the following settings:
	- port 22 from office and home IPs.
    - ports 80 and 443 from all.

Make sure that this EC2 instance's security group is added to the permitted ingress traffic for the RDS instance's security group.

Edit `/etc/httpd/conf/httpd.conf` to update the `ServerName` directive.

Restart Apache.


Verify now that you can connect to the database using wordpress's username and password.

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
