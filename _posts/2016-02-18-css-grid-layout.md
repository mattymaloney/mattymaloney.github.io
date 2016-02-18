---
published: true
layout: post
title: Simplest CSS Grid Layout
---

There are countless css grid options. Countless options within nearly countless approaches to the problem.

One approach that stands out from the others is [ungrid - the simplest responsive css grid](http://chrisnager.github.io/ungrid/). I like its genius simplicity.

The css:

```
.container-selector {
    .row { display: table; table-layout: fixed; width: 100%; }
    .col { display: table-cell; }
}
```

I find this approach particularly useful when I need to organize a single row of 100% width -- and when, for compatibility reasons, flexbox ([A Complete Guide to Flexbox | CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)) is not an option.

