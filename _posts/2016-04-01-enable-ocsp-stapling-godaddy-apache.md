---
published: true
layout: post
title: Enabling OCSP Stapling in Apache for GoDaddy Certificates
---

I didn't find a write-up for enabling OCSP for GoDaddy certs, but this writeup for StartSSL certs is useful. We just need to download the full certificate chain from godaddy's respository as well.

[How To Configure OCSP Stapling on Apache and Nginx | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-ocsp-stapling-on-apache-and-nginx)

[GoDaddy's Certificate Repository](https://certs.godaddy.com/repository)

Actually though, the zip file that's issues with the certificate contains the full CA chain, complete with intermediate and root certificate. I delete the root certificate from this file for the ssl chain directive, but am using the full chain/bundle file for ocsp stapling.

In the end, I added these directives to the server configuration, outside of any virtual host.

```
SSLStaplingCache shmcb:/run/httpd/ssl_stapling_cache(128000)
SSLCACertificateFile /etc/httpd/tls/gd_bundle-g2-g1.crt
SSLUseStapling on
```
