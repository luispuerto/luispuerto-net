---
id: 1114
title: Disconnecting the dGPU in a late 2011 MacBook Pro –another way
date: 2017-12-08T17:09:29+00:00
author: Luis Puerto
layout: post
guid: http://luisspuerto.net/?p=1114
permalink: /2017/12/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-another-way/
image: /wp-content/uploads/2017/12/maxresdefault.jpg
categories:
  - Personal
  - Technology
tags:
  - dGPU
  - graphic card
  - high sierra
  - how to
  - macOS
---
As I&#8217;ve told in the [previous post](http://luisspuerto.net/2017/12/my-macbook-pro-late-2011s-discrete-graphics-card-said-ciao-again/), the discrete graphic car of my MacBook Pro late 2011 is faulty and can be trusted any more. So I decided to disconnect it.

Yesterday, I updated the system to 10.13.2 and although in the beginning everything was working fine without enforcing the dGPU disconnection, the graphic card later failed and I had to apply the fix again. For some reason the fix didn&#8217;t working as well as it was working before so after too much booting and trying I decided to take the middle way. Besides, the GRUB soliton was nice, but it made the booting much more slower.

What I&#8217;ve done basically is move the kexts from the extensions folder to other place and apply the wake up handle.

In other words this are the steps. Please keep in mind that I&#8217;m running High Sierra in my machine.

  1. Reset the [SMC](https://support.apple.com/en-us/HT201295) and the [NVRAM](https://support.apple.com/en-us/HT204063).
  2. Boot your Mac on recovery single user mode (pressing and holding <span class="lang:sh highlight:0 decode:true crayon-inline">Cmd + S + R</span> ) and run the following commands. <pre class="lang:sh decode:true">$ nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
$ csrutil disable
$ reboot</pre>

  3. You are going to reboot to your normal desktop. Now you can move the GPU kexts to other place: <pre class="lang:sh decode:true" title="Moving the GPU kexts">$ sudo mkdir /AMD_Kexts/ # make a directory to store the AMD drivers in case you'll need them in future
$ sudo mv /System/Library/Extensions/AMD*.* /AMD_Kexts/ # move the AMD drivers
$ sudo rm -rf /System/Library/Caches/com.apple.kext.caches/ # remove the AMD drivers cache
$ sudo mkdir /System/Library/Caches/com.apple.kext.caches/ # just in case OS X will be dumb and will not recreate this directory, I am creating it for OS X
$ sudo touch /System/Library/Extensions/ # to update the timestamps so that new driver caches - without AMD drivers - will be definitely rebuilt</pre>

  4. Take the [AMDGPUWakeHandler.kext](http://luisspuerto.net/wp-content/uploads/2017/12/AMDGPUWakeHandler.kext_.zip) and copy it to <span class="lang:sh highlight:0 decode:true crayon-inline">/Library/Extensions</span>  then run the following commands <pre class="lang:sh decode:true" title="Applying the AMDGPUWakeHandler.kext">$ sudo chmod -R 755 /Library/Extensions/AMDGPUWakeHandler.kext
$ sudo chown -R root:wheel /Library/Extensions/AMDGPUWakeHandler.kext
$ sudo touch /Library/Extensions</pre>

  5. Make sure to have change the way the system sleeps: <pre class="lang:sh decode:true" title="Making the system hibernate by default">$ sudo pmset -a hibernatemode 25</pre>

  6. Now you can reboot.
  7. Perhaps it&#8217;s recomendable to re-enable the SIP. To do that just boot your Mac on recovery single user mode (pressing and holding <span class="lang:sh highlight:0 decode:true crayon-inline">Cmd + S + R</span> ) and run: <pre class="lang:sh decode:true " title="Reenabling SIP">$ csrutil enable
$ reboot</pre>

This solution perhaps it&#8217;s a little bit easier, but has the disadvantage that you need to apply it all every time you update the system.

Anyway, I recommend to read the [previous post](http://luisspuerto.net/2017/12/my-macbook-pro-late-2011s-discrete-graphics-card-said-ciao-again/) to understand fully what is going on.