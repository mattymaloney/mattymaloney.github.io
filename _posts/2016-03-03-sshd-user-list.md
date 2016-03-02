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


## Which users have authorized_keys?

Check `/etc/ssh/sshd_config` for the `AuthorizedKeysFile` directive. This will tell you where `sshd` expects to find users' `authorized_keys` file(s). Apparently, the default is to look in `~/.ssh/authorized_keys` as well as `~/.ssh/authorized_keys2`.

After determining where `sshd` is looking for the public keys, look in that location in each users' `/home` directory to see which users have public keys stored on the server.


## Which users are allowed or disallowed by sshd_config

