---
published: true
layout: post
title: Waiting Forever to Take MS SQL Server Database Offline
---

Waiting forever to take a database offline.

This stckoverflow answer was very helpful: [Extreme wait-time when taking a SQL Server database offline - Stack Overflow](http://stackoverflow.com/a/808334/1298086).

To find out what process is holding the database open, `exec sp_sho2`.

To kill the spid that's running that process, `kill <spid>`, using the spid(s) found via `exec sp_who2`.


