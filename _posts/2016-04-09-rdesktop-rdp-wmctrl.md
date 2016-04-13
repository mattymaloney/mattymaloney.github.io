---
published: true
layout: post
title: Remote Desktop via RDP with rdesktop
---



After dealing with too many reliability (keyboard mostly) issues with the Fedora Xfce default RDP client (Vinagre Remote Desktop Viewer v3.18.2), I'm searching for a better solution. I came across rdesktop as one of the recommended alternatives. 

Project site:
[rdesktop: A Remote Desktop Protocol Client](http://www.rdesktop.org/)

Github:
[rdesktop/rdesktop: Unix client for Microsoft Remote Desktop Services.](https://github.com/rdesktop/rdesktop)

Some help (jeez this is old stuff):

* [How to use rdesktop (Windows remote desktop connection)](http://ubuntuforums.org/showthread.php?t=824710)
* [How to Use The Real Windows Remote Destop in Ubuntu](http://www.geekyprojects.com/tutorials/how-to-use-the-real-windows-remote-destop-in-ubuntu/)

---

## Example commands (mostly from the Ubuntu Forums post above)

#### Full screen, remote session gets all keys

`rdesktop -u USERNAME -fP SOMEIPORADDRESS`

Display remote desktop in full screen (`-f`) and bitmap cache is enabled for speed (`-P`). Pressing alt+tab doesn't switch local windows, it switches windows on the remote Windows desktop. And ctrl+alt+right (for switching to another workspace) won't work locally either. This is useful when you want alt+tab and othe special keystrokes to work affect only the remote Windows.


#### Full size, but not full screen

`rdesktop -u USERNAME -g 100% -PKD SOMEIPORADDRESS`

Display remote desktop in full screen (`-g 100%`) and bitmap cache is enabled for speed (`-P`). Keyboard shortcuts such as ctrl+alt+right and alt+tab work on the local desktop (`-K`). This is useful because you can put the remote desktop on the seperate workspace then you can switch between your local ubuntu workspace and your remote Windows desktop just by pressing ctrl+alt+right and ctrl+alt+left.

`-g 100%` uses 100% of the client window area, which generally excludes the loal OS taskbar. Unfortunately, I don't see the remote taskbar when using this mode, I think because it's hidden underneath the bottom edge of the screen.

`-D` removes the local OS's window decorations like title bar, edges, etc and let's the remote window fill that space.


####

`rdesktop SOMEIPORADDRESS -g 1200x950 -K -a 16 -u USERNAME -r disk:mylinuxdisk=/home/username`

`rdesktop -g 1152x864 -r disk:home=/home/username -r clipboard:CLIPBOARD -r sound:off -x l -P 192.168.1.200 -u "administrator@domain.local" -p password`


#### My command

`rdesktop -u matt 192.168.2.14 -a 15 -x -z -g 96% -PD -a 16 -x m -r clipboard:CLIPBOARD -r disk:homedir=/home/matty -r sound:off -K`

Using 96% makes the window the right size to fit underneath my taskbar.

`-g x%` uses x% of the client window area, which generally seems to exclude the local OS taskbar. Unfortunately, I don't see the remote taskbar when using this mode, I think because it's hidden underneath the bottom edge of the screen.

`-g XXXxYYY` puts the remote desktop in a window with the specified dimmensions.

`-D` removes the local OS's window decorations like title bar, edges, etc and let's the remote window fill that space.

`-P` cache bitmaps for performance, in exchange for some disk space.

`-a 16` color depth is 16 bits per pixel.

`-z` compress the data stream.

`-x m` remove all "extra" visual features from the remote display (animations, themes, etc).

`-0` console connection, but only for Server v2003+.

`-K` keep "special" key bindings available for the local OS -- don't send the "special" keys to the remote OS. I don't know what keys/combos are in the list of keystrokes not sent to the remote.

I'm using the command with `wmctrl`, like this: `wmctrl -s 2 & rdesktop -u matt 192.168.2.14 -a 15 -x -z -g 96% -PD -a 16 -x m -r clipboard:CLIPBOARD -r disk:homedir=/home/matty -r sound:off -K`, to switch to a specific desktop/workspace (#2 in this case) and then start the rdesktop session.

In addition to the `man` page, [wmctrl user documentation | spiralofhope](http://blog.spiralofhope.com/1042/wmctrl-user-documentation.html) is a good reference for `wmctrl`.


###

In summary, a little buggy, like basically every non-server component in Linux.

Consider using remmina instead.??

Yes. Remmina is much better actually. rdesktop has scaling issues, where it doesn't seem to know whether 100% means 100% of screen size or 100% of screen minus the taskbar. Also, the `-K` option is unreliable, as it doesn't work the same way on every invocation. Also, when using the `-f` option, the rdesktop window froze on one occasion, and I had no way that I knew of to get back to my desktop. Out of laziness and the hurry I was in, I did a hard reset for my PC. With some research, I may have been able to recover without the hard reset. At that point, I tried Remmina, which I actually like a lot.

Remmina is pretty great, but I'm not getting shared clipboard functionality for RDP connections. I've tried the freerdp/xfreerdp (library that Remmina depends upon) client directly, and I don't get shared clipboard in that envirnment either). I've read that I need the freerdp v2.0 library to fix this issue. Unfortunately, I don't understand how to make Remmina use the v2.0 library, and I'm having authentication errors trying to connect to RDP hosts to which I can connect without error using the older freerdp v1.2.0 libraries. Hmm.

Some resources:

- [Prebuilds · FreeRDP/FreeRDP Wiki](https://github.com/FreeRDP/FreeRDP/wiki/PreBuilds)
- [Compile on Fedora 20 · FreeRDP/Remmina Wiki](https://github.com/FreeRDP/Remmina/wiki/Compile-on-Fedora-20)

