---
title: 'MBP wakes up from sleep to back screen —  Solution'
date: 2018-05-02 09:23:04
header:
  overlay_image: /assets/images/blog/2018/apple-wake-up.jpg
  teaser: /assets/images/blog/2018/apple-wake-up.jpg
image: /assets/images/blog/2018/apple-wake-up.jpg
categories:
  - Technology
tags:
  - dGPU
  - high sierra
  - macOS
---

**Note:** Just you all to know, that I don't have this mac anymore and that this solution never really worked for long time. In the end I got really mixed results and the issue arise again sometimes. My best advice for you all is just replace the dGPU or even better buy a new mac. 
{: .notice--warning}

A couple of weeks ago I [wrote about the problem I have with my old MacBook Pro wakening from sleeping to a black screen](/blog/2018/04/13/mbp-wakes-up-from-sleep-to-back-screen/). I also pointed out that I was [testing a solution](/blog/2018/04/13/mbp-wakes-up-from-sleep-to-back-screen/#the-lidwake-on-test) related to the lid. I can tell that since I've testing this solution I haven't suffered any wake up to a black screen, so I guess that the problem is related to the lid, either to the hinge or to the magnet that trigger the wake up and the sleep processes.

I'm doing two things to prevent the black screen behavior. First I'm sleeping the computer using the command line:

```shell
sudo shutdown -s now
```

I've also change the hibernation mode of the machine to zero, in other words, it doesn't save the state in a file in the hard drive. I've also deleted that file from my hard drive.

```shell
sudo rm -f /var/vm/sleepimage
```

Finally I've disable the options to wake up the computer when you connect to power source and when you open the lid.

```shell
sudo pmset -a lidwake 0
```

So, now to wake up the computer I have to hit any key on the computer keyboard or the trackpad after I've open the lid.

So far it has been working great.
