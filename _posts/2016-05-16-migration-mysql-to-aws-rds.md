---
published: false
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

---

Create and mount a large storage/transfer volume for the EC2 instance.

[Making an Amazon EBS Volume Available for Use - Amazon Elastic Compute Cloud](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)

---

Bash script to create and tar|gzip the exports. But first, `mkdir` and `cd` into an empty directory to store the dumps.

```
dbnames=( costumec_blog costumec_orders costumec_reviews costumec_wap costumec_ccraze costumec_photos costumec_school costumec_wasatch ccraz_manny costumec_dev costumec_phplive costumec_search costumec_weblog )
for dbname in "${dbnames[@]}"
do
  [[ -d $dbname ]] || mkdir $dbname
  chown mysql:mysql $dbname
  mysqldump --fields-terminated-by ',' --fields-enclosed-by='"' --lines-terminated-by 0x0d0a --single-transaction --order-by-primary --tab=$dbname $dbname
  tar czf $dbname.tar.gz $dbname
  rm -rf $dbname
done
```

For bash arrays, see also: [Bash For Loop Array: Iterate Through Array Values](http://www.cyberciti.biz/faq/bash-for-loop-array/).

---

Install mysql client on test box:

`sudo yum install mysql56`

---

Use `sftp` to transfer the tar.gz backup files:

`sftp -o IdentityFile=~/.ssh/ncali-03.pem ec2-user@52.9.125.176`

---

User netcat `nc` to transfer files or streams:

[Easy way to do a mysqldump and restore using netcat (this is NOT encrypted) · GitHub](https://gist.github.com/vanjos/6053606)

---

