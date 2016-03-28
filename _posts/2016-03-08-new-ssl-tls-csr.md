---
published: true
layout: post
title: Creating a New Private Key and Certificate Signing Request (CSR)
---





I do this rarely enough that I always have to look it up and be very careful that I'm doing exactly what I want to do.

Update for modern day, we want the signing request and the certificate to be signed with SHA-2 (SHA256).

## Check Prerequisites

Check if and what version of openssl is installed:

`openssl version` to get basic versioning and a date.

`rpm -qa | grep -i openssl` to get more detailed version info.


## Generate Private Key and CSR

```
openssl req -new -sha256 -newkey rsa:2048 -nodes -keyout example.com.key -out example.com.csr
```

Also interesting, apparently you can include the CSR details on the command line. e.g.

```
openssl req -new -newkey rsa:2048 -nodes -out www.example.com.csr -keyout www.example.com.key -subj "/C=US/ST=SC/L=Magic Hills/O=Dinglepie Berry Farms/CN=www.example.com"
```

## Check the CSR

```
openssl req -in www.example.com.csr -noout -text
```

---

See also:

* [Generate an OpenSSL Certificate Request with SHA256 Signature | IT Igloo](http://itigloo.com/security/generate-an-openssl-certificate-request-with-sha-256-signature/)

* [Apache SSL CSR Creation - OpenSSL Apache CSR | DigiCert.com](https://www.digicert.com/csr-creation-apache.htm)

* [Generate a CSR with OpenSSL](https://support.rackspace.com/how-to/generate-a-csr-with-openssl/)

* [CSR Generation: Using OpenSSL (Apache w/mod_ssl, NGINX, OS X) - Powered by Kayako Help Desk Software](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/1/19/)

* [The Most Common OpenSSL Commands](https://www.sslshopper.com/article-most-common-openssl-commands.html)

