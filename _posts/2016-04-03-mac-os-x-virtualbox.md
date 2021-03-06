---
published: true
title: Installing MAC OS X El Capitan (10.11.4) in a VirtualBox VM
layout: post
---

http://www.insanelymac.com/forum/topic/309654-run-vanilla-os-x-el-capitan-yosemite-or-mavericks-in-virtualbox-5010-on-a-windows-host/

This is the secret sauce. After creating the machine, increasing VRAM to 128mb and adding a second CPU, make these modifications to the vm.

```
VBoxManage.exe modifyvm "El_Capitan" --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff
VBoxManage setextradata "El_Capitan" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac11,3"
VBoxManage setextradata "El_Capitan" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
VBoxManage setextradata "El_Capitan" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Mac-F2238BAE"
VBoxManage setextradata "El_Capitan" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
VBoxManage setextradata "El_Capitan" "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1
```


To do this for macOS Sierra, see: 

* http://www.macworld.com/article/3092900/macs/how-to-create-a-bootable-macos-sierra-installer-drive.html
* http://www.wikigain.com/install-macos-sierra-10-12-virtualbox/

The insanelymac post referenced above also has an update at http://www.insanelymac.com/forum/topic/309654-run-vanilla-os-x-el-capitan-yosemite-or-mavericks-in-virtualbox-5010-on-a-windows-host/page-3#entry2247366 for macOS Sierra.

