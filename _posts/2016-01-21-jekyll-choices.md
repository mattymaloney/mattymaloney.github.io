---
published: true
title: Jekyll Choices
layout: post
---


# Jekyll Blog Setup


## Choose Editors


### Edit directly on GitHub

Of course, you can edit directly from the repo on the github website.

[Creating files on GitHub](https://github.com/blog/1327-creating-files-on-github)


### prose.io

[Prose](http://prose.io/)

Website doesn't work very well on android browser. But the desktop website is pretty solid.

The diff previews before saving are a great feature.

Unfortunately, prose stumbles when it comes to front matter. Much better than tinypress, but still failing.

* Front matter with empty values are not shown in the editor.
* Post titles have to be created in front matter, at which point the post filename is hidden. Prior to creating the title, the filename is shown at the top of the editor. Lots of clicking just to enter the title and then make sure the filename still says what you want.
* I type the same 3 front matter elements for every post: title, layout, and published.


### MrHyde

For Android, this shows great promise. It allows editing arbitrary files in arbitrary repos. Not quite yet well-tuned for blog editing.


### Jekyll Editor

A chrome app, so not helpful for mobile editing.

[Jekyll Editor - Chrome Web Store](https://chrome.google.com/webstore/detail/jekyll-editor/dfdkgbhjmllemfblfoohhehdigokocme?hl=en)


### tinypress.co

A combined Editor and choice of 6 themes.

Pros:

* Easy to get started. Just fork repo and rename to {username}.github.io.
* Has an android app which is a wonderful model of simplicity.
* I like tinypress because it has a functional editor on the website as well as on android.

Cons:

* Limited to supporting user- and organization-level blogs. Does not appear to support project- or repo-level blogs.
* Despite claims that it does, I can not see any way to edit front matter. This is a deal breaker. As much as I like it, I have to push it to the end of the list.

I did go with tinypress's implementation of the Pixyll theme, but am not using their editor.


### [LightPaper](http://lightpaper.42squares.in/)

"Probably the best text+markdown editor for your Mac"

Not specifically for Jekyll or github, but Jekyll preview rendering is supported, I think. Not sure if it can directly browse the github repos, or if edits are local and then need to be pushed to github.

Bonus: See add-on for creating diagrams using markdown-friendly syntax. [LightPaper/Adding Mermaid Diagrams](https://github.com/42Squares/LightPaper/blob/master/doc/Adding%20Mermaid%20Diagrams.md)

Nice-looking editor, but it doesn't directly browse github repos, which is a level of simplicity I need in my editor. I don't want a local copy of the blog repo.


## Choose a Starting State/Theme

### Pixyll

Lots of themes available in the [tinypress github repos](https://github.com/tinypressco) (even themes that don't appear to be supported in tinypress). I'm particularly interested in [Pixyll](http://pixyll.com/) ([github](https://github.com/tinypressco/pixyll)), which is supported in tinypress.


### Phlow/feeling-responsive

[Phlow/feeling-responsive](https://github.com/Phlow/feeling-responsive)

"Feeling Responsive« is a flexible theme for Jekyll built on Foundation framework. You can use it for your company site, as a portfolio or as a blog."


### Phlow/simplicity

[Phlow/simplicity](https://github.com/Phlow/simplicity)

"A Jekyll theme for minimalists focused on essentials avoiding unnecessary clutter. But still giving you all you need to build wonderful pages for your content."


### Poole and Lanyon

IE >= 9 required for client browsers.

[Poole · The Jekyll Butler](http://demo.getpoole.com/)
[Lanyon · A Jekyll theme](http://lanyon.getpoole.com/)

[How I Created a Beautiful and Minimal Blog Using Jekyll, Github Pages, and poole · Joshua Lande](http://joshualande.com/jekyll-github-pages-poole/)


### Jekyll Now

[barryclark/jekyll-now](https://github.com/barryclark/jekyll-now)

A forkable starting state for a blog.


### skinny-bones

[mmistakes/skinny-bones-jekyll: ](https://github.com/mmistakes/skinny-bones-jekyll)

"A Jekyll starter with a variety of customizable layouts and components."

Another forkable starting state.


## See Also

* [Build A Blog With Jekyll And GitHub Pages – Smashing Magazine](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)

* [jekyll/jekyll: Jekyll is a blog-aware, static site generator in Ruby](https://github.com/jekyll/jekyll)

* [How I Created a Beautiful and Minimal Blog Using Jekyll, Github Pages, and poole · Joshua Lande](http://joshualande.com/jekyll-github-pages-poole/)

* [christian-fei/Simple-Jekyll-Search: A JavaScript library to add search functionality to any Jekyll blog.](https://github.com/christian-fei/Simple-Jekyll-Search)

Try or Do
Shall I try? Or shall I do? Or something in between the two?
