---
title: New Web in Jekyll
# date: 2017-07-21 21:00:00
header: 
  overlay_image: assets/images/blog/2018/jekyll.png
  teaser: assets/images/blog/2018/jekyll.png
---

As you probably have noticed, a some days ago I rolled out a new version of my website, which you probably think it's pretty similar to the previous one, at least talking about its external appearance. However, it's really different in the inside, since it's a [static web](https://en.wikipedia.org/wiki/Static_web_page) build with Jekyll. Previous one was build on Wordpress. This isn't a change that occurred to me just out of the blue and some moths ago I made a [post about Jekyll](https://luisspuerto.net/blog/2018/03/04/jekyll/) where I stated my desire to change, or at least to try, to Jekyll for several important reasons, being the mean ones cost and simplicity. 

Although building a simple site in Jekyll it's really straightforward and simple, make a transition from Wordpress to Jekyll it's a little bit more complicate. Even more when I was trying to mimic my original website as much as possible. 

Right now, the site is up and running —[as you can see](https://luisspuerto.net)— but there are some missing features. I have a [TODO](https://github.com/luisspuerto/luisspuerto.net/blob/master/TODO.md) list where I'm trying to track down all the features I want to implement and the pending changes I want to do. Some are improvements from the previous version but most of them are missing features that are not implemented in Jekyll right away or they are really basic. These is usually related to the fact I'm hosting the web in [GitHub Pages](https://pages.github.com), which is free but you have some missing features since they build your page in `--safe` mode. So, you can't use custom plugins[^1]. 

Some of those missing features are 

Some of those problems are related to the fact the web is right now hosted in GitHub pages. GitHub pages allows you to host your website for free, but it has several downsides, being the main one that you can't use any plugin that haven't been whitelisted by them and the list it's really small. 

This means that for example in this moment I have no post archive –[only pagination](https://luisspuerto.net/blog/)– and you can't filter post by category or tag. Jekyll can provide those functionalities without the help of any plugin, but you have to create yourself the page for each category and for each tag, while if you have a plugin like [jekyll-archives](https://github.com/jekyll/jekyll-archives) it will build for you. 

To be able to build Jekyll with any plugin while still hosting your site in GitHub pages you should enable Travis and make it build your site on every commit you make. Other options are also to build it locally and puss the `_site` folder to GitHub. 

I wanted to roll out the site before it was fully finished because I wanted to share with you the final stage of building the site. I really think that we live in a society where we are too fixated on the final product but we usually don't share how we arrive to those product, something that sometimes it's even more important. This is somethings that I really like of Git and GitHub, and all other repo hostings. You can share all the changes with others and understand why things are the way they are. 

---

**Footnotes:**

[^1]: 