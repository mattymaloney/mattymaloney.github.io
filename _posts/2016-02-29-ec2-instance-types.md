---
published: true
layout: post
title: Choosing an EC2 Instance Type
---

I need some solid information about EC2 instance types. Up until now, I've been choosing rather blindly, judging instance suitability based only on instict.

Turns out, Amazon has a rather nice summary document laying out the intended roles of various instance groups and specific instances.

[Choosing the Right EC2 Instance Type for Your Application | AWS Official Blog](https://aws.amazon.com/blogs/aws/choosing-the-right-ec2-instance-type-for-your-application/)

### Updates/Additions

A webinar video from June 2016: [Choosing the Right EC2 Instance and Applicable Use Cases - AWS June 2016 Webinar Series - YouTube](https://www.youtube.com/watch?v=y-xicDOIiSQ)

A video from November 2014: [AWS re:Invent 2014 | (SDD406) Amazon EC2 Instances Deep Dive - YouTube](https://www.youtube.com/watch?v=ujGx0tiI1L4)

Three web pages enumerating EC2 instance types: 

* [EC2 Instance Pricing – Amazon Web Services (AWS)](https://aws.amazon.com/ec2/pricing/)
* [Amazon EC2 Instance Comparison](http://www.ec2instances.info/?cost=monthly)
* [EC2 Instance Types – Amazon Web Services (AWS)](https://aws.amazon.com/ec2/instance-types/)

---

For insight into changing an instance's type, see:
* [How to safely upgrade an Amazon EC2 instance from t1.micro to large? - Stack Overflow](http://stackoverflow.com/questions/5898308/how-to-safely-upgrade-an-amazon-ec2-instance-from-t1-micro-to-large).
* [Resizing Your Instance - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html)
* [Changing the Instance Size of Your Reservations - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-modification-instancemove.html)
* [Moving an EC2 Instance to a Larger (or Smaller) Instance Type · Alestic.com](https://alestic.com/2011/02/ec2-change-type/)

