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

LetsEncrypt has significant public backing right now, and I think that goes a long way to ensuring its near-future reliability.

LetsEncrypt apparently allows up to 100 domain names on each certificate. This makes it especially attractive for our internal app and all the alternate and subdomains that 3rd parties use to communicate with the app.

Let's Encrypt does have rate limits, but they appear to address certificate create rather than limits on client traffic for certificate validation. See [Rate Limits for Let's Encrypt](https://community.letsencrypt.org/t/rate-limits-for-lets-encrypt/6769).

A guide, some client scripts (because the official client may not be ideal), and a more comprehensive list of available clients:

* [Getting Started - Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/getting-started/)
* [Let’s Encrypt Clients – Simon Josefsson's blog](https://blog.josefsson.org/2015/12/17/lets-encrypt-clients/)
* [diafygi/letsencrypt-nosudo: Free HTTPS certificates without having to trust the letsencrypt cli with sudo/root](https://github.com/diafygi/letsencrypt-nosudo)
* [diafygi/acme-tiny: A tiny script to issue and renew TLS certs from Let's Encrypt](https://github.com/diafygi/acme-tiny/)
* [List of Client Implementations - Documentation - Let's Encrypt Community Support](https://community.letsencrypt.org/t/list-of-client-implementations/2103)

