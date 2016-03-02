---
published: true
layout: post
title: Show the Active Samba (SMB) Shares in Linux
---

According to [command line - List samba shares and current users - Ask Ubuntu](http://askubuntu.com/questions/102924/list-samba-shares-and-current-users), there should be 3 ways to list the samba/smb shares defined on a linux box.

## `testparm`
This is the only one that works for me, and basically returns a compiled regurgitaion of the smb.conf file, I think.

## `net usershare info --long`
Produces "net usershare: usershares are currently disabled" response.

## `smbstatus --shares`
Returns nothing, implying that there are no shares.

However, in the case of this server, the shares do in fact exist and are successfully shared to windows boxes. So I am confused. 
