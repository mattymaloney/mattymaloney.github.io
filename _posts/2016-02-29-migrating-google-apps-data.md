---
published: true
layout: post
title: Options for Migrating Google Apps Data
---


In a cleanup effort, I'm experimenting with options for migrating Google Apps data from former employees to various archive accounts. 

The data we want to preserve from each retiring gapps account:

* Gmail
* Drive
* Calendar
* Contacts
* Keep

Additionally, Gmail, Drive and Calendar should be usable/searchable.

...



---

[Data tools - Download your data](https://takeout.google.com/settings/takeout)

Google Takeout produces a .zip or .tgz archive that can be stored in Drive, Dropbox, OneDrive, or downloaded directly via an emailed link.

I don't know anything yet about how to access the information in the archive.

---

One of the old ways to migrate data from one account to another is Dashboard's Migration tool. [Moving data between Google accounts](https://www.google.com/dashboard/migration/?srcAccount=camille@costumecraze.com&dstAccount=backupemail@costumecraze.com). The tool is no longer supported, but I may create a test account and try it. Dunno. We'll see how the other transfer options pan out.

---

For transferring data in a way that can more easily be searched/used, I'm looking at Drive, Gmail, and Keep for the best way to transfer those contents to an archive account.

### Drive

#### Change Ownership

When there aren't too many files to worry about, you can simply share the files/folders with the archive user, then change the owner to the archive user. Alternatively, you can then remove the original user from edit/view permissions.

#### Transfer via Desktop App

Signin to the migration-ready account with the desktop app. During sign-in, set a custom folder name (e.g. Google Drive [username]) and allow "all" folders to sync.

When sync is complete, use the Drive app's preferences to "disconnect" the user from the local storage.

Next, sign in to drive.google.com with the archive user account. Create a folder named for the migration-ready user (e.g. [username]).

Next, sign to the Drive App with the archive user account. Again, set a custom Google Drive folder name (e.g. Google Drive Archive Master). This time, when selecting folders to sync, the only folder we care about is the one you just created in the previous step -- the folder for the migration-ready user's Drive data.

Once the sync setup is ready, simply move the files from the migration ready account folder (e.g. Google Drive [username]) to the archive account folder (e.g. Google Drive Archive Master/[username]). When sync is complete, you can disconnect the archive account from the Google Drive app.

So far, I think this is the easiest and most reliable transfer method for Drive data.

### Keep

Tranferring Keep data was weak. Have to select and share each note individually, then in the archive account, the note needs to be copied, then labelled to indicate the account from which it migrated.

### Gmail

I'm using Thunderbird to download/sync all of the migration-ready user's email. Then, I will move all of those messages into a newly created folder, named for this migrating user, in the archive account's gmail.

At this time, I haven't completed the process, so I don't have detailed notes.

Well, that really didn't work well at all. Not for a large mailbox anyway. I can see how it might be ok for smaller mailboxes. But, for a large mailbox, because "move" in Thunderbird means it copies all the messages and then deletes the originals, it really needs to complete the entire copy and then the entire delete, but it's not done as an atomic operation. And holy pants Batman, it takes a long time.

A better option may be either Google's Mail Fetcher or Got Your Back. Some guidance:

* [A Gmail Miscellany: Housekeeping your Google Accounts](http://gmail-miscellany.blogspot.jp/2013/05/housekeeping-your-google-accounts.html)
* [How can I import a gmail takout file to another gmail account? - Google Product Forums](https://productforums.google.com/forum/#!topic/gmail/vNBtG4zpZHc)


#### Google Mail Fetcher

Google Mail Fetcher is integrated directly into the gmail web app. Just use it to connect to the old user's POP3 service and import the mail into a specified label of the archive account. I think this is sufficient for our needs, because we want the emails for searchability. I don't think we need the original user's label structure.

* [Check email from other accounts with Gmail - Gmail Help](https://support.google.com/mail/answer/21289?hl=en)

#### Got Your Back

* [GitHub - jay0lee/got-your-back: Got Your Back (GYB) is a command line tool for backing up your Gmail messages to your local computer. It uses Gmail's API over HTTPS. For help with GYB, see the Getting Started Guide at https://github.com/jay0lee/got-your-back/wiki](https://github.com/jay0lee/got-your-back)

The [Got Your Back Getting Started Guide](https://github.com/jay0lee/got-your-back/wiki) is rather nice.

Looks like it's ok to interrupt GYB, as it picks up where it left off. It doesn't do double-restores or double-backups when being asked to resume.

When it restores to our archive account, it restores with a specified label, keeping all other labels already specified in the retired account's gmail, and joins shared messages with already existing messages in the archive folder (e.g. company wide emails).

I'm very impressed with this so far. I'm running it on a 0.05gb mailbox as first test...

Example commands:

To backup: `python3 gyb.py --email [username]@example.com`

To restore: `python3 gyp.py --email [archive-account]@example.com --action restore --local-folder ./GYB-GMail-Backup-[username]\@example.com/ --label-restored "[username]"`

After the restore, archive all items in the inbox. As far as I can tell, there is no way to archive the messages as they're imported, so they will show up in the inbox.

It does take a while. The 0.05gb test took at least 30 minutes, maybe an hour.

### Calendar

After getting takeout archive, log in to the achive master account's calendar, go to Settings -> Calendars, and look between the "My Calendar" and "Other Calendars". Click "Create new calendar" to create a calendar for each ics you want to import from the old user's takeout archive. For each calendar, import into the appropriate calendar you just created.
