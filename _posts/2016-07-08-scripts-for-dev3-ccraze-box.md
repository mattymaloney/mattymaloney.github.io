---
title: Various Scripts for aws-test cc-www apache2.4 php5.6 dev3.costumecraze.com box
---

## create-sftp-only-user.sh

```
username=cc-manage

sudo adduser $username
sudo passwd $username
sudo usermod -g www $username
sudo mkdir /home/$username/www
sudo chown $username:$username /home/$username/www
sudo chown root:root /home/$username/
sudo chmod 755 /home/$username
#sudo mount --bind /efs/www /home/$username/www

sudo tee -a /etc/fstab <<EOF
/efs/www /home/$username/www none defaults,bind 0 0
EOF

sudo mount -a

sudo tee -a /etc/ssh/sshd_config <<EOF
Match User $username
  PasswordAuthentication yes
  ChrootDirectory %h
  X11Forwarding no
  AllowTcpForwarding no
  ForceCommand internal-sftp
EOF

sudo service sshd restart
```


## rsync.sh

```
rsynclog=rsync-$(date +%Y%m%d-%H%M%S)
sudo rsync -Wave ssh --delete-after --compress --chown=cc-manage:www --chmod=D2775,F664 root@host.static-pages.com:/var/www/html/costumec2/ /efs/www > $rsynclog.log 2> $rsynclog.err.log
```


## fix-www-permissions.sh

```
cd /efs/www
sudo chown -R cc-manage:www .
sudo chmod 2775 .
find . -type d -exec sudo chmod 2775 {} \;
find . -type f -exec sudo chmod 0664 {} \;
```

## Archive Data

Both of these data archiving scripts expect that the `~/.my.cnf` (traditional mysql client configuration file) or `~/.mylogin.cnf` (mysql v5.6+ encrypted authentication information file) files contain the desired mysql authentication information in the `[client]` section.

Edit the `~/.mylogin.cnf` file with `mysql_config_editor`. If using this authentication method, you can also specify a `--login-path` for creating the stored credentials and for selecting them when running `mysqldump`.


### archive-ccraze.sh

Archive relevant data from the costumec_ccraze database.

```
database=costumec_ccraze
filename=$database-$(date +%Y%m%d-%H%M%S).sql

echo "dumping \`$database\` to $filename"
echo -n "" > $filename

echo "DROP DATABASE IF EXISTS \`$database\`;" >> $filename
echo "CREATE DATABASE IF NOT EXISTS \`$database\`;" >> $filename
echo "use \`$database\`;" >> $filename
echo "" >> $filename

mysqldump --quick --single-transaction --databases $database --tables adsales agid catfiles cat_metadata Coupons misspellings optname_new_cat_map ordernum prodcats prodcats_filtered prodfiles product_themes prod_colors promotions redirect_map search sizedata stockinfo store_credit themes vendors >> $filename

mysqldump --quick --single-transaction --databases $database --tables checkout_log order_affiliate_log referer_log --where "0=1" >> $filename

mysqldump --quick --single-transaction --databases $database --tables notifymes --where "created_date > date_sub(now(), interval 1 year)" >> $filename

mysqldump --quick --single-transaction --databases $database --tables save_cart --where "cart_stamp > date_sub(now(), interval 1 year)" >> $filename

mysqldump --quick --single-transaction --databases $database --tables orders --where "orderdate > date_sub(now(), interval 1 year)" >> $filename

echo "tar-gzipping into $filename.tar.gz"
tar czf $filename.tar.gz $filename
```

### archive-tokenq.sh

Archive relevant data from the costumec_tokenq database.

```
database=costumec_tokenq
filename=$database-$(date +%Y%m%d-%H%M%S).sql

echo "dumping \`$database\` to $filename"
echo -n "" > $filename

echo "DROP DATABASE IF EXISTS \`$database\`;" >> $filename
echo "CREATE DATABASE IF NOT EXISTS \`$database\`;" >> $filename
echo "USE \`$database\`;" >> $filename
echo "" >> $filename

mysqldump --quick --single-transaction --databases $database --tables card_errors token_responses --where "stamp > date_sub(now(), interval 1 month)" >> $filename

echo "tar-gzipping into $filename.tar.gz"
tar czf $filename.tar.gz $filename
```
