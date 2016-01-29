---
published: true
layout: post
title: "Controlling Spaces Between Inline Tags for Jade-Generated HTML"
---

For reasonably complex HTML, I need better control of spaces between inline-elements in the generated or further-minimized HTML. 

### `&thinsp;`

I've experiemented with `&thinsp;`, and am mostly happy with the results.

### *pretty* mode

The most common suggestion is to use Jade in "pretty" mode, but that results in unminimized HTML, doesn't account for down-the-chain publishing tools that may minimize the HTML, and doesn't actually work consistently anyway. Even in pretty mode, some (all?) neighboring inline tags are pressed against each other like teenage lovers.

### `&nbsp;`

The other common suggestion is to use `&nbsp;`, which is useful in its own right. However, it's not a fluid space, and will not line-break. It's a solution for other problems, not this one.

### `=" "`

This is workable. It's not pretty, but it does produce the desired code. Down-the-chain minimizers may remove the space, so maybe it's still a danger.

### `&#32;`

This is the best idea I've heard. Preserve the space as an HTML entity, and then feel free to minimize the HTML to whatever extent you wish. The only downside I forsee is not even worth the time it takes to type this sentence -- these space characters take 5 characters instead of 1, which admittedly is a little outrageous, but not effectively bad.


## See Also

[Do I have to use &nbsp; for space in Jade? - Stack Overflow](http://stackoverflow.com/questions/20727360/do-i-have-to-use-nbsp-for-space-in-jade)
