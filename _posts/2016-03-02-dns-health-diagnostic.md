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

For `nslookup`, I don't think we can make an SOA query directly from the command line. Including starting an interactive `nslookup` session, we can use:

```
$ nslookup
> set q=soa
> example.com
```

This returns a parsed and labeled version of the SOA record as well as a listing of all the servers in the NS record and the resolved IP addresses of up to 4 of those name servers.


---

```
dig example.com +nssearch
```

lists the SOA records on each NS in the zone. These SOA records should all be identical.

---

See also: [Find the DNS Authority Record for a DNS Domain (SOA)](http://www.cyberciti.biz/faq/find-dns-domain-start-of-authority-record/)

