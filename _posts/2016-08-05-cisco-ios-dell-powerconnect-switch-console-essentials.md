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

## New Firmware

[Dell PowerConnect 5424, PowerConnect 5448, v.V2.0.0.46, A10 Driver Details | Dell US](http://www.dell.com/support/home/us/en/04/Drivers/DriversDetails?driverId=777T5)


```
enable
copy tftp://192.168.2.47/powerconnect_54xx-20046.ros image
copy tftp://192.168.2.47/powerconnect_54xx_boot-2000.rfb boot
boot system image-2
show bootvar
reload
```

Use `show bootvar` to show indication that image-2 is the uploaded firmware version, that it's not active (of course), and that it has an '*' next to it, indicating that it will be active on the next boot.

### TFTP Server

http://www.extraputty.com/features/tftp.html

I tried to use ExtraPuTTY first. I see how to configure the TFTP server; that's intuitive enough. I don't see how to start the server, and I didn't find any quick answers online. Must be intuitive for everyone else.


http://downloads.solarwinds.com/solarwinds/Release/FreeTool/SolarWinds-TFTP-Server.zip

After getting the windows firewall issues worked out, the SolarWinds TFTP Server seems to work very well.


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
exit

enable
configure
hostname good-hostname
username admin password GoodPassword level 15
exit
exit

enable
copy running-config startup-config
reload
```

To see configured user accounts:

```
enable
show users accounts
exit
```


### NIST Time Servers

[NIST Internet Time Service](http://tf.nist.gov/tf-cgi/servers.cgi)



## iSCSI Setup

This is a management network which mostly contains iSCSI traffic and inter-server communications traffic.

Following the advice at [Configuring a PowerConnect 5424 or 5448 Switch for use with an iSCSI storage system - Storage - Wiki - Storage - Dell Community](http://en.community.dell.com/techcenter/storage/w/wiki/2721.configuring-a-powerconnect-5424-or-5448-switch-for-use-with-an-iscsi-storage-system):

```
enable
configure
port jumbo-frame
spanning-tree mode rstp
interface range ethernet g1-24
flowcontrol on
spanning-tree portfast
exit
--no-- interface vlan 1
--no-- sntp client enable
--no-- ip address xxx.xxx.xxx.xxx 255.255.255.0
--no-- exit
--no-- ip default-gateway xxx.xxx.xxx.xxx
enable password level 15 GoodPassword
line telnet
password GoodPassword
exit
```

Remove VOIP traffic config commands.

```
voice vlan oui-table remove 0001e3
voice vlan oui-table remove 00036b
voice vlan oui-table remove 00096e
voice vlan oui-table remove 000fe2
voice vlan oui-table remove 0060b9
voice vlan oui-table remove 00d01e
voice vlan oui-table remove 00e075 
voice vlan oui-table remove 00e0bb
```

Suggested: remove iSCSI prioritization. Since we use this network for other inter-server communication, I'm not running these commands. These commands remove `iscsi enable` mode, I guess, but as far as I can tell, that feature/optimization is not enabled anyway.

```
no iscsi enable
no iscsi target port 860
no iscsi target port 860exit
```

Instead of those commands, I ran `iscsi enable`. Later, I'll get a look at our existing management switch to get a look at its configuration. I may need to configure something with the iscsi target. I don't know what that means yet.

```
enable
configure
iscsi enable
exit
exit
```

```
enable
copy running-config startup-config
reload
```

## Other Command-Line Guidance

* [Basic Command Line Interface on Dell Networking PowerConnect 2800 model switches | Dell US](http://www.dell.com/support/Article/us/en/19/HOW10655/EN)
* [PowerConnect Common Example Commands - PowerConnect Forum - Network Switches - Dell Community](http://en.community.dell.com/support-forums/network-switches/f/866/t/19445143)
* [Dell PowerConnect 5324 - Console cable tips, basic setup and configuration via Putty - YouTube](https://www.youtube.com/watch?v=LCF347c1uPc)
* [Using the Cisco console in Linux](http://useopensource.blogspot.jp/2007/01/using-cisco-console-in-linux.html)
* [Cisco IOS Command Line Interface: Introduction - Part 1 - YouTube](https://www.youtube.com/watch?v=DZf0niNLgXY)
* [Configuring a PowerConnect 5424 or 5448 Switch for use with an iSCSI storage system - Storage - Wiki - Storage - Dell Community](http://en.community.dell.com/techcenter/storage/w/wiki/2721.configuring-a-powerconnect-5424-or-5448-switch-for-use-with-an-iscsi-storage-system)
* [Dell Powerconnect 5424 basic configuration help! - Networking - Spiceworks](https://community.spiceworks.com/topic/306234-dell-powerconnect-5424-basic-configuration-help)
* [Cisco IOS CLI for beginners - Part 1 - YouTube](https://www.youtube.com/watch?v=-zvihHxrfzM)

Cisco Packet Tracer (network setup/configuration simulation)

* [Packet Tracer](http://www.cisco.com/web/learning/netacad/course_catalog/PacketTracer.html)
* [Download Packet Tracer | Cisco NetAcad](https://www.netacad.com/about-networking-academy/packet-tracer/)


## To Do for existing management 1.x switch

* examine config
* copy config to tftp server ([Configuration and Image Files: Dell PowerConnect 3324/3348 Switch CLI Guide](http://stuartconnections.com/products/Computers/Systems/Server/Switches/Dell_PowerConnect_3348/Command_Line_Interface_Guide/configim.htm#1035067))
* upgrade firmware
* set ip and enable web admin

