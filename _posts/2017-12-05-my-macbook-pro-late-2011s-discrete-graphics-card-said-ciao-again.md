---
id: 1070
title: 'My MacBook Pro late 2011&#8217;s discrete graphics card said &#8220;ciao&#8221; &#x1f44b;&#x1f3fb; –again'
date: 2017-12-05T11:51:30+00:00
author: Luis Puerto
layout: post
guid: http://luisspuerto.net/?p=1070
permalink: /2017/12/my-macbook-pro-late-2011s-discrete-graphics-card-said-ciao-again/
image: /wp-content/uploads/2017/12/maxresdefault.jpg
categories:
  - Personal
  - Professional
  - Technology
tags:
  - dGPU
  - graphic card
  - high sierra
  - how to
  - macOS
---
Last Sunday wasn&#8217;t really a pleasant day. On Saturday late night, or rather around Sunday 00.30 am, my MacBook Pro late 2011&#8217;s discrete graphic card begin to fail to in the end not being able to boot it properly. The computer was working just fine, it wasn&#8217;t even using the discrete card, connected to the external screen as I&#8217;ve been doing lately, or any doing any other intensive task. I just rebooted it and 5&#8242; after loading the desktop a **solid gray screen** appear that allowed to do nothing. After I forced reboot pushing the on/off button, the normal loading **gray screen** had glitches as thin-horizontal-weird lines. After 3 or 4 boots into the desktop and then the **_gray screen of dead_** the computer begin to load directly just to the **_gray screen of death_**. Nothing could be done to load the computer normally. I tried safe mode, pressing <span class="lang:sh highlight:0 decode:true crayon-inline">shift</span> key on boot, and nothing, just the same **_gray screen of death_**. Restore mode, <span class="lang:sh highlight:0 decode:true crayon-inline">alt + R</span> on boot, also the **_gray screen of death_**. So, I decided to left the issue to sleep –it was around 1.30 am in the morning, and led the computer to make a hardware test, pressing <span class="lang:sh highlight:0 decode:true crayon-inline">D</span> key on startup.

It&#8217;s not the first time that the discrete graphic card fails in my laptop. It&#8217;s a malaise that occurs to almost any 2011 Macbook Pro&#8217;s computer. I think there is two kind of machines out there, the ones where the issue already happened and the ones where it&#8217;s going to happen. My entire logic board was changed on July 2014 and 3 years and a half down the road it failed again last Sunday. Again, [**the same faulty chip**](http://appleinsider.com/articles/15/01/15/2011-macbook-pro-graphics-class-action-suit-expands-accuses-apple-of-concealing-defects). This is not proper quality Apple (AMD / Nvidia are also culprits here), and I don&#8217;t know whose idea was to mount those logic board / chips on this Mac model, but I think it wasn&#8217;t the brightest idea ever.

So, in Sunday morning, after I slept on the issue and I had a proper coffee and breakfast, I got my hands on to try to fix the problem. I begun with searching on internet about the problem and some people suggested that it was a RAM problem. Well, it could have been. The last time I did a hardware test, when I changed my hard drive in the US, it yielded a RAM problem, and this time threw me the same error.

<div id="attachment_1072" style="width: 3274px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420.jpg"><img class="wp-image-1072 size-full" src="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420.jpg" alt="" width="3264" height="2448" srcset="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420.jpg 2048w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420-300x225.jpg 300w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420-768x576.jpg 768w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420-1024x768.jpg 1024w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420-480x360.jpg 480w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420-1216x912.jpg 1216w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4420-333x250.jpg 333w" sizes="(max-width: 3264px) 100vw, 3264px" /></a>
  
  <p class="wp-caption-text">
    Error 4MEM/66/40000000: 0x846b3798
  </p>
</div>

So I decided to give it a try to this [guy&#8217;s ](https://www.ifixit.com/Answers/View/142544/My+MBP+won%27t+boot%2C+with+horizontal+line+at+apple+logo#answer181366)suggestion and I even switched the RAM for the original ones, since I upgraded 4 years ago to two 8 GB modules. However, the result was always the very exact one. So I decided to ditch the idea that the RAM was the cause of the **_grey screen of death_,** reinstalled the RAM and looked for another solution.

Finally, I found these solutions ([1](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/), [2](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/), [3](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44) and [4](https://github.com/blackgate/AMDGPUWakeHandler)), which were quite recent. It seems that a lot of boards are dying all over again. The solution is quite simple, since what is failing is the discrete graphic card and you usually don&#8217;t use it in your daily life, you just have to stop using that card at all, and use all the time the integrated one. Problem solved. But, how do you make that happen when the only thing you are able to see is the **_grey screen of death?_** Basically you make the computer to not boot using the drivers for the discrete graphic car. The solution works, but there is some caveats, under High Sierra. One is that your Mac is not longer going to sleep properly, but it can be fixed just changing the sleep mode to hibernation. And that the bright control is not longer going to work, at least at the moment. I can live with that.

I&#8217;m going to summarize here what I&#8217;ve done to fix it under High Sierra macOS 10.13.1 on my Late 2011 MacBook Pro.

## The solution

First of all, I have to say that I begun with the solution number [1](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/) and then I switch to the [2](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/), [3](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44) and [4](https://github.com/blackgate/AMDGPUWakeHandler) ones. Probably, you are fine to proceded from the solution number [2](http://luisspuerto.net/2017/12/my-macbook-pro-late-2011s-discrete-graphics-card-said-chao-again/#using-solutions-2-3-and-4) onwards. To make make those solutions work in the long run, you are going to need a USB stick in order to have a way of booting your Mac, from the very beginning, or when you update your system and the fix stop working as a consequence of the update. You don&#8217;t need a very big USB stick. What you are going to store on it it&#8217;s not going to more than 10 mb.

### Moving the kexts

This step isn&#8217;t really necessary, and you can jump to the [next step](http://luisspuerto.net/2017/12/my-macbook-pro-late-2011s-discrete-graphics-card-said-chao-again/#using-solutions-2-3-and-4). However, this is the exactly how I did things, since first I found one solution and then the others. Also, if you have trouble booting with the other solutions, perhaps this part is going to help you.

You need to boot on single user mode (press and hold <span class="lang:sh highlight:0 decode:true crayon-inline ">Cmd + S + R</span> ) and run the following commands.

<pre class="lang:sh decode:true" title="Deleting the kext for the discrete graphic card on my mac">$ fsck -fy # to check a disk
$ mount -uw / # mount a root filesystem with read/write permissions 
$ sudo mkdir /AMD_Kexts/ # make a directory to store the AMD drivers in case you'll need them in future
$ sudo mv /System/Library/Extensions/AMD*.* /AMD_Kexts/ # move the AMD drivers
$ sudo rm -rf /System/Library/Caches/com.apple.kext.caches/ # remove the AMD drivers cache
$ sudo mkdir /System/Library/Caches/com.apple.kext.caches/ # just in case OS X will be dumb and will not recreate this directory, I am creating it for OS X
$ sudo touch /System/Library/Extensions/ # to update the timestamps so that new driver caches - without AMD drivers - will be definitely rebuilt
$ sudo umount / # umount a partition to guarantee that your changes are flushed to it
$ sudo reboot</pre>

However, in the same way as the solution&#8217;s poster, when I tried to delete the kext the Mac was throwing me the error <span class="lang:sh highlight:0 decode:true crayon-inline ">operation not allowed</span>  or something similar. Probably because in the same way as s/he, I have my disk locked as &#8220;read-only&#8221; after too many attempt of booting. Lucky, I didn&#8217;t need to mount my disk on Linux, as s/he did. I just took my disk out of my Mac and put it in a USB enclosure that I connected to another Mac with High Sierra. I have High Sierra installed in my machine with the new APFS, so that means that my disk in only readable by other Macs with High Sierra installed. Dangerous, yes, but I wanted to take advantage of the new features. Backups were invented for some reason.

From there, I just needed to performed the same commands but a little bit different. You have to take into account that in macOS your hard drive is going to mount automatically, so it wasn&#8217;t necessary to mount it like before, and you just have to run the rest of the commands with the proper path and the name of your drive. In my case my hard drive name is <span class="lang:sh highlight:0 decode:true crayon-inline ">Macintosh SSD</span> , and in this Mac there is also a <span class="lang:sh highlight:0 decode:true crayon-inline ">Macintosh SSD</span> , so when it mounted my hard drive macOS renamed it to to <span class="lang:sh highlight:0 decode:true crayon-inline ">Macintosh SSD 1</span> , In the shell you have to proper indecate the blank spaces on name and paths using the backslash symbol <span class="lang:sh highlight:0 decode:true crayon-inline ">\</span> , therefore I could access to my hard drive using the name <span class="lang:sh highlight:0 decode:true crayon-inline ">Macintosh\ SSD\ 1</span> . Mind the name of your hard drive (usually <span class="lang:sh highlight:0 decode:true crayon-inline ">Macintosh HD</span> ) and change the path in the commands in consequence.

<pre class="lang:sh decode:true" title="Deleting the kext for the discrete graphic card on my mac">$ sudo mkdir /Volumes/Macintosh\ SSD\ 1/AMD_Kexts/ # make a directory to store the AMD drivers in case you'll need them in future
$ sudo mv /Volumes/Macintosh\ SSD\ 1/System/Library/Extensions/AMD*.* /AMD_Kexts/ # move the AMD drivers
$ sudo rm -rf /Volumes/Macintosh\ SSD\ 1/System/Library/Caches/com.apple.kext.caches/ # remove the AMD drivers cache
$ sudo mkdir /Volumes/Macintosh\ SSD\ 1/System/Library/Caches/com.apple.kext.caches/ # just in case OS X will be dumb and will not recreate this directory, I am creating it for OS X
$ sudo touch /Volumes/Macintosh\ SSD\ 1/System/Library/Extensions/ # to update the timestamps so that new driver caches - without AMD drivers - will be definitely rebuilt
$ sudo umount /Volumes/Macintosh\ SSD\ 1/ # umount a partition to guarantee that your changes are flushed to it&lt;br&gt;</pre>

Now, you can take your disk, reinstall it in your Mac and begin from there. I booted to something like this<sup id="fnref-1070-1"><a class="jetpack-footnote" href="#fn-1070-1">1</a></sup>:

<div id="attachment_1075" style="width: 1090px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4422.jpg"><img class="size-full wp-image-1075" src="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4422.jpg" alt="" width="1080" height="805" srcset="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4422.jpg 1080w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4422-300x224.jpg 300w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4422-768x572.jpg 768w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4422-1024x763.jpg 1024w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4422-335x250.jpg 335w" sizes="(max-width: 1080px) 100vw, 1080px" /></a>
  
  <p class="wp-caption-text">
    Booting without kexts.
  </p>
</div>

### Using solutions 2, 3 and 4

That&#8217;s beginning&#8230; at least now I knew that my computer can be booted. Then, I decided to switch solutions and continue with the fix explained in [2,](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/) which is fully explained in [3](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44). The reason&#8230; because seemed more recent and better explained, and more stable in the long term. So, as it&#8217;s detailed in that solutions, first reset the [SMC](https://support.apple.com/en-us/HT201295) and the [NVRAM](https://support.apple.com/en-us/HT204063). Then, boot your Mac on recovery single user mode (pressing and holding <span class="lang:sh highlight:0 decode:true crayon-inline ">Cmd + S + R</span> ) and run the following commands.

<pre class="lang:sh decode:true" title="Change the gpu-power-prefs and disable SIP">$ nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
$ csrutil disable
$ reboot</pre>

Now, and since you moved the GPU kext from their original location you are going to boot to something like this.

<div id="attachment_1076" style="width: 1088px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4423.jpg"><img class="size-full wp-image-1076" src="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4423.jpg" alt="" width="1078" height="777" srcset="http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4423.jpg 1078w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4423-300x216.jpg 300w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4423-768x554.jpg 768w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4423-1024x738.jpg 1024w, http://luisspuerto.net/wp-content/uploads/2017/12/IMG_4423-347x250.jpg 347w" sizes="(max-width: 1078px) 100vw, 1078px" /></a>
  
  <p class="wp-caption-text">
    Booting normally.
  </p>
</div>

Hey!! you probably are thinking&#8230; I&#8217;ve done it, I&#8217;ve fixed. it. Yes & not. Since you&#8217;ve moved the kexts from their original place things are working more or less correctly, however, if you updated the system, you are going to probably have to move your kext and apply the solution again. For that reason, they decided to created a nicer solution implementing [GRUB](https://en.wikipedia.org/wiki/GNU_GRUB) on your start up disk and blocking the loading of those kexts on booting. GRUB is the same system you put in place when you are installing Linux in a computer and you want to have more than one OS in the machine. This solution allow us to keep the kexts in place and boot without loading them. The solution isn&#8217;t perfect, but at least is easy to implement and in case you install a system update you can easily reimplement.

At this point, I recommend to move the kexts to the original location <span class="lang:sh highlight:0 decode:true crayon-inline ">/System/Library/Extensions/</span> .

<div id="attachment_1093" style="width: 892px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-12.43.04.png"><img class="size-full wp-image-1093" src="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-12.43.04.png" alt="" width="882" height="548" srcset="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-12.43.04.png 882w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-12.43.04-300x186.png 300w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-12.43.04-768x477.png 768w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-12.43.04-402x250.png 402w" sizes="(max-width: 882px) 100vw, 882px" /></a>
  
  <p class="wp-caption-text">
    Moved kexts
  </p>
</div>

You can do it dragging and dropping those back to its original location (it&#8217;s going to ask for your password), or you just can move then with terminal:

<pre class="lang:sh decode:true" title="Moving back the kext.">$ sudo mv /AMD_Kexts/*.* /System/Library/Extensions/</pre>

#### Getting a GRUB

Now, to implement the complete solution you have to [download ubuntu](https://www.ubuntu.com/download/desktop) to take the GRUB from there. I&#8217;ve downloaded [Ubuntu 17.10](http://www.nic.funet.fi/pub/mirrors/releases.ubuntu.com/17.10/ubuntu-17.10-desktop-amd64.iso), as it&#8217;s specified in the fix. When you&#8217;ve downloaded the .ISO, you have to attach and mount it, so assuming that you have the ISO in downloads:

<pre class="lang:sh decode:true" title="attaching ubuntu iso">$ hdiutil attach -nomount ~/Downloads/ubuntu-17.10-desktop-amd64.iso
</pre>

Which probably turns something like this:

<pre class="lang:sh decode:true">/dev/disk2              Apple_partition_scheme             
/dev/disk2s1            Apple_partition_map             
/dev/disk2s2            Apple_HFS</pre>

The disk number could be different, for example in my case the first time was disk3, but mounted a second time and was disk2. Depends on how many disks have you mounted before.

Now you can finally mount the ISO with the following commands:

<pre class="lang:sh decode:true" title="mounting the ubuntu disk. ">$ mkdir /tmp/ubuntu 
$ mount -t cd9660 /dev/disk2 /tmp/ubuntu/
$ open /tmp/ubuntu/shell</pre>

#### Preparing the USB stick and editing GRUB file

You have to [format you USB stick to FAT32](https://support.apple.com/kb/PH22241?locale=en_GB) and it&#8217;s recomendable to name it <span class="lang:sh highlight:0 decode:true crayon-inline ">RESCUE</span>  to easy things using this guide, then copy the <span class="lang:sh highlight:0 decode:true crayon-inline ">EFI</span>  and <span class="lang:sh highlight:0 decode:true crayon-inline ">boot</span>  folders from the ISO to your USB stick root.

<div id="attachment_1079" style="width: 892px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-13.38.12.png"><img class="size-full wp-image-1079" src="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-13.38.12.png" alt="" width="882" height="548" srcset="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-13.38.12.png 882w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-13.38.12-300x186.png 300w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-13.38.12-768x477.png 768w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-13.38.12-402x250.png 402w" sizes="(max-width: 882px) 100vw, 882px" /></a>
  
  <p class="wp-caption-text">
    Folders you have to copy
  </p>
</div>

When you have those folders on your USB stick, you have to edit the file <span class="lang:sh highlight:0 decode:true crayon-inline ">/RESCUE/boot/grub/grub.cfg</span> . I like Atom to edit, but perhaps you don&#8217;t have it installed so if you type:

<pre class="lang:sh decode:true">$ open /Volumes/RESCUE/boot/grub/grub.cfg</pre>

Your default text editor will open. In case you want to be sure and open with Text Edit:

<pre class="lang:sh decode:true">$ open -a TextEdit /Volumes/RESCUE/boot/grub/grub.cfg</pre>

Then you have to delete all the file content and paste the following.

<pre class="toolbar:1 toolbar-overlay:false lang:sh highlight:0 decode:true" title="grub.cfg">if loadfont /boot/grub/font.pf2 ; then
    set gfxmode=auto
    insmod efi_gop
    insmod efi_uga
    insmod gfxterm
    terminal_output gfxterm
fi

set menu_color_normal=white/black
set menu_color_highlight=black/light-gray

set timeout=0
menuentry "macOS" {
    outb 0x728 1
    outb 0x710 2
    outb 0x740 2
    outb 0x750 0
    outb 0x714 0xFF
    exit
}
</pre>

Take into account that is a specific GRUB configuration for High Sierra and with a single OS installed. If you have more that one OS or you are under other OS check [this](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44#34-edit-the-grubcfg-file). Note that I&#8217;ve added a line to the original proposed GRUP file at almost at the end. This is because someone suggest that line on this [thread](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/page-4#post-25518184). I&#8217;ve also set the `timeout` variable to zero, since I don&#8217;t want to push enter to continue or wait 10 seconds. I don&#8217;t have anything to chose from.

Save the file and let&#8217;s copy now to you Mac. Or perhaps, you can test if this works properly rebooting using the USB stick. For that you have to reboot and then push `alt` after you hear the chime, introduce the USB stick and choose it on the menu. You should be able to reboot normally.

#### Making it permanent

Now you can make this permanent and without need the USB. You run on terminal the following commands with your USB still plugged and assuming that you named it <span class="lang:sh highlight:0 decode:true crayon-inline">RESCUE</span> .

<pre class="lang:sh decode:true" title="Installing GRUP in your computer">$ cd /Volumes
$ sudo mkdir efi
$ sudo mount -t msdos /dev/disk0s1 /Volumes/efi
$ sudo mkdir /Volumes/efi/boot
$ sudo mkdir /Volumes/efi/EFI/grub
$ sudo cp -R /Volumes/RESCUE/boot/ /Volumes/efi/boot
$ sudo cp -R /Volumes/RESCUE/EFI/boot/ /Volumes/efi/EFI/grub
$ sudo bless --folder=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot
$ sudo bless --mount=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot</pre>

Now&#8230; you can unmount the USB and boot without it.

#### Preventing GPU from waking up from sleep

When you are under High Sierra, the next step doesn&#8217;t really work, and even installing this kext is going to return to a black screen when you return form sleep. To prevent this, you can just change the way your Mac sleep and make it hibernate. If you have a SSD in place, like I have, the difference on time between waking up from a normal sleep than from hibernation is going to be neglectable.  The only real difference is, your computer isn&#8217;t going to wake up when you lift the lid and you have to push the on/off button to wake your Mac up. To set this up you have to run on terminal:

<pre class="lang:sh decode:true">$ sudo pmset -a hibernatemode 25
</pre>

If you want to return to the normal sleep mode you set in the following way:

<pre class="lang:sh decode:true">$ sudo pmset -a hibernatemode 3</pre>

Anyhow, I decided to apply the solution as it&#8217;s explained [here](https://github.com/blackgate/AMDGPUWakeHandler) and I even [created a kext myself for High Sierra](http://luisspuerto.net/wp-content/uploads/2017/12/AMDGPUWakeHandler.kext_.zip). Just in hopes that in a close future things improve I can normally sleep. If you download the kext you just have to unzip it and then copy to <span class="lang:sh highlight:0 decode:true crayon-inline ">/Library/Extensions</span>  and run the following commands.

<pre class="lang:sh decode:true">$ sudo chmod -R 755 /Library/Extensions/AMDGPUWakeHandler.kext
$ sudo chown -R root:wheel /Library/Extensions/AMDGPUWakeHandler.kext
$ sudo touch /Library/Extensions</pre>

And reboot.

Now, you are done.

#### Reimplementing the fix

If you need to reimplement the solution because you updated the system you are going to need to just bless GRUB disk again –make it a bootable disk. You just boot from the rescue USB stick and run the following commands on terminal<sup id="fnref-1070-2"><a class="jetpack-footnote" href="#fn-1070-2">2</a></sup>.

<pre class="lang:sh decode:true " title="Reblessing GRUB">$ cd /Volumes
$ sudo mkdir efi
$ sudo mount -t msdos /dev/disk0s1 /Volumes/efi
$ sudo bless --folder=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot
$ sudo bless --mount=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot</pre>

## Final note

As I&#8217;ve mentioned earlier, there are only two caveats to this solution if you are under High Sierra. One is that you lose the ability to sleep you computer normally and you have to hibernate. Nothing really serious if you have an SSD. The other is more serious&#8230; or at least is going to affect you more in your daily basics. You are not going to be able to control the brightness of your computer anymore. It&#8217;s something that [MacRumors community](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/page-5) is trying to fix.

Anyhow, the weir way that High Sierra manages the bright isn&#8217;t something new. And during the betas, some people commented about this on the developers forum as well, [here](https://forums.developer.apple.com/thread/80705) and [here](https://forums.developer.apple.com/thread/82500).

### Why this happens?

You&#8217;re probably wondering yourself why this is happening to your computer although if you have researched a little bit about the problem you probably already know what is the problem. Really bad quality graphic chips that due to heat in the computer finally fails. You can learn a little bit in this video.

<div class="jetpack-video-wrapper">
  <span class="embed-youtube" style="text-align:center; display: block;"></span>
</div>

To sum it up&#8230; AMD / Nvidia chips mounted in logic board of this model are really bad and I guess most of them are faulty. The lack of proper ventilation on Macs, specially in this models, doesn&#8217;t improve things and make then even more prone to this kind of issues. There is a technique called reballing, which basically consist repair the littles balls that the chip uses to connect to the logic board. However, this is just a temporal solution since the problem is in the chip itself. Reballing works just because when you heat up the chip to reball it you mess with some of the internal parts of the chip –I think they call this [reflow](https://en.wikipedia.org/wiki/Reflow_soldering)– and that makes it to work properly again, but after a while the problem is going to return. I&#8217;ve read that some people just put the board in the oven at around 200ºc to fix the issue, but I guess doesn&#8217;t last a lot either.

The only long working solution is just get another board, or just another chip, and pray that this time it isn&#8217;t faulty. Change the board isn&#8217;t complicated, at least not incredible. But invest 400€, or even more, in a 6 years old computer is something you really have to ponder.

Anyway, the only ones here to blame is AMD / Nvidia and or course Apple, they knew the issue all this time and they&#8217;ve done nothing to fix it. And for that reason [Apple is being sued](https://www.macworld.co.uk/news/mac/macbook-pro-graphics-failures-addressed-by-apple-repair-programme-3497935/).

It&#8217;s time to think if it&#8217;s really worth to spend the money Apple tags their computers. I&#8217;m really willing pay, but I expect a top-notch silent computer and service.  Besides, it seems that Apple has forgotten that professional customers really need professional computers and the last batch seems that [isn&#8217;t up to the mark](https://www.theverge.com/2016/11/7/13548052/the-macbook-pro-lie) and Apple has received a lot of backlash for it. So big has been the adverser reaction that Apple has to even make an [statement](https://www.macworld.co.uk/news/mac/new-macbook-pro-2018-rumours-uk-release-date-price-features-specs-3661715/) addressing it.

### Related issues?

While I was in the US we got a beautiful 24&#8243; cinema display that I connected to my machine and we enjoyed a lot. At that moment, I was concerned by the _extensive_ use I was doing of the dGPU when connected to the external screen and I asked in the Apple communities about the issue. They replied that _[it was the intended way to use the computer and I shouldn&#8217;t worry](https://discussions.apple.com/thread/7963970)_. I don&#8217;t blame then, because what they told me, was true, it was/is the normal way to use the computer and how it was designed.  Perhaps bad designed, or at least not really thoroughly thought. When you connect the computer to a external screen you trigger the use of the dGPU, not because you really need it –you can perfectly work with the integrated as other models do, but because the thunderbolt connection where you connect the external screen leads directly to the graphic card there is no way to avoid it&#8217;s use. This cause an increase of temperature of more or less 20º from the normal operation, just for connecting a external display, and  in my humble opinion there is nothing of _extensive_ use in connecting it to a external display.

In the same way&#8230; An almost month ago I installed High Sierra and I noticed a weird error related to graphics on the console:

<div id="attachment_1090" style="width: 1147px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-00.23.58.png"><img class="size-full wp-image-1090" src="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-00.23.58.png" alt="" width="1137" height="792" srcset="http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-00.23.58.png 1137w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-00.23.58-300x209.png 300w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-00.23.58-768x535.png 768w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-00.23.58-1024x713.png 1024w, http://luisspuerto.net/wp-content/uploads/2017/12/Screen-Shot-2017-12-04-at-00.23.58-359x250.png 359w" sizes="(max-width: 1137px) 100vw, 1137px" /></a>
  
  <p class="wp-caption-text">
    [ERROR] – Unknown CGXDisplayDevice: 0x41dcd00
  </p>
</div>The error is still there right now, and I told Apple about it 

[when I noticed](https://twitter.com/lpuerto/status/928885729916342272). They contacted me and collected some data. They never return to me about this issue. Now, I wondering if this two issues are connected and High Sierra accelerated the degradation process of the Nvidia / AMD chip.

<li id="fn-1070-1">
  To be entirely honest I don&#8217;t remember if after deleting the kexts I was able to boot directly to <a href="#attachment_1075">the striped desktop</a> or I needed to run the commands in the beginning of solution <a href="http://luisspuerto.net/2017/12/my-macbook-pro-late-2011s-discrete-graphics-card-said-chao-again/#using-solutions-2-3-and-4">2</a>. <a href="#fnref-1070-1">&#x21a9;</a>
</li>
<li id="fn-1070-2">
  Today, 8th of December 2017, I decided to install the update to 10.13.2. It worked really well, and even booted without needed to apply the fix again. In other words, the discrete GPU was working. However, after a while fail, and took me more than what I wanted to reestablish everything. So my recommendation is, if you update, apply the fix as soon as possible, but cause sooner or later things are going to go south and it&#8217;s going to take you even more time to fix it. In my case I needed to apply the solution almost from the very beginning and a NVRAM reset was necessary to be able to operate again the computer. Good luck! <a href="#fnref-1070-2">&#x21a9;</a></fn></footnotes>