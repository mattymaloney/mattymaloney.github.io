---
published: true
layout: post
title: "Forcing Client Redirect from HTTP -> HTTPS with HTTP-Only EC2 Instance Through AWS ELB"
---
This looks like the right way to do it.

```
RewriteEngine on 
RewriteCond %{HTTP:X-Forwarded-Proto} ^http$
RewriteRule .* https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

See [AWS Developer Forums: Redirect http into https using load balancer?](https://forums.aws.amazon.com/thread.jspa?messageID=723281)
