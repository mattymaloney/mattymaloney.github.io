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


Or, to install this on a slim box on which you don't want dev tools, run this from an NFS or other shared directory that contains the compiled binaries.

```
/bin/mkdir -p /usr/bin
/usr/bin/install -c  -m 755 rsync /usr/bin
/bin/mkdir -p /usr/share/man/man1
/bin/mkdir -p /usr/share/man/man5
if test -f rsync.1; then /usr/bin/install -c -m 644 rsync.1 /usr/share/man/man1; fi
if test -f rsyncd.conf.5; then /usr/bin/install -c -m 644 rsyncd.conf.5 /usr/share/man/man5; fi
```

The binaries that should have been copied from the source directory after running `make` are:

```
rsync
rsync.1
rsyncd.conf.5
```

Or, just run `rsync` directly from the shared directory.
