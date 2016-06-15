---
layout: post
title: Using `dnsmasq` To Setup Private DNS for Development Environment
published: true
---

* [DNSMasq, your local development DNS · Edd Mann](http://eddmann.com/posts/dnsmasq-your-local-development-dns/)
* [DNS server in a development environment](https://www.adrikodde.nl/blog/2016/running-a-dns-server-in-a-development-environment)
* [Dnsmasq - network services for small networks.](http://www.thekelleys.org.uk/dnsmasq/doc.html)

---

## Setup DNS Server

Setting up dnsmasq in an AWS EC2 t2.nano machine was as simple as can be. I added some hosts to the hosts file, and like magic, I have a DNS server that directly answers queries for which it finds domains in the hosts file, and provides pass-through answers for any other queries. Couldn't be simpler.

---

## To temporarily change dns server settings on Mac OS X...

We need to choose and identify a network interface. To see the choices listed:

```
networksetup -listallnetworkservices
```

Output may look something like this:

```
An asterisk (*) denotes that a network service is disabled.
Ethernet
LTE Mobile Router
FireWire
Wi-Fi
Bluetooth PAN
vpn-connection-1
vpn-connection-2
```

In my case, I'm using the `Wi-Fi` interface right now. So, to temporarily change the dns servers for that interface (use the IP address for your dnsmasq server):

```
networksetup -setdnsservers Wi-Fi 111.222.111.222 
```

To remove the temp dns server:

```
networksetup -setdnsservers Wi-Fi Empty
```

To view current dns server settings:

```
networksetup -getdnsservers Wi-Fi
```

Note that the "normal" or default list of dns servers is empty. As far as I can tell, the OS uses the DNS servers configured with the network connection in Network Preferences unless those DNS server entries are replaced via `networksetup`.

Also worthy of clarification, the Network Preferences settings are per **connection** -- i.e. different wi-fi connections can have different DNS server settings. The `networksetup` dns servers are per **interface**, meaning that when they're applied to the Wi-Fi interface, those dns servers will be used for any wireless networks used via that interface.
