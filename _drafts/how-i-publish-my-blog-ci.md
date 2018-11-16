---
title: "How I publish my blog: Using Continuous Integration with Jekyll"
# date: 2018-11-01 00:00 +00:00
# last_modified_at: 2018-11-11 20:45 +00:00
header: 
 overlay_image: /assets/images/blog/2018/jekyll+travis.png
 teaser: /assets/images/blog/2018/jekyll+travis.png
 caption: Jekyll and Travis logos
 overlay_filter: 0.3
image: /assets/images/blog/2018/jekyll+travis.png
categories: 
 - Technology
 - Personal
tags: [jekyll, blogging, CI, travis]
# toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
# toc_sticky: true
---

As I've mention [before](/blog/2018/07/23/new-web-in-jekyll/), this blog is build as a [static website](https://en.wikipedia.org/wiki/Static_web_page) with [Jekyll](https://jekyllrb.com), which allow me not to worry about data bases, logins or anything of the sorts when I'm publishing and managing the web. I use the free tier of [GitHub Pages](https://pages.github.com) as hosting. This has its downsides and upsides, the main upside is, it's really convenient and free. On the other hand, I'm limited to [certain Jekyll plugins](https://pages.github.com/versions/) if I want to use GitHub Pages building capabilities. This can be circumnavigate if you use continuous integration to build your site. 

## What is Continuous Integration?

You can read about this in the [Wikipedia](https://en.wikipedia.org/wiki/Continuous_integration), but CI is —summarizing a lot— basically just testing your code before you *integrate* —merge— it with the master branch, so you know that you aren't breaking anything —serious. Usually what you do is *building* your copy and then you merge it with the master branch. However, building is a pain, and even more if you have to do it all the time. For this reason, the CI services were born. 

Usually a CI works as it follows —again simplified: 

- It's usually related / connected to your online repo and when you push commits to the repo the [webhooks](https://en.wikipedia.org/wiki/Webhook) are triggered and CI comes into action. 
- CI download the last commits of your repo to a machine —usually virtual— with a OS you can specify and begin tu run a series of scripts[^1]. 
- If those scripts give an error, so your code can't be *build* report to you as failing and —bad luck— you have to review what you have done because it's not working. 
- If the scripts exit without an error —you're lucky— and now a couple of things can happen. 
  - Just nothing else. You are notify there is not errors in your code —so you are free to merge. 
  - The code is automatically deployed. 
    - It's merged into another branch —usually master. 
    - It's deploy to a server or something else. 

All of these have a lot of sense in GitHub, GitLab and other repo online services where webhooks can be triggered even with a pull request, so you can know beforehand if someone else code is worth to review in an even of a pull request. 

That's more of less the official function of the CI. However, it can do much more than that. For example is Homebrew repos it builds the bottles. It can build software and apps and deploy it to your website. Or like in our case can build your Jekyll site and deploy to GitHub Pages. 

## How? 







[^1]: These scripts can be whatever you want —more or less. 