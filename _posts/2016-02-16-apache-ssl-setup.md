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

Advice from google for migrating a site from HTTP to HTTPS, as well as links to more advice from google.

[Secure your site with HTTPS](https://support.google.com/webmasters/answer/6073543?hl=en)

[Make intra-site URLs relative | Web Fundamentals - Google Developers](https://developers.google.com/web/fundamentals/security/encrypt-in-transit/make-intra-site-urls-relative)

---

After we're satisfied with the HTTPS configuration, maybe a couple months in or more, we should enable HSTS as well.

[How to configure HTTP Strict Transport Security (HSTS) on Apache & NGINX | IT Igloo](http://itigloo.com/security/how-to-configure-http-strict-transport-security-hsts-on-apache-nginx/)

---

For an older article dicussing the speed of ssl and how to optimize it, see [5 easy tips to accelerate SSL – Unhandled expression](http://unhandledexpression.com/2013/01/25/5-easy-tips-to-accelerate-ssl/). But this is too old for the actual cipher suite recommendations to be useful.

---

More interesting recommendations about cipher suites.

[Strong SSL Security on Apache2 - Raymii.org](https://raymii.org/s/tutorials/Strong_SSL_Security_On_Apache2.html#The_Cipher_Suite)

---

Recommendations specific to Deploying Diffie-Hellman for TLS, "custom groups" and more that I don't completely understand. There's also reference here to Mozilla's "medium" cipher suite selection. Deploying the custom HL groups requires apache 2.4.7 or later.

[Logjam: PFS Deployment Guide](https://weakdh.org/sysadmin.html)

---

Mozilla's recommendations for SSL/TLS configuration:

[Security/Server Side TLS - MozillaWiki](https://wiki.mozilla.org/Security/Server_Side_TLS)

---

For details and insights about enabling forward secrecy:

[SSL Enabling Forward Secrecy | DigiCert.com](https://www.digicert.com/ssl-support/ssl-enabling-perfect-forward-secrecy.htm)

---

Some details about supported cipher suites in Internet Explorer versions back to IE7.

[IE Supported Cipher Suites · client9/sslassert Wiki](https://github.com/client9/sslassert/wiki/IE-Supported-Cipher-Suites)

---

Four online tools to evaluate SSL implementation:

* [SSL Server Test (Powered by Qualys SSL Labs)](https://www.ssllabs.com/ssltest/analyze.html)
* [mozilla/tls-observatory: An observatory for TLS configurations, X509 certificates, and more.](https://github.com/mozilla/tls-observatory)
* [jvehent/cipherscan: A very simple way to find out which SSL ciphersuites are supported by a target.](https://github.com/jvehent/cipherscan)
* [SSL Certificate Inspector | DigiCert.com](https://www.digicert.com/cert-inspector.htm)
* [SSL Analyzer and SSL Certificate Checker | COMODO](https://sslanalyzer.comodoca.com/)



---
