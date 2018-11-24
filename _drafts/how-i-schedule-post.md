---
title: How I schedule post in Jekyll
# date: 2018-11-01 00:00 +00:00
# last_modified_at: 2018-11-11 20:45 +00:00
header: 
 overlay_image: /assets/images/blog/2018/jekyll+travis.png
 teaser: /assets/images/blog/2018/jekyll+travis.png
 caption: Jekyll and Travis logos
 overlay_filter: 0.3
categories: [Technology, Personal]
tags: [jekyll, blogging, CI, travis, raspberry pi]
twitter: 
  image: /assets/images/blog/2018/jekyll+travis.png
#  hashtags: [tag1, tag2]
toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
---

The [other day](/blog/2018/11/26/how-i-publish-my-blog-ci/) I was explaining how I publish my blog using CI. Today I want to explain how I schedule post in my blog while still involving CI. 

You know that [Jekyll](/archive/tags/jekyll) is a great, but sometimes it's lacking some features, like be able to schedule post. I don't think it's a dealbreaker, but it's definitely helpful sometimes. Sometimes I write a couple of post at once, just because they are part of a series, or a single post that is split because it's really big, or just because I like to... However, I don't want to publish those post right away so I would like to schedule them. 

There are several solutions out there to schedule, like [this](https://serverless.com/blog/static-site-post-scheduler/), this [other](https://forestry.io/blog/automatically-publish-scheduled-posts-for-static-site/#option-2-using-a-lambda-task-to-trigger-your-build) or that [one](http://brettterpstra.com/2013/01/17/scheduling-posts-with-jekyll/). However, most of those are too complicate for my taste, or you need to have *something* on AWS, etc. 

Since I have a [Raspberry pi](/archive/tags/raspberry-pi) at home, it's connected to internet all the time and all the time it's on, I can schedule [cron jobs](https://en.wikipedia.org/wiki/Cron) on it to run scripts. On the other side, you can trigger your site rebuild on Travis using [their API](https://docs.travis-ci.com/user/triggering-builds/). I'm already doing that with a cron job to rebuild my site everyday at 8 in the morning. Cron it's really handy and allow you a lot of different [setups](https://crontab.guru). For instance, you can run a script every minute, every six hours, once a week, once a month, a year... etc. 

## What is cron? 

Just in case you don't know or you are so lazy that you can't check the [Wikipedia](https://en.wikipedia.org/wiki/Cron), I'm going to sum up for you what is cron. 

Cron is just a scheduler for jobs that most of UNIX machines —linux and mac for example— have. It's really basic, but really handy if you know more or less how to program it. I recommend you to know more or less how to use it, but because can make your life quite easier. 

## How do you access to Travis API

On travis documentation you can find more options, but this is the basic script you need to run on cron —or on your terminal— if you want to access Travis API 

```sh 
#!/usr/bin/env bash
body='{
"request": {
"branch":"master"
}}'

curl -s -X POST \
   -H "Content-Type: application/json" \
   -H "Accept: application/json" \
   -H "Travis-API-Version: 3" \
   -H "Authorization: token YOUR-TOKEN" \
   -d "$body" \
   https://api.travis-ci.com/repo/your-github-username%2Fyour-repo-name/requests
```

You can get your Travis token on your [preferences page](https://travis-ci.com/account/preferences) on Travis page. You also need to set `your-github-username` and `your-repo-name` in the url. Remember you also need to make your script executable. 

```shell
$ chmod +x path/to/your/script.sh
```

## Running jobs in cron

If you want to add jobs to cron you just need run in the terminal

```shell
$ crontab -e
```

and you default text editor will open. There you can add jobs taking into account the following [rules](https://en.wikipedia.org/wiki/Cron#Overview): 

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * command to execute
```

In command to execute is where you put the route to your script. 

For example, if I wanted to run my building script on Nov 26th at 6.07 am I have to set up the following line in cron: 

```
7 6 26 11 * /root/scripts/cron-builds-jekyll/luispuerto-net.sh
```

You can set up as much lines you want in cron, so you can set up multiple post at once for different dates and so. 

**Note:** Please mind in what time zone is your blog and in what time zone is your pi —or whatever you are using to schedule post— because if they aren't in the same time zone you need to take into account when you schedule your cron job. 
{: .notice--warning}

## When I'm out home

When I'm out of home and I still want to program 