---
title: New Web in Jekyll
date: 
header: 
  overlay_image: assets/images/blog/2018/jekyll.png
  teaser: assets/images/blog/2018/jekyll.png
---

As you probably have noticed, a coupe of days ago I rolled out a new version of my website. It's pretty similar to to the previous one, at related to the external design. However, it's really different form the previous one on its core. The new version is a static website build with Jekyll, which is a huge change in comparison with the previous web build on Wordpress. I announced that I was about to make this transition [awhile ago](https://luisspuerto.net/blog/2018/03/04/jekyll/). 

Although building a simple site in Jekyll it's really straightforward and simple, make a transition from Wordpress to Jekyll it's a little bit more complicates. My main, has been to create and mimic all the functionalities I had in the previous one. 

Right now, the site is up and running, but there are some things missing. I have a [TODO](https://github.com/luisspuerto/luisspuerto.net/blob/master/TODO.md) list where I try to track down all the things I have pending to do in the site. Some of those problems are related to the fact the web is right now hosted in GitHub pages. GitHub pages allows you to host your website for free, but it has several downsides, being the main one that you can't use any plugin that haven't been whitelisted by them and the list it's really small. 

This means that for example in this moment I have no post archive –[only pagination](https://luisspuerto.net/blog/)– and you can't filter post by category or tag. Jekyll can provide those functionalities without the help of any plugin, but you have to create yourself the page for each category and for each tag, while if you have a plugin like [jekyll-archives](https://github.com/jekyll/jekyll-archives) it will build for you. 

To be able to build Jekyll with any plugin while still hosting your site in GitHub pages you should enable Travis and make it build your site on every commit you make. Other options are also to build it locally and puss the `_site`folder to GitHub. 

I wanted to roll out the site before it was fully finished because I wanted to share with you the final stage of building the site. I really think that we live in a society where we are too fixated on the final product but we usually don't share how we arrive to those product, something that sometimes it's even more important. This is somethings that I really like of Git and GitHub, and all other repo hostings. You can share all the changes with others and understand why things are the way they are. 