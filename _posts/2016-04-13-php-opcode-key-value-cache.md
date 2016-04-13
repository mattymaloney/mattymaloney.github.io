---
published: true
layout: post
title: "Info-Bucket for PHP opcode and Key/Value Cache Alternatives"
---


[Simple PHP Cache Class | Fast PHP Caching Class](http://www.phpfastcache.com/)

Something from outside the normally closed circle of PHP cache recommendations.

*"phpFastCache is a high-performance, distributed object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load."*

---

## Opcode Caches

According to the charts at [List of PHP accelerators - Wikipedia, the free encyclopedia](https://en.wikipedia.org/wiki/List_of_PHP_accelerators#XCache), opcache and xcache are the only 2 viable alternatives -- projects that work with modern php and run in linux.

---

[PHP: APC - Manual](http://php.net/manual/en/book.apc.php)

APC is retired and deprecated.

---

[PHP: OPcache - Manual](http://php.net/manual/en/book.opcache.php)

opcache has taken the place of APC as PHP's "standard" opcode caching system.

See also:

* [PECL :: Package :: ZendOpcache](http://pecl.php.net/package/ZendOpcache)

---

[XCache](http://xcache.lighttpd.net/)

This seems to have been created for Lighttpd, although apparently it works with Apache as well. Last update in 2014 with support for php 5.6.


## Key/Value Caches

[WordPress + Memcached – Scott Taylor](http://scotty-t.com/2012/01/20/wordpress-memcached/)

This is a rather interesting and insightful post about using memcached with WordPress. It describes useful details about wordpress's built-in cache class and how to implement it. I don't think we need this right now.

More installation and configuration details:

* [WordPress + Memcached: How to set it up properly | Maje Media](https://majemedia.com/wordpress-memcached-how-to-set-it-up-properly/)
* [Configuring WordPress to use Memcached | Matt's Technology Blog](http://technology.mattrude.com/2011/11/configuring-wordpress-to-use-memcached/)

And a wordpress plugin that might make all of this very easy: [Memcached Object Cache — WordPress Plugins](https://wordpress.org/plugins/memcached/)

---

More resources that I just don't have time to finish digesting:

* [How to configure WordPress to use Memcached](https://www.siteground.com/tutorials/supercacher/wordpress_memcached.htm)
* [Scaling Pinterest | Percona Live: MySQL Conference And Expo 2012](https://www.percona.com/live/mysql-conference-2012/sessions/scaling-pinterest)
* [Memcached vs Redis? — Big Data Little Geek](http://www.bigdatalittlegeek.com/blog/2014/3/25/memcached-vs-redis)
* [Wordpress Memcached](https://redislabs.com/wordpress-memcached)
* [Memcached VS Redis | Cheeky Monkey Media](http://cheekymonkeymedia.ca/blog/web-development/memcached-vs-redis)
* [How To Configure Redis Caching to Speed Up WordPress on Ubuntu 14.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-redis-caching-to-speed-up-wordpress-on-ubuntu-14-04)
* [Install Redis on Amazon EC2 AMI · GitHub](https://gist.github.com/dstroot/2776679)
