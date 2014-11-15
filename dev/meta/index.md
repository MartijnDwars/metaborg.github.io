---
title: MetaBorg Meta
layout: page
modified: 
excerpt: "Website maintenance"
image:
  feature: 
  credit: 
  creditlink: 
context: dev
sideimage: 
---

This web site is generated with <a href="http://jekyllrb.com">Jekyll</a> using the <a href="http://mademistakes.com/minimal-mistakes/">Minimal Mistakes</a> theme and hosted with [github pages](https://pages.github.com/).


### Contributing

The sources of this site are in the repository <github.org/metaborg/metaborg.github.io>.

We accept pull requests.

### Migrating from TWiki

__pandoc__ is nice program for converting between various markup formats. The easiest way to convert the material on the old site is to take the HTML that is produced by TWiki and convert it to markdown. Take the 'printable' version of the page to avoid the sidebar:

    pandoc -f html -t markdown http://strategoxt.org/view/Spoofax/News?skin=print.pattern > news.md

The result may still need some tweaking; in particular the stuff at the bottom should be removed.