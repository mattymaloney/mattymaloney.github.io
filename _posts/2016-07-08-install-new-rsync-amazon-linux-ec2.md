---
layout: post
title: Install a newer `rsync` in Amazon Linux AMI on EC2
---

I can't find an rpm that I understand well enough is compatible with Amazon Linux. I'm not sure which Fedora rpm I could use, if any.

```
sudo yum groupinstall "Development Tools"
wget https://download.samba.org/pub/rsync/src/rsync-3.1.2.tar.gz
tar xzf rsync-3.1.2.tar.gz
pushd rsync-3.1.2
./configure --prefix=/usr
make
sudo make install
popd
rm -f rsync-3.1.2.tar.gz
rm -rf rsync-3.1.2
```

Yay!
