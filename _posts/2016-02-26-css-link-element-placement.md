---
published: true
layout: post
title: "New Developments in Placement of `<link>` Elements"
---

[The future of loading CSS - JakeArchibald.com](https://jakearchibald.com/2016/link-in-body/?imm_mid=0e0e86&cmp=em-web-na-na-newsltr_20160224)

Interesting developments. Interesting solutions.

Unfortunately, with our webserver only serving HTTP/1.x, and most of our clients not understanding HTTP/2, it doesn't make sense to split stylesheets into multiple requests.

But in the not-so-distant-future, with HTTP/2 more commonly implemented, this article describes an interesting approach to `<link>` element placement.


#### TL;DR

Before each section of content to which the styles apply,

1. load the stylesheet
2. add a functionally empty `<script>` tag
3. add the content.

Like this, e.g.:

```
<link rel="stylesheet" href="/article.css">
<script> </script>
<main>…</main>
```

