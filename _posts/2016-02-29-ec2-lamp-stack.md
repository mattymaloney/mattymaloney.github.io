---
published: true
layout: post
title: EC2 LAMP Stack Implementation Options and Considerations
---



I was just starting to look into ways to improve our LAMP stack implementation on EC2, but I need to clean up and concentrate on some other tasks. This is what I found so far:

* [Install LAMP Stack, Download LAMP Stack](https://bitnami.com/stack/lamp/installer)
* [Bitnami AMP Stacks - Bitnami documentation](https://wiki.bitnami.com/Infrastructure_Stacks/Bitnami_AMP_Stacks)
* [Bitnami Sign in](https://bitnami.com/sign_in)
* [(2) Can anyone recommend a good, public AMI for a LAMP stack on Amazon's EC2? - Quora](https://www.quora.com/Can-anyone-recommend-a-good-public-AMI-for-a-LAMP-stack-on-Amazons-EC2)
* [(2) What are some tips for deploying and managing a LAMP stack on Amazon EC2? - Quora](https://www.quora.com/What-are-some-tips-for-deploying-and-managing-a-LAMP-stack-on-Amazon-EC2)
* [(2) What's a good, clean, Linux base AMI for AWS EC2? - Quora](https://www.quora.com/What-s-a-good-clean-Linux-base-AMI-for-AWS-EC2)
* [(2) What is the checklist for hosting a website on Amazon EC2 using LAMP stack? - Quora](https://www.quora.com/What-is-the-checklist-for-hosting-a-website-on-Amazon-EC2-using-LAMP-stack)
* [LAMP Stack Cloud Hosting, LAMP Stack Hosting - Installers and VM](https://bitnami.com/stack/lamp)

---

I also went back to basics a bit and started reading some amazon literature which does seem to have been updated since years ago when I started fiddling with EC2. I should read this stuff again to make sure I'm not making silly mistakes of oversight.

* [Tutorial: Installing a LAMP Web Server on Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html)
* [Getting Started with Amazon EC2 Linux Instances - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-launch-instance_linux)

---

Now, to actually set up an ec2 instance with apache and php, using mysql from an rds instance, I followed these guides:

* [Tutorial: Installing a LAMP Web Server on Amazon Linux - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html) (but didn't install or configure mysql server)

* [Tutorial: Configure Apache Web Server on Amazon Linux to use SSL/TLS - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-an-instance.html) (but just using a self-signed cert and amazon's default public dns name)



