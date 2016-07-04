---
layout: post
title: Trying a Reverse Proxy Setup for WordPress
---

The goal is to server wordpress from https://www.example.com/*blog*/ without running wordpress from the www.example.com server. The only way I know to do this is to reverse proxy from the www.example.com server. In this case, a request to `https://www.example.com/blog/some-path` will be accepted by the www.example.com server, sent to `https://blog.example.com/` transparently in the background, and the response forwarded on to the client as a response to its original request.

## Useful Resources

[Running a Reverse Proxy with Apache:](http://www.apachetutor.org/admin/reverseproxies)

And old guide, but it goes over the underlying principals rather well.

