---
published: true
layout: post
title: How to Use Footnotes in Markdown
---

To use footnotes in markdown, use a `<sup>`-formatted link in the text and link it to the footnote at the bottom of the page, which should also contain a link back to the text that referenced the footnote.

An example:

This is the text that needs a footnote<sup id="a1">[1](#f1)</sup>

<b name="f1">1</b> This is the footnote, which may or may not contain a [link](http://example.com/) itself, but should end with a link back to the referencing text.[&#x21E7;](#a1)

See also:
http://stackoverflow.com/questions/1696619/displaying-unicode-symbols-in-html