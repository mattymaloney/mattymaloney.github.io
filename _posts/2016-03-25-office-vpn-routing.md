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

---

When that wasn't working anymore, I tried this:

```
sudo route add 192.168.0.0/21 192.168.0.250
```

That command alone worked well, despite previous tests giving me troubles using this CIDR notation.

---

To see current routing table on Mac OS X:

```
netstat -rn
```

---

## Notes for Windows

To prevent all internet traffic from traversing the VPN, turn off "Use default gateway" in the VPN connection's IPv4 properties. See also: [networking - How can I disable 'Use default Gateway for remote networks' setting in Windows 10? - Super User](http://superuser.com/questions/954801/how-can-i-disable-use-default-gateway-for-remote-networks-setting-in-windows-1) for other ways to turn this feature off.

After split tunneling is enabled, connecting to the VPN may automatically route appropriate traffic through the VPN, while sending other traffic directly through the Ethernet or WiFi connection. However, if the VPN server is not giving your client the right mask or gateway information, see [networking - How to prevent Windows 7 using VPN gateway by default? - Super User](http://superuser.com/questions/861454/how-to-prevent-windows-7-using-vpn-gateway-by-default/862409#862409) to understand the `route` command. In one case for me, I needed a route command similar to the following:

```
route add 192.168.0.0/21 192.168.0.250 if 44
```

`if` specifies the interface's ID. Use `route print` to find it.

Use the `-p` flag to make this route persistent after reboots. I'm not using this flag because I only want this route active when I'm connected to the VPN.
