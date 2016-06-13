---
layout: post
title: Things to Do for Every New Amazon Linux EC2 Instance
published: true
---

## Get Updates

```
sudo yum udpate
```


## Set the Time Zone

[Setting the Time for Your Linux Instance - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html)

```
sudo sed -i 's/"UTC"/"America\/Denver"/' /etc/sysconfig/clock
sudo ln -sf /usr/share/zoneinfo/America/Denver /etc/localtime
```


## 
