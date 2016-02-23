---
published: true
layout: post
title: BCP for All Tables
---

I ran a lot of (am still running a lot of) `bcp` commands today. I just barely thought of the notion that I should/could have scripted a sproc to run all the `bcp` commands.

Here's a well-discussed example: [BCP all tables into files from a database](http://blogs.lessthandot.com/index.php/datamgmt/dbprogramming/bcp-all-tables-into-files/) from [Less Than Dot](http://blogs.lessthandot.com/).

```
SELECT 'EXEC xp_cmdshell ''bcp '           --bcp
+  QUOTENAME(DB_NAME())+ '.'               --database name
+  QUOTENAME(SCHEMA_NAME(SCHEMA_ID))+ '.'  -- schema
+  QUOTENAME(name)                         -- table
+ ' out c:temp'                          -- output directory
+  REPLACE(SCHEMA_NAME(schema_id),' ','') + '_' 
+  REPLACE(name,' ','')                    -- file name
+ '.txt -T -c'',no_output'   -- extension, security, no output 
FROM sys.tables
```

With or without `xp_cmdshell`, I guess we can't actually run these commands as we create them. They still need to be run in Query Analyzer or SSMS.
