---
published: true
layout: post
title: "Workstation's Trust Relationship with the Primary Domain has Failed"
---

I'm stuck with this issue where "The trust relationship between this workstation and the primary domain failed" for a PC that we've brought back from the dead. When this PC was sent to its grave, a new PC took its place. The new PC inherited the old PC's computername and staticly assigned IP address.

When bringing this PC back to life, it caused a duplicate IP issue. That was resolved, with physical presence at the console, by logging onto the computer with a domain ID (with the old password for that domain account) and changing the IP settings to use DHCP.

Next issue is that we can't log onto the computer at all. I may not have the correct local administrator account password, but I do have the users' old domain passwords. No dice though, as "The trust relationship between this workstation and the primary domain failed" is killing all login attempts.

I'll have to have someone disconnect the PC from the network and with physical presence, log onto the machine again to remove it from the domain.

See also:
[DON’T REJOIN TO FIX: The trust relationship between this workstation and the primary domain failed](http://implbits.com/active-directory/2012/04/13/dont-rejoin-to-fix.html)
A good resource for recreating the domain account, at least in cases where I have physical or remote access to the machine.
