---
id: 1819
title: 'MBP wakes up from sleep to back screen &#8211;  Solution'
date: 2018-05-02T09:23:04+00:00
author: Luis Puerto
layout: post
guid: http://luisspuerto.net/?p=1819
permalink: /2018/05/mbp-wakes-up-from-sleep-to-back-screen-solution/
wtr-disable-reading-progress:
  - ""
wtr-disable-time-commitment:
  - ""
image: /wp-content/uploads/2018/04/apple-wake-up.jpg
categories:
  - Technology
tags:
  - dGPU
  - high sierra
  - macOS
---
A couple of weeks ago I [wrote about the problem I have with my old MacBook Pro wakening from sleeping to a black screen](http://luisspuerto.net/2018/04/mbp-wakes-up-from-sleep-to-back-screen/). I also pointed out that I was [testing a solution](http://luisspuerto.net/2018/04/mbp-wakes-up-from-sleep-to-back-screen/#the-lidwake-on-test) related to the lid. I can tell that since I&#8217;ve testing this solution I haven&#8217;t suffered any wake up to a black screen, so I guess that the problem is related to the lid, either to the hinge or to the magnet that trigger the wake up and the sleep processes.

I&#8217;m doing two things to prevent the black screen behavior. First I&#8217;m sleeping the computer using the command line:

<pre class="lang:sh decode:true">$ sudo shutdown -s now</pre>

I&#8217;ve also change the hibernation mode of the machine to zero, in other words, it doesn&#8217;t save the state in a file in the hard drive. I&#8217;ve also deleted that file from my hard drive.

<pre class="lang:sh decode:true">$ sudo pmset -a hibernatemode 0
$ sudo rm -f /var/vm/sleepimage</pre>

Finally I&#8217;ve disable the options to wake up the computer when you connect to power source and when you open the lid.

<pre class="lang:sh decode:true ">$ sudo pmset -a acwake 0
$ sudo pmset -a lidwake 0</pre>

So, now to wake up the computer I have to hit any key on the computer keyboard or the trackpad after I&#8217;ve open the lid.

So far it has been working great.