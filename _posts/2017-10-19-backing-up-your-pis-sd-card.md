---
id: 496
title: 'Backing up your Pi&#8217;s SD card'
date: 2017-10-19T19:24:16+00:00
guid: http://luisspuerto.net/?p=496
permalink: /2017/10/backing-up-your-pis-sd-card/
categories:
  - Personal
  - Professional
  - Technology
tags:
  - linux
  - macOS
  - raspberry pi
  - raspbian
---
I&#8217;ve been trying make something work in my Pi, but so far I&#8217;ve just messing things up and I had to install Raspbian a couple of times from scratch. For that reason I&#8217;ve decided to make a copy of the SD card before I repeat the process and try to figure out what is going on.

To make a back up of your SD card using macOS I&#8217;ve found two options.

  * On one hand, you can install [dd utility](https://github.com/thefanclub/dd-utility) and use a graphical interface to make the backup to happen.
  * On the other, you can use the <span class="lang:sh highlight:0 decode:true crayon-inline ">dd</span>   command [[ref.](https://ss64.com/osx/dd.html)] on the shell to create the backup.

If want to try the dd utility you can download it from the site and normally installing it on macOS or you can install it using [Homebrew](https://brew.sh) [Cask](https://caskroom.github.io) and the following command.

<pre class="lang:sh decode:true" title="installing the dd utility">$ brew cask install dd-utility</pre>

### Backing up

When you executes the dd utility you only have two options: backup or restore a card with an image. Here you don&#8217;t have a lot of complication. However, you have to take into account that at least in my case the internal card reader of the Mac doesn&#8217;t work with this app in particular, and I need to insert the app in a external usb SD card reader.

<div id="attachment_510" style="width: 542px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-19.12.08.png"><img class="size-full wp-image-510" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-19.12.08.png" alt="" width="532" height="351" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-19.12.08.png 532w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-19.12.08-300x198.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-19.12.08-379x250.png 379w" sizes="(max-width: 532px) 100vw, 532px" /></a>

  <p class="wp-caption-text">
    dd utility, backup or restore
  </p>
</div>

I did my backup with the dd utility first, but in the end of the process it gave me an error message. I have the back up file in my hard drive and it works, or at least it unzips and I can mount the image.

To be sure that everything is OK and I I&#8217;m not less without a back up, I decided to do it with the shell commands as it&#8217;s explained [here](https://thepihut.com/blogs/raspberry-pi-tutorials/17789160-backing-up-and-restoring-your-raspberry-pis-sd-card).

The commands are the following ones:

First we use <span class="lang:sh highlight:0 decode:true crayon-inline ">diskutil</span>  [[ref.](https://ss64.com/osx/diskutil.html)] to see all the volumes connected.

<pre class="lang:sh decode:true" title="listing volumes">$ diskutil list</pre>

<div id="attachment_503" style="width: 972px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-18.17.45.png"><img class="size-full wp-image-503" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-18.17.45.png" alt="" width="962" height="757" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-18.17.45.png 962w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-18.17.45-300x236.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-18.17.45-768x604.png 768w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-19-at-18.17.45-318x250.png 318w" sizes="(max-width: 962px) 100vw, 962px" /></a>

  <p class="wp-caption-text">
    List of volumes
  </p>
</div>

Now you just need to choose what are you going to back up and where using the <span class="lang:sh highlight:0 decode:true crayon-inline ">dd</span>  command.

<pre class="lang:r decode:true" title="making the backup with dd command">$ sudo dd if=/dev/diskX of=users/YOURUSERNAME/Downloads/SDCardBackup.dmg</pre>

You can see that I haven&#8217;t used the same path that they use in the linked instructions because using the <span class="lang:sh highlight:0 decode:true crayon-inline ">~</span>  tilde symbol, denoting my home folder, gave me an error <span class="lang:sh highlight:0 decode:true crayon-inline ">dd: ~/SDCardBackup.dmg: No such file or directory</span>  so I use the full path. You have to change also the <span class="lang:sh highlight:0 decode:true crayon-inline ">/dev/diskX</span>  path for the <span class="lang:sh highlight:0 decode:true crayon-inline ">diskX</span>  that represent your SD card. In my case was the number 4, but in your case could be different.

<span class="lang:sh highlight:0 decode:true crayon-inline">dd</span> command doesn&#8217;t produce any feedback of how much of the process is left. The only indication that is running is that the prompt hasn&#8217;t returned to be the normal terminal prompt. Wait until the process finish and don&#8217;t close the window or kill he process. It probably going to take time, much more time that with the previous method. At least in my case it started at 17.48 and it&#8217;s 19.22 and it hasn&#8217;t finished yet for a 32GB SD card. I think that it check all the sectors of the card and copy them to the backup.

**EDIT**: I got bored of the slowness of the process and I cancel it after a while and try to implement [this suggestion](http://daoyuan.li/solution-dd-too-slow-on-mac-os-x/) about raw volumes. So the code would be like this now:

<pre class="lang:sh decode:true" title="improvement on speed. ">$ sudo dd if=/dev/rdiskX of=users/YOURUSERNAME/Downloads/SDCardBackup.dmg</pre>

### Restoring

To restore you first have to unmount your SD card.

<pre class="lang:sh decode:true" title="unmounting the SD card">$ diskutil unmountDisk /dev/diskX</pre>

To following write the image in the SD card with this command.

<pre class="lang:sh decode:true" title="Restoring the SD card">$ sudo dd if=~/SDCardBackup.dmg of=/dev/diskX
</pre>

And finally you eject the card with using this.

Once it has finished writing the image to the SD card, you can remove it from your Mac using:

<pre class="lang:sh decode:true " title="eject the SD card">$ sudo diskutil eject /dev/rdisk3</pre>

So&#8230; you are ready now to mess up with your Raspberry Pi without worrying.
