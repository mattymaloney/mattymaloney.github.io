---
published: true
layout: post
title: Creating a New Private Key and Certificate Signing Request (CSR)
---

I do this rarely enough that I always have to look it up and be very careful that I'm doing exactly what I want to do.

## Check Prerequisites

Check if and what version of openssl is installed:

`openssl version` to get basic versioning and a date.

`rpm -qa | grep -i openssl` to get more detailed version info.


## Generate Private Key and CSR

```
openssl req -new -newkey rsa:2048


---

Also interesting, apparently you can include the CSR details on the command line. e.g.

```
openssl req -new -newkey rsa:2048 -nodes -out www.example.com.csr -keyout www.example.com.key -subj "/C=US/ST=SC/L=Magic Hills/O=Dinglepie Berry Farms/CN=www.example.com"
```

