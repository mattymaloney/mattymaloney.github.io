---
layout: post
title: Trying a Reverse Proxy Setup for WordPress
---

The goal is to server wordpress from https://www.example.com/*blog*/ without running wordpress from the www.example.com server. The only way I know to do this is to reverse proxy from the www.example.com server. In this case, a request to `https://www.example.com/blog/some-path` will be accepted by the www.example.com server, sent to `https://blog.example.com/` transparently in the background, and the response forwarded on to the client as a response to its original request.

## Useful Resources

[Running a Reverse Proxy with Apache:](http://www.apachetutor.org/admin/reverseproxies)

And old guide, but it goes over the underlying principals rather well.

---

[Setting up WordPress behind a reverse proxy – Tanya Nam](https://tanyanam.com/2015/07/13/setting-up-wordpress-behind-reverse-proxy/)

[apache 2.2 - Wordpress behind reverse proxy - Server Fault](http://serverfault.com/questions/653981/wordpress-behind-reverse-proxy)

---

[WordPress › Support » WordPress behind a reverse proxy/ssl endpoint, slightly borked](https://wordpress.org/support/topic/wordpress-behind-a-reverse-proxyssl-endpoint-slightly-borked)

See this one for notes on using wp-cli for search and replace.

----

[Hosted WordPress on a Subdirectory Using Reverse Proxy](https://pressable.com/blog/2015/10/15/reverse-proxy-plugin-for-using-a-hosted-wordpress-site-in-a-subdirectory/)

For the plugin-approach to making sure all the wordpress links and references use the reverse-proxied url.

---

[WordPress behind a reverse proxy.](https://gist.github.com/neverything/7675846)

Another plugin, but much simpler than the previously mentioned plugin. Not sure if we'll need either of the plugins for our scenario. This gist also reminds us to update the .htaccess `RewriteBase` and `RewriteRule` lines. Although I presently expect these lines to be updated when we change the wordpress settings to refer to the new root url.

---
