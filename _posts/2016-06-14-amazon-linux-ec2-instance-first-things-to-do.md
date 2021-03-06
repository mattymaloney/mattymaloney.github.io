---
layout: post
title: First Things to Do for Every New Amazon Linux EC2 Instance
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
echo 'alias ll="LC_ALL=C ls -lahF"' >> ~/.bashrc
echo 'alias rm="rm -i"' >> ~/.bashrc
echo 'alias cp="cp -i"' >> ~/.bashrc
echo 'alias mv="mv -i"' >> ~/.bashrc
echo 'alias view="vim -R"' >> ~/.bashrc
```

And for `root` (this is only effective for interactive bash):

```
echo '' | sudo tee -a /root/.bashrc
echo '# Custom bash prompt via kirsle.net/wizards/ps1.html' | sudo tee -a /root/.bashrc
echo 'export PS1="\[$(tput sgr0)\]\n\[$(tput setaf 3)\]\u\[$(tput setaf 0)\]\[$(tput bold)\]@\[$(tput sgr0)\]\[$(tput setaf 3)\]\h \[$(tput setaf 7)\][\[$(tput setaf 5)\]aws-host\[$(tput setaf 7)\]]\n\[$(tput setaf 2)\]\w\n\[$(tput setaf 0)\]\[$(tput bold)\]\\$ \[$(tput sgr0)\]"' | sudo tee -a /root/.bashrc
echo '' | sudo tee -a /root/.bashrc
echo 'alias ll="LC_ALL=C ls -lahF"' | sudo tee -a /root/.bashrc
echo 'alias rm="rm -i"' | sudo tee -a /root/.bashrc
echo 'alias cp="cp -i"' | sudo tee -a /root/.bashrc
echo 'alias mv="mv -i"' | sudo tee -a /root/.bashrc
echo 'alias vi="vim"' | sudo tee -a /root/.bashrc
echo 'alias view="vim -R"' | sudo tee -a /root/.bashrc
```

## Better Way to Affect bash Prompt

Create the file `/etc/profile.d/zzz-cc-www-box.sh` with these contents:

```
export boxenv='aws-test'
export boxtag='application-service-version'
export boxnum=''

# Custom bash prompt via kirsle.net/wizards/ps1.html
export PS1="\[$(tput sgr0)\]\n\[$(tput setaf 3)\]\u\[$(tput setaf 0)\]\[$(tput bold)\]@\[$(tput sgr0)\]\[$(tput setaf 3)\]\h \[$(tput setaf 7)\][\[$(tput setaf 5)\]$boxenv/$boxtag #$boxnum\[$(tput setaf 7)\]]\n\[$(tput setaf 2)\]\w\n\[$(tput setaf 4)\]\$ \[$(tput sgr0)\]"

alias ll="LC_ALL=C ls -lahF --color=auto"
alias rm="rm -i"
alias cp="cp -i"
alias mv="mv -i"
alias vi="vim"
alias view="vim -R"
```

## Create own ssh keys

```
ssh-keygen
```

Accept all defaults.


## Reboot

```
sudo reboot
```


## Security Modifications

In decreasing levels of complexity and detail, here are 3 related aproaches to securing a new Ubuntu installation. Most of this just needs mild modification to be useful for Amazon Linux instances. Although, I'm ignoring most of the firewall recommendations, as I'm relying almost exclusively on Amazon Security Groups.

* [2016 Guide to User Data Security | Inversoft](https://www.inversoft.com/guides/2016-guide-to-user-data-security)
* [My First 10 Minutes On a Server - Primer for Securing Ubuntu](http://www.codelitt.com/blog/my-first-10-minutes-on-a-server-primer-for-securing-ubuntu/)
* [My First 5 Minutes On A Server; Or, Essential Security for Linux Servers](https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)


This is codelitt's repo where they've opensourced their thought processes and best practices. Codelitt itself is of questionable value, but this is a good collection of commonly asserted best practices for security and restful nights.

[codelittinc/incubator-resources: Processes, policies, and tools for startups and product teams. All open sourced and open for contribution.](https://github.com/codelittinc/incubator-resources)

