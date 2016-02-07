---
published: true
layout: post
title: "Let's Encrypt"
---

While setting up ssl on a dev server, I got sidetracked by a quick foray into free SSL. Let's Encrypt appears to be a beast of complicated software with a huge dependency tree. That, along with the old CentOS (v5.3) is what killed this. Supporting older OS versions just isn't a realistic expectation for the Let's Encrypt project.

Pages I used to get the software, read the instructions, and help with troubleshooting. 

* [Entering Public Beta - Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/2015/12/03/entering-public-beta.html)
* [User Guide — Let's Encrypt 0.4.0.dev0 documentation](https://letsencrypt.readthedocs.org/en/latest/using.html#installation)
* [letsencrypt/letsencrypt: This Let's Encrypt repo is an ACME client that can obtain certs and extensibly update server configurations (currently supports Apache on .deb based systems, nginx support coming soon)](https://github.com/letsencrypt/letsencrypt#understanding-the-client-in-more-depth)
* [kuba/simp_le: Simple Let's Encrypt Client](https://github.com/kuba/simp_le)
* [CentOS 6.7 + Python 2.7 errors on letsencrypt-auto run · Issue #1106 · letsencrypt/letsencrypt](https://github.com/letsencrypt/letsencrypt/issues/1106#issuecomment-150746851)
* [[SOLVED] Virtualenv : command not found - Client - Let's Encrypt Community Support](https://community.letsencrypt.org/t/solved-virtualenv-command-not-found/2127/8)
