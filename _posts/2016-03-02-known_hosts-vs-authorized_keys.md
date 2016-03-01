---
published: true
layout: post
title: known_hosts vs authorized_keys
---

Resolving some confusion between the known_hosts and authorized_keys files.

**known_hosts** is a client's memory of servers it has already authenticated.

**authorized_keys** is a server's stash of public keys that can be used to authenticate clients.

The private keys that a client uses to connect to a server should be generated (along with the public key) on the client. The private key should never leave the client machine. *Is that actually possible?* So how do you initially authenticate a new client server? Don't we need to copy the private key from an existing client to the new client?

See also:

* [authentication - What is the difference between authorized_key and known_host file for SSH? - Information Security Stack Exchange](http://security.stackexchange.com/questions/20706/what-is-the-difference-between-authorized-key-and-known-host-file-for-ssh)
* [security - SSH key-based authentication: known_hosts vs authorized_keys - Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/42643/ssh-key-based-authentication-known-hosts-vs-authorized-keys)
