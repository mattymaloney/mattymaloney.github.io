---
published: true
layout: post
title: Instructions for Setting Up ssh Access via authorized_keys
---

Setting up authorized_keys access to boxes has always been a point of confusion. I manage to get it done, but it never feels clean. I probably don't protect the private key properly, and several times, I've generated the key pair on the server rather than the client.

It's time finally to write out my simple instructions for getting this done efficiently.

On the server:

Make sure that the user account you're connecting to has an `~/.ssh` directory with `700` permissions and an `authorized_keys` file with `600` permissions. The home directory itself, should also not be writable by any other users, so max permissions for `~` are `755`. It's possible you don't have to take these initial steps if you're using the `ssh-copy-id` command later in the process, but that command does not exist on my mac, so I need a bit more manual intervention.

While logged in as (or `su`'d as) the target user:

```
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

On the client:

Decide the filename of your private key. If you are making a general-use private key that you will use to connect to multiple ssh servers, then you can accept the default name (`~/.ssh/id_rsa`). However, if you need (or due lack of experience, wonder if you should use) different key pairs for different ssh servers, then choose a more specific name to prefix the key pair files. e.g. `~/.ssh/[server-name]_rsa`. When prompted later for the name of the private key file, accept the default (`id_rsa`) or enter another.

```
ssh-keygen -t rsa
```

I don't think the `-t rsa` portion of that command is necessary, as it seems to default to rsa anyway.

You'll be prompted for the filename in which to save the private key. Accept the default or change it as described above.

You'll also be prompted for a passphrase. We're not using a passphrase because we're mostly doing this for automation purposes rather than personal logins.

Now, we need to copy the public key to the server. This can be done via copy/paste (my usual method), with the `ssh-copy-id` command (which I haven't tried), or with a `cat` piped via `ssh` to the server's `cat`, redirecting output to your `authorized_keys` file.

```
ssh [username]@[host-or-ip-address] "cat >> ~/.ssh/authorized_keys"
```
---

See also:

[How To Set Up SSH Keys | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)
