---
title: New Web in Jekyll
date: 2018-07-23 20:20:00
header: 
  overlay_image: /assets/images/blog/2018/jekyll.png
  teaser: /assets/images/blog/2018/jekyll.png
image: /assets/images/blog/2018/jekyll.png
categories:
  - Personal
  - Technology
tags: 
  - jekyll
  - blogging
  - coding
  - markdown
  - static web
---

As you probably have noticed, some days ago I rolled out a new version of my website, which you probably think it's pretty similar to the previous one, at least talking about its external appearance. However, it's really different in the inside, since it's a [static web](https://en.wikipedia.org/wiki/Static_web_page) build with [Jekyll](https://jekyllrb.com), while the previous one was build on Wordpress. This isn't a change that occurred to me just out of the blue and some moths ago I made a [post about Jekyll](/blog/2018/03/04/jekyll/) where I stated my desire to change to Jekyll, or at least to try,  for several important reasons, being the main ones cost and simplicity. 

Although building a simple site in Jekyll it's really straightforward and simple, make a transition from Wordpress to Jekyll has more "curves" on the way than you would expect. Even more when I was trying to mimic my original website as much as possible. I used a [Wordpress to Jekyll exporter](https://wordpress.org/plugins/jekyll-exporter/) and although the export was smooth, some of the post had the format messed up and I had to edit them manually. It took me a solid month to have everything more or less ready in a decent way. 

{% include figure image_path="/assets/images/blog/2018/luispuerto.net-at-20180723.png" alt="luispuerto.net at 23rd July 2018" caption="luispuerto.net at 23rd July 2018" %}{: .align-center}

Right now, the site is up and running —[as you can see](https://luispuerto.net)— but there are some missing features. I have a TODO list where I'm trying to track down all those features I want to implement and the pending changes I want to make. Some are improvements from the previous version but most of them are missing features that are not implemented in Jekyll right away or they are really basic as you take your Jekyll site *out of the box*. These shortcomings are usually related to the fact I'm hosting the web in [GitHub Pages](https://pages.github.com), which is free, but you have some missing features since they build your page in `--safe` mode. So, you can't use custom plugins[^1]. 

Yet Jekyll provides an archive, categories and tags *out of the box*, those features aren't as nice as those provided by a dynamic site. For instance, you need to create yourself the those pages —which isn't really a problem— but then you need to create manually the pages for each category and tag a page you want to show. However, if you use a plugin like [Jekyll Archives](https://github.com/jekyll/jekyll-archives) all the process is much nicer and the plugin build the all the tag and category pages for you. As you can see right now, I don't even have an archive page and I'm relying in a [paginated chronological archive](/blog/), what, at this moment, is more than enough. 

There are a couple of ways to overcome the shortcomings of GitHub pages if you really want to use custom plugins. The first and most obvious one is to build your site locally every time you make a change and then you push it `_site` folder to GitHub. From there on, you can go the extra mile and make everything automatically in the server side, whether you use a [Continuous Integration](https://jekyllrb.com/docs/deployment/automated/#continuous-integration-service) service or you use somethings more complex like [Netlify or others](https://jekyllrb.com/docs/deployment-methods/)[^2]. I'll probably try to implement the Travis CI method and then research other more sophisticated ways to deploy. 

I'm taking this as a way of learning about Jekyll platform —[liquid](https://shopify.github.io/liquid/)— html, css and a little bit of other things like javascript and ruby —ruby looks like a really useful language that I would love to learn in the future. Even I improve my Git skills since to publish in Jekyll you need to push. I wanted to roll out the site before it was fully finished because I find interesting to share how I finish to build the site. I really think we live in a society where we are too fixated on the final product but usually we don't share how we arrive to it, something that sometimes it's even more important. This particular idea is one of the characteristics I like the most from Git and GitHub, and all other repo hostings. You can share all the changes with others and understand how and why to reached the final "product". I hope you enjoy also "the way", most of the times as satisfying as "the end". 



[^1]: You can see a list of the allowed plugins [here](https://pages.github.com/versions/).
[^2]: There are several other services besides the ones explained in the Jekyll documentation. If you research a little bit you are going to find several more options. [Jenkins](https://sketchingdev.co.uk/blog/continuous-deployment-of-jekyll-website-with-jenkins.html) seems also to be able to build Jekyll. 



