---
published: true
layout: post
title: Data Migration from MySQL to AWS RDS MariaDB
---
Most of this holds true for migration from any MySQL/Maria DB to any other MySQL/Maria DB.

---

Very helpful resources:

[Importing Data to an Amazon RDS MySQL or MariaDB DB Instance with Reduced Downtime - Amazon Relational Database Service](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MySQL.Procedural.Importing.NonRDSRepl.html)

[mysql - How can I move a database from one server to another? - Database Administrators Stack Exchange](http://dba.stackexchange.com/questions/174/how-can-i-move-a-database-from-one-server-to-another)

---

We want to dump into delimited data rather than straight to SQL commands. See the Amazon "Importing Data" article above for more details on this and other chosen switches.

Basic format of the dump commands is:

```
mysqldump --fields-terminated-by ',' --fields-enclosed-by='"' --lines-terminated-by 0x0d0a --single-transaction --order-by-primary --tab=[dir-name] [db-name]
```

where, for easy identification, `dir-name` and `db-name` are ideally the same string value.

