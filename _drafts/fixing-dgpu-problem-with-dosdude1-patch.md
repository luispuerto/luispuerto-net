---
title: Disabling dGPU on macOS with Dosdude1 patch
# date: 2018-11-01 00:00 +00:00
header: 
  overlay_image: "assets/images/blog/2018/grey_screen.jpg"
  teaser: "assets/images/blog/2018/grey_screen.jpg"
  caption: "Apple grey screen of dead"
categories: 
  - Personal
  - Technology
tags: [dGPU, graphic card, macOS, high sierra, how to]
---

As you know I have a problem on my *vintage* MacBook Pro late 2011 related to my dGPU. Basically it died at the end of 2017. So I have to find a *patch* to be able to continue use it, and I finally did in [this way](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/). 

Yesterday, Apple released a [security update](https://support.apple.com/en-us/HT209193) that in some parts is related to the graphic interface of the system, so I definitely I needed to reapply the patch after the update. However, recently I discovered Dosdude1 page and I found out that he has a [patch to disable the dGPU on macs like mine](http://dosdude1.com/gpudisable/). I thought it was a great opportunity to test the patch. I really think the patch is really handy because it makes things a little bit easier, even if you are a dummy with your computer. 

## Installing the update

The fist thing I did was install the update. I didn't even follow any of the steps I described [here](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/#in-case-you-have-to-update) —I should have— but I just installed the update. 

I encounter problems during the update installation. And at some point I have to force quit the computer —pressing the turn-on/off button till the computer shut off— and upon the start press `cmd+s` and inter in the prompt: 

```shell
$ nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
$ reboot
```

After that the update install continue and I was finally able to access to my desktop. 

## Undoing previous changes

Since I wanted to apply to Dosdude1 patch, I thought that it would be wise to undo some of the changes I've done in the system go disable the dGPU. There are two main changes I think I should revert: 

- Disable the `Loginghook` we created during [the fix](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/#the-fix) —steps 6 to 8: 

  ```shell
  $ sudo defaults delete com.apple.loginwindow LoginHook
  ```

- Restore the `AMDRadeonX3000.kext` that we moved during [the fix](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/#the-fix) — step 4. However, if you have already updated —like me— you have to have already a kext there. To be honest, I didn't check, but I suppose should be there, so I didn't restore. 

## Applying the patch





