These are the 4 options I'm aware of for mounting an S3 bucket in the Linux filesystem.

* [s3fs-fuse/s3fs-fuse: FUSE-based file system backed by Amazon S3](https://github.com/s3fs-fuse/s3fs-fuse)
* [kahing/goofys: a Filey System for Amazon S3 written in Go](https://github.com/kahing/goofys)
* [skoobe/riofs: Userspace S3 filesystem](https://github.com/skoobe/riofs)
* [danilop/yas3fs: YAS3FS (Yet Another S3-backed File System) is a Filesystem in Userspace (FUSE) interface to Amazon S3. It was inspired by s3fs but rewritten from scratch to implement a distributed cache synchronized by Amazon SNS notifications. A web console is provided to easily monitor the nodes of a cluster.](https://github.com/danilop/yas3fs)

I tried goofys, and didn't get it to work as easily as 3sfs. goofys apparently has better performance, so maybe it's worth more time, but I'm going with s3fs for now.

I haven't tried riofs.

I haven't tried YAS3F3 yet, but it's interesting. We may be able to use the SNS notifications to automatically invalidate CDN resources when their S3 origin objects are updated.

From SO, this is the page that introduced me to s3fs. [FTP/SFTP access to an Amazon S3 Bucket - Stack Overflow](http://stackoverflow.com/questions/23939179/ftp-sftp-access-to-an-amazon-s3-bucket)

---

## Here's My Setup

Mostly just following the instructions at [s3fs-fuse/s3fs-fuse: FUSE-based file system backed by Amazon S3](https://github.com/s3fs-fuse/s3fs-fuse).

New Amazon Linux instance.

```
sudo yum update
sudo yum install automake fuse fuse-devel gcc-c++ git libcurl-devel libxml2-devel make openssl-devel
git clone https://github.com/s3fs-fuse/s3fs-fuse.git
cd s3fs-fuse
./autogen.sh
./configure
make
sudo make install
```

Add this line to `/etc/fstab`

```
s3fs#bucket-name /mnt/folder-name fuse _netdev,allow_other,umask=077,uid=sftp-user,gid=www 0 0
```

This makes `stfp-user` appear to be the owner and `www` appear to be the group for all files and folders. All files and folders end up appearing with permissions mode `700`. If user and group should both have access, then umask would be `007`. See `man fuse` for an explanation of why umask is the opposite of the permissions mode you'd use on the command line.

It's not clear to me yet what the `_netdev` and `allow_other` options do.

Test the `/etc/fstab` line with `sudo mount -a`.

Because we're doing this to create a gateway service allowing sftp clients to access an s3 bucket, we need to create an sftp-only user as well. See [Setup an sftp-only User in Linux](https://github.com/mattymaloney/mattymaloney.github.io/blob/master/_posts/2016-05-23-linux-sftp-only-user.md).

## To Do

Expand this document to describe the complete setup of the pub.site.com box.

## Do the Mounting

ACCESS_KEY_ID and SECRET_KEY are those belonging to an IAM user with read/write access to the S3 bucket.

```
mkdir /mnt/folder-name
echo ACCESS_KEY_ID:SECRET_KEY | sudo tee /etc/passwd-s3fs
sudo chmod 600 /etc/passwd-s3fs
sudo mount -a
```

