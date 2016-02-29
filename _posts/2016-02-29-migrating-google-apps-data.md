---
published: true
layout: post
title: Options for Migrating Google Apps Data
---

In a cleanup effort, I'm experimenting with options for migrating Google Apps data from former employees to various archive accounts. 

---

[Data tools - Download your data](https://takeout.google.com/settings/takeout)

Google Takeout produces a .zip or .tgz archive that can be stored in Drive, Dropbox, OneDrive, or downloaded directly via an emailed link.

I don't know anything yet about how to access the information in the archive.

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

Don't know, haven't done this yet.

### Gmail

I'm using Thunderbird to download/sync all of the migration-ready user's email. Then, I will move all of those messages into a newly created folder, named for this migrating user, in the archive account's gmail.

At this time, I haven't completed the process, so I don't have detailed notes.
