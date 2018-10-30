---
title: Cutting down the size of your repo
# date: 2018-10-29 00:00 +00:00
header: 
  overlay_image: assets/images/blog/2018/image.jpg
  teaser: assets/images/blog/2018/explorer-header-01.jpg
  caption: Me enjoying the sunny Yellowstone
categories: 
  - Professional
tags: 
  - git
  - technology
---

Latterly I've been playing with git and trying to fix a couple of size problem with more or less luck. Git, is a wonderful tool to track and follow changes in almost any kind of document, specially is they are plain text documents —or sort of— like any kind of code is. Problem is, code isn't always alone and oftentimes it goes in company with other files, mostly binary files. Those files can still be track with Git, but git usually can't see through them. So, you only know that the file changes and you record the whole size changing. If you have big files and / or multiple of small files that change could be a problem since every time you change —or move— the file, you duplicate the size of the file in the repo since git is keeping a copy of the old file plus the new file. 



