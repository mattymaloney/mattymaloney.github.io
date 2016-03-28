---
published: true
layout: post
title: "Setting Up Apache, PHP, WordPress, phpMyAdmin on and AWS VPC with EC2 Linux and RDS MariaDB, Take 2"
---

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


## Create phpMyAdmin EC2 instance

Choosing:

- **OS/AMI**: Amazon Linux (ami-08111162)
- **Instance Type**: t2.nano (this is a necessary, but not-oft-used vm)
- **VPC**: the VPC that was create with the RDS instance
- **Availability Zone**: same that was chosen when creating the RDS instance
- **Enable termination protection**: yes
- Security Group: Select or create one with the following settings:
	- port 22 from office and home IPs.
    - ports 80 and 443 from all.

## 