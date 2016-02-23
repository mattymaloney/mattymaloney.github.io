---
published: true
layout: post
title: "Finding a Better Windows File-Copy Utility"
---

I took a few extra minutes today, while copying a very large file across the network, to look at better file copy options.

Ideally, I was looking for something that can:

* resume a broken/failed copy. Bonus points if it can copy entire directories, picking up where it left off, even if it left off in the middle of a file.
* compress contents while copying to make better use of the network resources. I assume this would require a service listening on the other end of the copy. Therefore, this would best be done by a native windows utility.
* show progress while copying.

---

I didn't actually decide on anything, but I did get reminded about robocopy, which is a pretty good option. For more options, and a rather good camparison of 10 or so file copy utilities, see [RoboCopy vs RichCopy vs SyncBack vs EMCopy vs XXCopy vs XCopy vs KillCopy vs FastCopy vs Rsync vs TeraCopy vs 7zip](http://mfctips.com/2013/11/01/robocopy-vs-richcopy-vs-syncback-vs-emcopy/).

Robocopy for Windows Server 2003 can be downloaded from [Windows Server 2003 Resource Kit Tools from Official Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=17657).