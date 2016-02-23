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

Robocopy for Windows Server 2003 can be downloaded from [Windows Server 2003 Resource Kit Tools from Official Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=17657). Additionally, I found [Robocopy and a Few Examples - TechNet Articles - United States (English) - TechNet Wiki](http://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx) to be a good source of examples I used for some experimentation.

---

For a discussion leading to the suggestion to disable “Large Send Offload (LSO)” on the source side of the copy, see [Local Area Network : How to fix slow LAN transfer speed of files in Windows](http://www.howtosolutions.net/2013/06/fixing-slow-sending-or-receiving-of-files-through-lan-network-using-windows/)

---

None of these file-copy utilities offer over-the-wire compression. We need something with a listener on the other end of the copy.

Consider [WinSCP - Free SFTP and FTP client for Windows](https://winscp.net/eng/index.php). Does this offer the same abilty as scp+tar offers in Linux?

[Using SSH/SCP on Windows](http://ged.msu.edu/angus/tutorials/using-putty-on-windows.html) may help with using winscp with compression on Windows.

Also, try [PuTTY's pscp](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) utility.

A couple more hints here ([windows server 2008 - Compressed network file copy - Server Fault](http://serverfault.com/questions/400538/compressed-network-file-copy)) including a tip on using 7-zip to do the compression.

Also, consider having mssql do the compression during the backup. But, this is not supported until SQL Server 2008.

A bunch more insight, but only a little bit for Windows, see [How to transfer large amounts of data via network](http://moo.nac.uci.edu/~hjm/HOWTO_move_data.html).

Consider using Syncrify or DeltaCopy (probably DeltaCopy, because it's open-source and free) [Difference between Syncrify and DeltaCopy](http://web.synametrics.com/SyncrifyVsDeltaCopy.htm). For DeltaCopy, see [DeltaCopy - Rsync for Windows](http://www.aboutmyip.com/AboutMyXApp/DeltaCopy.jsp).

---


