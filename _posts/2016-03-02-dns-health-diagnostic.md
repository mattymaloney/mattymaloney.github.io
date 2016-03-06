---
published: true
layout: post
title: Getting a DNS Server and Settings Diagnostic Health Check
---


[DNS check tool](http://dnscheck.pingdom.com/)

*"Test DNS servers and settings for a domain name."*

---

To query for SOA records, one of these commands should do:

```
host -t soa example.com
```

returns the SOA record.

```
dig soa example.com
```

adds the SOA record to the normal `dig` output.

---

```
dig example.com +nssearch
```

lists the SOA records on each NS in the zone. These SOA records should all be identical.

---

See also: [Find the DNS Authority Record for a DNS Domain (SOA)](http://www.cyberciti.biz/faq/find-dns-domain-start-of-authority-record/)

