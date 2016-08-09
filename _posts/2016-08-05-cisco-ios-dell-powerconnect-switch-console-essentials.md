## Cable selection

Here's some technical detail about the console cables, and their pin-outs, for Dell switches and routers. I think all the pin-out information applies to Cisco devices as well.

* [What type of console cables are used for Dell PowerConnect, N Series, and Force10 switches | Dell US](http://www.dell.com/Support/Article/us/en/19/QNA43618)

I think this is the cable I'd be using if our switches had RJ-45 console ports.

* [Ftdi USB to Serial / Rs232 Console Rollover Cable for Cisco Routers - Rj45](https://www.amazon.com/dp/B00M2SAKMG)

These 2 cables together is what I'm using to connect from a USB port to a male serial console port on a Dell PowerConnect 5424:

* [TRENDnet TU-S9 USB to Serial Converter](https://www.amazon.com/dp/B0007T27H8)
* [Tripp Lite Null Modem Serial RS232 Cable (DB9 F/F) 6-ft. (P450-006)](https://www.amazon.com/dp/B000067SCH) (I'm not positive this is the same cable, as I didn't buy it here)

This is the cable I'd be using if I had a PC with an actual serial port (or a USB->serial port) and a switch or router with an RJ-45 console port. I think this is essentially the same skyblue cable that comes with switches:

* [Cisco Console Cable RJ45-to-DB9](https://www.amazon.com/dp/B000GL3MOY/)

---

## Switch config commands

See also: [PowerConnect Common Example Commands - PowerConnect Forum - Network Switches - Dell Community](http://en.community.dell.com/support-forums/network-switches/f/866/t/19445143).

```
enable
delete startup-config
reload

enable
configure
clock source sntp
clock timezone -7 zone MST
clock summer-time recurring usa zone MDT
sntp unicast client enable
sntp server 198.60.73.8
exit
exit

enable
configure
interface vlan 1
ip address 192.168.1.201 /21
exit
ip default-gateway 192.168.0.250
exit

copy running-config startup-config
reload
```

## NIST Time Servers

[NIST Internet Time Service](http://tf.nist.gov/tf-cgi/servers.cgi)


## New Firmware

[Dell PowerConnect 5424, PowerConnect 5448, v.V2.0.0.46, A10 Driver Details | Dell US](http://www.dell.com/support/home/us/en/04/Drivers/DriversDetails?driverId=777T5)


## TFTP Server

http://www.extraputty.com/features/tftp.html

I tried to use ExtraPuTTY first. I see how to configure the TFTP server; that's intuitive enough. I don't see how to start the server, and I didn't find any quick answers online. Must be intuitive for everyone else.


http://downloads.solarwinds.com/solarwinds/Release/FreeTool/SolarWinds-TFTP-Server.zip

After getting the windows firewall issues worked out, the SolarWinds TFTP Server seems to work very well.


## Other Command-Line Guidance

* [Basic Command Line Interface on Dell Networking PowerConnect 2800 model switches | Dell US](http://www.dell.com/support/Article/us/en/19/HOW10655/EN)
* [PowerConnect Common Example Commands - PowerConnect Forum - Network Switches - Dell Community](http://en.community.dell.com/support-forums/network-switches/f/866/t/19445143)

