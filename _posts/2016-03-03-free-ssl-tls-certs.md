---
published: true
layout: post
title: Free SSL/TLS Certificates
---

So far, I see 3 ways to get free SSL certificates.

## [CAcert.org](http://www.cacert.org/)

As of March 2nd, 2016, https://www.cacert.org/ produces the error "This certificate was signed by an untrusted issuer". Chrome's Security Overview states "There are issues with the site's certificate chain (net::ERR_CERT_AUTHORITY_INVALID).".

This is probably related to the news item [Re-Signing Root Certificate | CAcert Blog](http://blog.cacert.org/2015/12/re-signing-root-certificate/), which has apparently been postponed.

Conclusion: ccert.org is not a good option.


## [StartSSL™ Certificates & Public Key Infrastructure](https://www.startssl.com/Support?v=39)

I haven't looked very closely, as I'm more interested in LetsEncrypt right now.


## [Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/)

