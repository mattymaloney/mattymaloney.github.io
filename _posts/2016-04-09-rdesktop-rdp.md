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
[How to use rdesktop (Windows remote desktop connection)](http://ubuntuforums.org/showthread.php?t=824710)

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

###

In summary, a little buggy, like basically every non-server component in Linux.
