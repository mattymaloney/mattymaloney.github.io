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


## 2

Run updates and install apache/php as well as the php mysql native driver and the mysql client cli. Start httpd and set it to start automatically when the server boots.

```
sudo yum update -y
sudo yum install -y httpd24 php56 php56-mysqlnd mysql56
sudo service httpd start
sudo chkconfig httpd on
```

At this point, we should be able to see the server using the public dns hostname shown in the ECs Management Console.

