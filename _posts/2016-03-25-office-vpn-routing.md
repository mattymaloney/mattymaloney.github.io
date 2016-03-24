---
published: true
layout: post
title: Specifying Proper WUbnet and Routing for PPTP VPN Connection
---

I've been enduring this problem where the office vpn (PPTP via Peplink with very old firmware) interface will only route properly to the /24 (255.255.255.0) network even though the real subnet is /21 (255.255.248.0).

As a result, I can contact other 192.168.2.x addresses, but I can't contact any other addresses between 192.168.0.0 and 192.168.7.254. Setting manual TCP/IP configuration with the proper netmask for the PPTP connection doesn't help. `ifconfig` still reports a netmask of 0xffffff00 (255.255.255.0) and `route get` still reports that contacting a 192.168.0.x address will still route through the `eth0` interface rather than the `ppp0` interface.

My amateur fix:

```
sudo ifconfig ppp0 inet 192.168.2.44 192.168.0.250 netmask 255.255.248.0
sudo route -n add 192.168.0.0 192.168.0.250 255.255.248.0
```

I found that using CIDR notation in the `ifconfig` and `route` commands were not producing the proper results, and I don't understand why there's a difference. All I know right now is that these commands are working for me right now.

Disconnecting and reconnecting the PPTP connection destroys both of these settings. I suppose I should understand more, such that I can have these settings applied whenever the PPTP connection is created. For now, I'll put them in a bash script to run manually after connecting.

Also, I don't exactly understand the purpose of `route`'s `-n` flag. In this case, the command seems to have the same result with and without the `-n` flag.