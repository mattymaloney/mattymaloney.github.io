---
layout: post
title: Validating an SSL/TLS Certificate, Key, and CSR
published: true
---

When you want to verify that a CR, private key, and certificate were all created by and for each other, try this:

```
(openssl req -in www.ccr.az.csr -noout -modulus;\
 openssl rsa -in www.ccr.az.key -noout -modulus;\
 openssl x509 -in www.ccr.az.crt -noout -modulus)\
 | uniq | wc -l
```

which should return `1`, as the modulus returned from each operation will be identical if the csr was created with the key, and the cert was inturn created with the csr.

See also: [Verifying that a Private Key Matches a Certificate](https://kb.wisc.edu/middleware/page.php?id=4064)
