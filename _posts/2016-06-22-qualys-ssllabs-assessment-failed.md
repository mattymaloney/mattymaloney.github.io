---
layout: post
title: Qualys ssllabs.com returns "Unable to connect to the server" Error
---

I have to put off the rest of this investigation now, since the site actually seems to work for all clients. I have to assume, dangerously I realize, that the problem with the ssllabs.com assessment is actually a bug on their side. Despite the error, they are successfully connected to the server. The error occurs later in their scanning process. Various answers from other admins online lead me to believe that this error is rather generic and is produced whenever the ssllabs scan fails with an error that they haven't fully defined.

For future reference, when I look into this again, here are my scattered bookmarks:

* [Generate Mozilla Security Recommended Web Server Configuration Files](https://mozilla.github.io/server-side-tls/ssl-config-generator/)
* [tls - curl 'SSL connect error' - Super User](http://superuser.com/questions/1015134/curl-ssl-connect-error)
* [libcurl - Error Codes](https://curl.haxx.se/libcurl/c/libcurl-errors.html)
* [Namecheap.com Knowledgebase • Installing a SSL certificate on Apache](https://www.namecheap.com/support/knowledgebase/article.aspx/9423/0/installing-a-ssl-certificate-on-apache)
* [Namecheap.com Knowledgebase • Where do I find SSL CA Bundle? (SSL Installation, Installation, )](https://www.namecheap.com/support/knowledgebase/article.aspx/9393/69/where-do-i-find-ssl-ca-bundle)
* [Source address for whitelisting | Qualys Community](https://community.qualys.com/thread/10559)
* [Assessment failed: Unable to connect to the server · Issue #308 · ssllabs/ssllabs-scan](https://github.com/ssllabs/ssllabs-scan/issues/308)
* [/bin/bash based SSL/TLS tester: testssl.sh](https://testssl.sh/)
* [testssl-standard.jpg (935×1899)](https://testssl.sh/testssl-standard.jpg)
* [SSL Labs DROWN Test Implementation Details – Qualys Blog](https://blog.qualys.com/securitylabs/2016/03/04/ssl-labs-drown-test-implementation-details)
