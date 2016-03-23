---
published: true
layout: post
title: Obtaining an OS X El Capitan ISO or Bootable USB Device
---

Apple is not providing an OS X El Capitan iso image -- not even for sale, as far as I can tell. Although they do allow download of the full installer app from the App Store.

I'm using these 2 resources to understand the makeup of the downloaded installer app and to create an ISO or bootable USB from it.

* [How to make a bootable OS X 10.11 El Capitan installer drive | Macworld](http://www.macworld.com/article/2981585/operating-systems/how-to-make-a-bootable-os-x-10-11-el-capitan-installer-drive.html)
* [HOW TO: Create a bootable El Capitan ISO fo VMware - OSx86 10.11 (El Capitan) - InsanelyMac Forum](http://www.insanelymac.com/forum/topic/308533-how-to-create-a-bootable-el-capitan-iso-fo-vmware/)

---

This seems to have worked, but I'm still running the install.

```
 #!/bin/bash
 
 # Mount the installer image
 hdiutil attach /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/SharedSupport/InstallESD.dmg -noverify -nobrowse -mountpoint /Volumes/install_app
 
 # Create the ElCapitan Blank ISO Image of 7316mb with a Single Partition - Apple Partition Map
 hdiutil create -o /tmp/ElCapitan.cdr -size 7316m -layout SPUD -fs HFS+J
 
 # Mount the ElCapitan Blank ISO Image
 hdiutil attach /tmp/ElCapitan.cdr.dmg -noverify -nobrowse -mountpoint /Volumes/install_build
 
 # Restore the Base System into the ElCapitan Blank ISO Image
 asr restore -source /Volumes/install_app/BaseSystem.dmg -target /Volumes/install_build -noprompt -noverify -erase
 
 # Remove Package link and replace with actual files
 rm /Volumes/OS\ X\ Base\ System/System/Installation/Packages
 cp -rp /Volumes/install_app/Packages /Volumes/OS\ X\ Base\ System/System/Installation/
 
 # Copy El Capitan installer dependencies
 cp -rp /Volumes/install_app/BaseSystem.chunklist /Volumes/OS\ X\ Base\ System/BaseSystem.chunklist
 cp -rp /Volumes/install_app/BaseSystem.dmg /Volumes/OS\ X\ Base\ System/BaseSystem.dmg
 
 # Unmount the installer image
 hdiutil detach /Volumes/install_app
 
 # Unmount the ElCapitan ISO Image
 hdiutil detach /Volumes/OS\ X\ Base\ System/
 
 # Convert the ElCapitan ISO Image to ISO/CD master (Optional)
 hdiutil convert /tmp/ElCapitan.cdr.dmg -format UDTO -o /tmp/ElCapitan.iso
 
 # Rename the ElCapitan ISO Image and move it to the desktop
 mv /tmp/ElCapitan.iso.cdr ~/Desktop/ElCapitan.iso
 ```
