---
published: true
layout: post
title: Apache SSL/HTTPS Setup
---



I always need a refresher when it's time to create a CSR and/or a self-signed certificate. I also always need to look up the basic httpd.conf directives for enabling SSL. This centos article is a very concise articulation of the process.

[HowTos/Https - CentOS Wiki](https://wiki.centos.org/HowTos/Https)

---

For deeper details about setting up the SSL ciphers to the desired levels of compatibility and security, see this Hynek post.

[Hardening Your Web Server’s SSL Ciphers · Homepage of Hynek Schlawack](https://hynek.me/articles/hardening-your-web-servers-ssl-ciphers/)

---

One more example with some different/additional details about some directives.

[Installing mod_ssl on apache: X.509,Certificate Authority,digital signatures explained ~ Technical Essentials](http://ramannanda.blogspot.jp/2009/08/installing-modssl-on-apache.html)

---

For configuing ciphers, consider browser compatibility.

[Template:TLS/SSL support history of web browsers - Wikipedia, the free encyclopedia](https://en.wikipedia.org/wiki/Template:TLS/SSL_support_history_of_web_browsers)

---

