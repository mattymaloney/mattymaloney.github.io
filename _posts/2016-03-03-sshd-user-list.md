---
published: true
layout: post
title: Determine Which Users Can Login Via SSH
---

... and which method, password and/or authorized_keys, those users are permitted to connect.

First, look at `/etc/shadow` to see which users have passwords set. Valid passwords start with a `$` after the first `:`.

