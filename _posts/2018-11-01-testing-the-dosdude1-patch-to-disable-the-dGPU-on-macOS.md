---
title: Testing the Dosdude1 patch to disable the dGPU on macOS
# date: 2018-11-01 10:30 +00:00
header: 
  overlay_image: "assets/images/blog/2018/grey_screen.jpg"
  teaser: "assets/images/blog/2018/grey_screen.jpg"
  caption: "Apple grey screen of dead"
image: /assets/images/blog/2018/grey_screen.jpg
categories: 
  - Personal
  - Technology
tags: [dGPU, graphic card, macOS, high sierra, how to]
toc: true
# toc_label: "Unique Title"
toc_icon: "laptop-code"  # corresponding Font Awesome icon name (without fa prefix)
---

As you [know](/blog/2017/12/05/my-macbook-pro-late-2011s-discrete-graphics-card-said-ciao-again/), I have a problem on my *vintage* MacBook Pro late 2011 related to my dGPU. Basically it died :skull: at the end of 2017. So I have to find a *patch* to be able to continue use it, and I finally did in [this way](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/). 

Yesterday, Apple released a [security update](https://support.apple.com/en-us/HT209193) that seems that in some parts is related to the graphic interface of the system, so I definitely was going to reapply the patch after the update —as after every update I usually do. However, recently I discovered [dosdude1](https://twitter.com/dosdude1?lang=en) and his [page](http://dosdude1.com) where I found out that he has a [patch to disable the dGPU on macs like mine](http://dosdude1.com/gpudisable/). I thought it was a great opportunity to test the patch, since it seems more convenient. 

## What does the patch do? 

I think the best thing is to copy verbatim from the patch itself: 

> This program will disable the dedicated GPU on 15" or 17" MacBook Pro System that have dual video cards installed. After this program has finished running, the following things will be done: 
>
> - An NVRAM variable will be set that prevents the machine from using its dedicated video card. 
> - The video acceleration drivers (kernel extensions) for the respective installed dedicated video card will be removed from the /System/Library/Extensions directory, and backed to up to the root of the hard disk
> - A LaunchDaemon will be installed that prevents the video cards drivers from being re-installed during Software Updates, and endures the necessary NVRAM variables is set correctly if PRAM is reset. 

## Installing the update

The fist thing I did was install the update. I didn't even follow any of the steps I described [here](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/#in-case-you-have-to-update) —I should have— but I just installed the update. 

I encounter problems during the update installation. And at some point I have to force quit the computer —pressing the turn-on/off button till the computer shut off— and upon startup press `cmd+s` and enter the following in the prompt: 

```shell
$ nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
$ reboot
```

After that the update install continue and I was finally able to access to the desktop. 

## Undoing previous changes

Since I wanted to apply to Dosdude1 patch, I thought that it would be wise to undo some of the changes I've done in the system go disable the dGPU. There are two main changes I though I should reverted: 

- Disable the `Loginghook` we created during [the fix](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/#the-fix) —steps 6 to 8: 
  ```shell
  $ sudo defaults delete com.apple.loginwindow LoginHook
  ```
- Restore the `AMDRadeonX3000.kext` that we moved during [the fix](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/#the-fix) — step 4:  
  ```shell
  $ sudo cp -p /System/Library/Extensions-off/AMDRadeonX3000.kext /System/Library/Extensions/
  ```

## Applying the patch

Well, this is pretty straightforward, isn't it? You just click on the app and follow the instructions. Take into account that you have to reboot at the end. 

## Outcome

I have to say that the outcome was positive and the computer worked well. However, there was a minor problem that for me was a deal breaker, the computer didn't woke up from sleep. I tried several times and never ever was able to wake up. I always got a black screen. 

I think the reason for this is pretty simple, what the patch is doing is just wiping all the `kext` from `/System/Library/Extensions/`. Nevertheless, you still need some of them to operate all the functions of the computer and to —I think— really disable the dGPU. I asked about these two problems to [@dosdude1](https://twitter.com/dosdude1), as you can see bellow. You can see there too that the temperature of the "GPU Diode" is still high, which mean the dGPU is still active. I haven't receive any reply at the moment of writing this post. 

<blockquote class="twitter-tweet tw-align-center" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/dosdude1?ref_src=twsrc%5Etfw">@dosdude1</a> Hey... I have a question. I&#39;ve just apply your patch to disable dGPU <a href="https://t.co/xUZakry4d7">https://t.co/xUZakry4d7</a> After it&#39;s applied, shouldn&#39;t the GPU Diode plumb down like in the image at the bottom of this post? <a href="https://t.co/EM2SBZKSLG">https://t.co/EM2SBZKSLG</a> <br>Thanks for the tool! <a href="https://t.co/BKzOJkbliJ">pic.twitter.com/BKzOJkbliJ</a></p>&mdash; Luis Puerto (@lpuerto) <a href="https://twitter.com/lpuerto/status/1057569368014495751?ref_src=twsrc%5Etfw">October 31, 2018</a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
<blockquote class="twitter-tweet tw-align-center" data-lang="en"><p lang="en" dir="ltr">I can&#39;t wake up from sleep... and I can&#39;t find the backup files on the root of the hard drive after using your tool</p>&mdash; Luis Puerto (@lpuerto) <a href="https://twitter.com/lpuerto/status/1057601068283166720?ref_src=twsrc%5Etfw">October 31, 2018</a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

As you can see in the thread, at that moment I already wanted to revert changes. Dosdude1 doesn't provide with a tool to revert changes, or with instructions to do so. The major problem I had at that moment was, I couldn't find the backup `kext` files in the root of my hard drive. I later found out that they are in a hidden folder names `.AMD_Backup`. So, I had to pull the backup files from my Time Capsule and apply the security update again —just in case— to reapply my patch to disable the dGPU. 

## How to roll back changes

If you want to roll back changes you have to

### Delete the LaunchDaemon

> A LaunchDaemon will be installed that prevents the video cards drivers from being re-installed during Software Updates, and endures the necessary NVRAM variables is set correctly if PRAM is reset. 

If you don't delete this LaunchDaemon your `kext` are going to be delete after you restore them next time you re/boot. So to delete it you have to run the following command: 

```shell
$ sudo rm -r /Library/LaunchDaemons/com.dosdude1.GPUDisableHelper.plist
```

### Restore the kext

> The video acceleration drivers (kernel extensions) for the respective installed dedicated video card will be removed from the /System/Library/Extensions directory, and backed to up to the root of the hard disk

Now, you can restore your `kext`

```shell
$ sudo cp -p /.AMD_Backup/*.* /System/Library/Extensions/
```

I would also check the ownership —just in case:  

```shell
$ sudo chown -R root:wheel /System/Library/Extensions/AMD*.*
```

### Reapply the previous patch

After that, you can reapply the previous patch like it's explained [here](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/). 

## Bottom Line

I wouldn't say the Dosdude1 patch is bad, but in my case didn't work as it should, since the wake up didn't work. I really don't know if this was due to my previous setup or configuration or if I did miss something on the way, but for me not be able to wake up from sleep isn't operate normally. 

I also think that the patch should be a little bit more documented. There isn't specific instructions to roll back changes or a tool to *uninstall* automatically. There isn't available, or at least I don't know where it is, the source code of the tool, so you can't really know what is really going on. It isn't I don't trust the *dude*, but transparency is always appreciated. 

Finally, I think that the reason the computer doesn't wake up is you can't delete all the kext. The purpose of the patch I've [shared](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/) —I didn't created it— if to disable just de dGPU, but still use some of the functions the  kext provide, and for that reason the problematic kext isn't deleted but moved to another folder, so it isn't loaded on boot, to later load it manually[^1]. The problem isn't just the `AMDRadeonX3000.kext`, but you need to have all the others to really make it work. 

```shell
$ kextstat | grep AMD
  114    2 0xffffff7f82d1b000 0x122000   0x122000   com.apple.kext.AMDLegacySupport (1.6.8) 69C5152C-0305-3914-AD56-6601DD449AF4 <103 12 11 7 5 4 3 1>
  133    0 0xffffff7f835f0000 0x12e000   0x12e000   com.apple.kext.AMD6000Controller (1.6.8) F08FE763-26A1-312E-B690-CB8FDBF8EC31 <114 103 12 11 5 4 3 1>
  144    0 0xffffff7f830c3000 0x22000    0x22000    com.apple.kext.AMDLegacyFramebuffer (1.6.8) 13E3BF67-6700-37F0-82EE-E87F8B71A033 <114 103 12 11 7 5 4 3 1>
  169    0 0xffffff7f83a6b000 0x568000   0x568000   com.apple.kext.AMDRadeonX3000 (1.6.8) 8559CEFE-85B8-3FB7-B20A-9346E5A7986A <168 145 103 12 7 5 4 3 1>
```

For these reasons I still stick to my solution. 



[^1]: This, as far as I know, also make it possible to total disabling of the dGPU. 

