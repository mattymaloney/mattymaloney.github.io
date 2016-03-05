---
published: true
layout: post
title: "Command-line Curl in Windows"
---

In looking for a simple way to get results from a url in windows, I came across a very convenient powershell method. This is the way to go if making command line http requests is rare, and/or installing software on the box is undesired.

```
powershell -Command "(new-object net.webclient).DownloadString('http://example.com')"
```

which, while a bit wordy, is similar to typing

```
curl http://example.com/
```

in a more Unix-ish environment.

More information about net.webclient is available here: [WebClient Methods (System.Net)](https://msdn.microsoft.com/en-us/library/system.net.webclient_methods(v=vs.110).aspx)

---

If installing software is not an issue, then curl itself can be installed. Windows 32 and 64-bit downloads are available curl.haxx.se: [cURL - Download](https://curl.haxx.se/download.html#Win32).

---

Apparently, Git for Windows has GNU bash built in, which I guess has `curl` built into it. Haven't tried this.

See [Git - Downloads](http://git-scm.com/downloads).

---

See also:

* [Run cURL commands from Windows console - Stack Overflow](http://stackoverflow.com/questions/2710748/run-curl-commands-from-windows-console/35818093#35818093)
* [Run cURL commands from Windows console - Super User](http://superuser.com/questions/134685/run-curl-commands-from-windows-console/1049138#1049138)

