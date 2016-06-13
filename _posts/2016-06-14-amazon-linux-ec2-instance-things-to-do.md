---
layout: post
title: Things to Do for Every New Amazon Linux EC2 Instance
published: true
---

Currently using 64-bit Amazon Linux AMI 2016.03. ([Release Notes](https://aws.amazon.com/amazon-linux-ami/2016.03-release-notes/))


## Get Updates

```
sudo yum udpate
```


## Set the Time Zone

[Setting the Time for Your Linux Instance - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html)

```
sudo sed -i 's/"UTC"/"America\/Denver"/' /etc/sysconfig/clock
sudo ln -sf /usr/share/zoneinfo/America/Denver /etc/localtime
sudo reboot
```


## Fix Locale Issues

Add 2 lines to the already existing but empty `/etc/environment`.

```
echo 'LANG=en_US.utf-8' | sudo tee -a /etc/environment
echo 'LC_ALL=en_US.utf-8' | sudo tee -a /etc/environment
```

See also: [Resolve Setting Locale Failed on Linux | Ri Xu Online](https://xuri.me/2015/09/06/resolve-setting-locale-failed-on-linux.html)

And for an alternative solution (modification for client pc): [MAC OS X – SSH LC_CTYPE Warning – EduRoll IT Blog](http://eduroll.eu/?p=119)


## Create better bash prompt and add cli aliases

```
echo '' >> ~/.bashrc
echo '# Custom bash prompt via kirsle.net/wizards/ps1.html' >> ~/.bashrc
echo 'export PS1="\[$(tput sgr0)\]\n\[$(tput setaf 3)\]\u\[$(tput setaf 0)\]\[$(tput bold)\]@\[$(tput sgr0)\]\[$(tput setaf 3)\]\h \[$(tput setaf 7)\][\[$(tput setaf 5)\]aws-host\[$(tput setaf 7)\]]\n\[$(tput setaf 2)\]\w\n\[$(tput setaf 0)\]\[$(tput bold)\]\\$ \[$(tput sgr0)\]"' >> ~/.bashrc
echo '' >> ~/.bashrc
echo 'alias ll="ls -lah"' >> ~/.bashrc
```

And for `root` (this is only effective for interactive bash):

```
echo '' | sudo tee -a /root/.bashrc
echo '# Custom bash prompt via kirsle.net/wizards/ps1.html' | sudo tee -a /root/.bashrc
echo 'export PS1="\[$(tput sgr0)\]\n\[$(tput setaf 3)\]\u\[$(tput setaf 0)\]\[$(tput bold)\]@\[$(tput sgr0)\]\[$(tput setaf 3)\]\h \[$(tput setaf 7)\][\[$(tput setaf 5)\]aws-host\[$(tput setaf 7)\]]\n\[$(tput setaf 2)\]\w\n\[$(tput setaf 0)\]\[$(tput bold)\]\\$ \[$(tput sgr0)\]"' | sudo tee -a /root/.bashrc
echo '' | sudo tee -a /root/.bashrc
echo 'alias ll="ls -lah"' | sudo tee -a /root/.bashrc
echo 'alias vi="vim"' | sudo tee -a /root/.bashrc
```

