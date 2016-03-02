---
published: true
layout: post
title: Determine Which Users Can Login Via SSH
---

Determine which users can login via SSH and with which method(s), password or authorized_keys, those users are permitted to connect.

## Which users have passwords?

First, look at `/etc/shadow` to see which users have passwords set. Valid passwords start with a `$` after the first `:`.

But, check `/etc/ssh/sshd_config` to be sure that empty passwords are not allowed. The `PermitEmptyPasswords` directive defaults to `no`, so if this directive is not set, then empty passwords are not allowed.

Also, to see if users are permitted to connect with passwords at all, find the `PasswordAuthentication` directive.

I think we also need to look at the `UsePAM` and `ChallengeResponseAuthentication` directives, but I'm not exactly sure what they mean yet.

Ideally, password authentication is disabled in favor or authentication by key pairs.

See also: [linux - SSH public key auth fails when UsePAM is set to "no" - Server Fault](http://serverfault.com/questions/475880/ssh-public-key-auth-fails-when-usepam-is-set-to-no)


## Which users have authorized_keys?

Check `/etc/ssh/sshd_config` for the `AuthorizedKeysFile` directive. This will tell you where `sshd` expects to find users' `authorized_keys` file(s). Apparently, the default is to look in `~/.ssh/authorized_keys` as well as `~/.ssh/authorized_keys2`.

After determining where `sshd` is looking for the public keys, look in that location in each users' `/home` directory to see which users have public keys stored on the server.


## Which users are allowed or disallowed by sshd_config

If `/etc/ssh/sshd_config` does not have an `AllowUsers` directive, then all users are allowed. Similarly, if it does not have a `DenyUsers` directive, then no users are disallowed.


## Look for `Match` Directives at the end of sshd_config

`Match User` and `Match Group` directives can override other directives for specific users and groups.

See also:

* [key management - Creating user specific authentication methods in SSH - Information Security Stack Exchange](http://security.stackexchange.com/questions/18036/creating-user-specific-authentication-methods-in-ssh)
* [linux - how to disable SSH login with password for some users? - Server Fault](http://serverfault.com/questions/285800/how-to-disable-ssh-login-with-password-for-some-users)

---

For more insight into trying to list all the ssh users, see also:

* [linux - Which users are allowed to log in via SSH by default? - Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/36804/which-users-are-allowed-to-log-in-via-ssh-by-default)
* [linux - List of all users that can connect via SSH - Stack Overflow](http://stackoverflow.com/questions/15802179/list-of-all-users-that-can-connect-via-ssh)

But neither of these references are actually a complete answer.

Seems that sshd could use a utility command that will explicitly determine exactly which users can access the box via ssh and by which authentication method.

---

For DigitalOcean's explanations of the `sshd_config` directives, see also: [How To Tune your SSH Daemon Configuration on a Linux VPS | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-tune-your-ssh-daemon-configuration-on-a-linux-vps)


