---
id: 391
title: Installing Raspbian in a Raspberry without keyboard and external screen
date: 2017-10-14T18:47:55+00:00
author: Luis Puerto
layout: post
guid: http://luisspuerto.net/?p=391
permalink: /2017/10/installing-raspbian-stretch-in-a-raspberry-without-keyboard-and-external-screen/
image: /wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03.png
categories:
  - Professional
  - Technology
tags:
  - how to
  - linux
  - raspberry pi
  - raspbian
---
<p dir="ltr">
  A few months ago we acquired a <a href="https://www.raspberrypi.org/products/raspberry-pi-3-model-b/" target="_blank" rel="noopener">Raspberry Pi 3 Model 3</a> in the hopes to play a little bit around with it to learn some Linux and perhaps to use it as a computer to host rStudio Server and run some scripts on it. But things have been little by little been delayed, mainly because we didn&#8217;t have at hand a proper monitor nor USB keyboard.
</p>

So… last week I finally decided to do something with the Pi and started to research if it would be possible to do an install without keyboard and screen&#8230; and it turned out that it&#8217;s totally possible.

First I want to mention that I&#8217;m doing this from MacBook Pro with El Capitan installed, and I&#8217;m going to install the Stretch version of Raspbian, the last one at the moment. The way we are going to configure Raspbian remotely is connecting to the Pi through [SSH](https://en.wikipedia.org/wiki/Secure_Shell) (Secure SHell) and we are going to access to the remote desktop using [VNC](https://en.wikipedia.org/wiki/Virtual_Network_Computing) (Virtual Network Computing).

## Getting ready

The first thing you need to do is download the OS image from the <a href="https://www.raspberrypi.org/downloads/raspbian/" target="_blank" rel="noopener">Raspberry Pi site</a>, where at least in my case I chose the desktop one, and you should too if you want to have a desktop where you log in later, even if it&#8217;s just remotely.

To flash the OS in the memory card you can just use [Etcher](https://etcher.io), what makes thinks a little bit easier. You just have drag and drop the unzipped image to the app window and then choose the flash card you want to put the OS in, after you&#8217;ve inserted it in the computer. Once you have flashed the card and to make it possible to log in after in the OS from the ssh console you have to create a file named &#8220;ssh&#8221; without extension in the root of the FAT partition of the card. The FAT partition is the one that you can access from your Mac after you flash OS in the card. You can do it in the terminal using this command:

<pre class="lang:sh decode:true">$ touch /Volumes/boot/ssh</pre>

<div id="attachment_464" style="width: 892px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-07.52.26.png"><img class="size-full wp-image-464" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-07.52.26.png" alt="" width="882" height="548" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-07.52.26.png 882w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-07.52.26-300x186.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-07.52.26-768x477.png 768w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-07.52.26-402x250.png 402w" sizes="(max-width: 882px) 100vw, 882px" /></a>
  
  <p class="wp-caption-text">
    File named ssh with extension on the FAT partition.
  </p>
</div>

From then on, you just have to put the SD card in the Pi and wait for the rest of the installation to finish, which probably won&#8217;t take more than a couple of minutes, and connect the Pi through LAN to the same network you are connected. In my case I connected the Pi to one of my Airport Express, but you can plug it in directly to your router.

## Connecting to the Pi

Now you just have to open terminal and type

<pre class="lang:sh decode:true" title="starting the ssh connection">$ ssh pi@192.168.1.X</pre>

Where ssh is the command in terminal to establish the ssh connection, pi is the default username in  on Raspbian, and 192.168.1.X is the usual IP the router is going to assign to the IP, being the X a number between 1 and 255 (well actually from 2 to 255, because the 1 usually is the router). Please, be aware that your IP could be totally different and depends on how your router is configure.

To find out what are the IP of your Pi you just need to log into your router and check the device list or IP table. Other option is just download an IP scan software (like [Angry IP Scanner](http://angryip.org)) and run it on your computer to see the different IPs of the devices on your network. You can also try to establish contact using the hostname instead of the IP, which in this case is like this:

<pre class="lang:sh decode:true" title="ssh to pi using hostname ">$ ssh pi@raspberrypi.local</pre>

Then you&#8217;ll be faced with a screen similar to this one.

<div id="attachment_417" style="width: 692px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.34-1.png"><img class="wp-image-417 size-full" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.34-1.png" alt="" width="682" height="478" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.34-1.png 682w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.34-1-300x210.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.34-1-357x250.png 357w" sizes="(max-width: 682px) 100vw, 682px" /></a>
  
  <p class="wp-caption-text">
    Login into the Pi using the SSH connection
  </p>
</div>

Where you are going to be asked about adding the key fingerprint of your Pi to your _knowhost_ file of your Mac for secure connections (yes, you want to continue connecting) and then you&#8217;ll be prompted for the password of the Pi. The default password is: _raspberry_.

### Configuring the Pi

Now, you are logged on the terminal/shell of your Pi, and you can start configuring using shell commands. First of all is to login as root user to make things a little bit easier and then enter in the _configuration wizard_ (or software configuration tool) using the command <span style="font-family: 'Courier 10 Pitch', Courier, monospace; font-size: 12.800000190734863px; background-color: #f4f4f4;">raspi-config </span> .

<pre class="lang:sh decode:true" title="root and configuration wizard">$ sudo su # if you want to login as root and don't type sudo in every command
$ sudo raspi-config</pre>

<div id="attachment_415" style="width: 692px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.54.png"><img class="wp-image-415 size-full" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.54.png" alt="" width="682" height="478" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.54.png 682w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.54-300x210.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.44.54-357x250.png 357w" sizes="(max-width: 682px) 100vw, 682px" /></a>
  
  <p class="wp-caption-text">
    Raspberry config tool
  </p>
</div>

Now, from here, you can change your password, the hostname, etc as you can see in the screenshot. I recommend you to change your password and leave your hostname as it is, unless you have more than one pi in your network. You should activate the VCN interface, in the _Interface Options_ and set your _Localization Options_, or at least take a look to check that everything is OK. When you hit _Finish_.

If you want to connect your Pi to your network via wi-fi, you can also do it. You can find more info in this [tutorial](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md), but basically you first scan for wi-fi networks.

<pre class="lang:sh decode:true" title="scanning for wifi networks">$ sudo iwlist wlan0 scan</pre>

And then you edit the <span class="lang:sh highlight:0 decode:true crayon-inline ">wpa_supplicant.conf</span> configuration file with the info of the network you want to connect.

<pre class="lang:sh decode:true" title="opening nano for editing file wpa_supplicant.conf">$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf</pre>

Using the previous command you open the config file on nano (a really simple editor) and you add at the end of the file.

<pre class="lang:vim highlight:0 decode:true" title="Editing wpa_supplicant/wpa_supplicant.conf">network={
    ssid="your wifi network name"
    psk="the password for your network"
}</pre>

To save you press crtl-o and to exit crtl-x.

Now you can disconnect the LAN cable from your Pi to the router if you wish and restart the SSH connection using the command <span class="lang:sh highlight:0 decode:true crayon-inline">exit</span>  and reconnect whether using the new IP that was assigned to the wi-fi connection or again the host (raspberrypi.local if you didn&#8217;t change it). Sometimes you need to reboot the Pi, to make it work.

You can see the wi-fi config with the following command.

<pre class="lang:sh decode:true" title="showing the wi-fi config">$ ifconfig wlan0</pre>

Perhaps, it&#8217;s also a good idea [update and upgrade the system](https://www.raspberrypi.org/documentation/raspbian/updating.md) before you continue.

<pre class="lang:sh decode:true" title="updating and upgrading the system">$ sudo apt-get update
$ sudo apt-get dist-upgrade</pre>

### Installing the VNC Server

When you are done, you can install the VNC server in the Pi. You can find a more detailed tutorial [here](https://www.raspberrypi.org/documentation/remote-access/vnc/), but basically you type in the SSH connection:

<pre class="lang:sh decode:true" title="Installing VNC">$ sudo apt-get install realvnc-vnc-server realvnc-vnc-viewer</pre>

You also have to install the [VNC client](https://www.realvnc.com/download/viewer/) in your Mac, set it up and then log into the Pi using as username _pi_ and as password _raspberry_ or the one you&#8217;ve set up.

<div id="attachment_412" style="width: 1690px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03.png"><img class="wp-image-412 size-full" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03.png" alt="" width="1680" height="1050" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03.png 1680w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03-300x188.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03-768x480.png 768w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03-1024x640.png 1024w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03-1216x760.png 1216w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-13.25.03-400x250.png 400w" sizes="(max-width: 1680px) 100vw, 1680px" /></a>
  
  <p class="wp-caption-text">
    Remote desktop of Raspberry Pi.
  </p>
</div>

<div id="attachment_422" style="width: 276px" class="wp-caption alignleft">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.14.10.png"><img class="size-full wp-image-422" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.14.10.png" alt="" width="266" height="110" /></a>
  
  <p class="wp-caption-text">
    VNC server dialog.
  </p>
</div>

There are further configuration you can do in the server-side of the VNC connection, as it&#8217;s explained in the tutorial. I would recommend you to activate the _experimental direct capture mode_ to be able to see apps that are directly render remotely (like Minecraft and I believe some of the settings windows). To do that you go to the VNC server dialog in your Pi and you click on the _Menu_ and go to** **_Options > Troubleshooting_ and select _Enable experimental direct capture_ mode.

<div id="attachment_423" style="width: 825px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.18.32.png"><img class="size-full wp-image-423" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.18.32.png" alt="" width="815" height="493" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.18.32.png 815w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.18.32-300x181.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.18.32-768x465.png 768w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-15-at-15.18.32-413x250.png 413w" sizes="(max-width: 815px) 100vw, 815px" /></a>
  
  <p class="wp-caption-text">
    Menu in the VNC server on Pi
  </p>
</div>

Now you can access completely to your Raspberry Pi desktop and you didn&#8217;t need and won&#8217;t need a keyboard or an additional screen.

Addendum: There is [another options to access to the remote desktop of your Pi](https://smittytone.wordpress.com/2016/03/02/mac_remote_desktop_pi/), that doesn&#8217;t need to install any additional software on your Mac, since the connection is carried out by the Finder.app, but they aren&#8217;t as optimal as this one that it&#8217;s recommended by the official Raspberry Pi documentation.

### Additional configuration

Perhaps you are interested into mount NTFS, exFAT and samba volumes, i.e. hard drives that whether you connect to the USB port or you connect through your local network.

The first two things are pretty easy to make it to happen. You just have to type in the SSH console or in the shell on the desktop:

<pre class="lang:sh decode:true" title="Install NTFS and exFAT support">$ sudo apt-get install ntfs-3g
$ sudo apt-get install exfat-fuse</pre>

<div id="attachment_454" style="width: 263px" class="wp-caption alignleft">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-16-at-17.59.20.png"><img class="wp-image-454" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-16-at-17.59.20.png" alt="" width="253" height="436" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-16-at-17.59.20.png 396w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-16-at-17.59.20-174x300.png 174w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-16-at-17.59.20-145x250.png 145w" sizes="(max-width: 253px) 100vw, 253px" /></a>
  
  <p class="wp-caption-text">
    Mounting a usb stick on the Pi.
  </p>
</div>

With this you are going to mount more or less any drive you want. Usually drives mount automatically, when you connect them to the Pi, and you can find them on <span class="tab-size:2 lang:sh highlight:0 decode:true crayon-inline">/home/pi/Media/pi</span> .

Now please, remember that like in Mac you have you unmount your units &#8211; volumes before you extract them from the USB port. You do so you can clip on the _eject_ icon on the top right corner of the desktop.

If you wanted to do the same in the terminal or in the SSH console you have to use the command <span class="lang:sh highlight:0 decode:true crayon-inline">mount</span>  and <span class="lang:sh highlight:0 decode:true crayon-inline">umount</span>  respectively. However you have to be aware that you have to create a directory where you wish to mount the volume before you execute the command <span style="font-family: 'Courier 10 Pitch', Courier, monospace; font-size: 12.800000190734863px; background-color: #f4f4f4;">mount</span> . A little bit more info about how to mount exFAT and NTFS can be found [here](https://raspberrypi.stackexchange.com/questions/32890/how-to-mount-ntfs-drive-on-a-raspberry-pi-b) and [here](https://miqu.me/blog/2015/01/14/tip-exfat-hdd-with-raspberry-pi/).

&nbsp;

&nbsp;

<pre class="lang:sh decode:true" title="mounting and unmount volumes">sudo fdisk -f # this is to list all the volumes available
mkdir /mnt/usb # directory where you are going to mount
sudo mount /dev/sda1 /mnt/usb # mounting the unit
sudo ntfs-3g /dev/sda1 /mnt/usb # alternative way of mounting if it's NTFS 
sudo umount /mnt/usb # to unmount the volume</pre>

Now, if you want to mount some samba shares you have connected in your network, like in your router or in the Time Capsule, like it&#8217;s my case you have to proceed as follows, being aware that you have to create the mounting (point) directory before.

<pre class="lang:sh decode:true">$ sudo mount -t cifs //192.168.1.X/share /mnt/box/</pre>

Change the IP for the one of your sharing device and &#8220;share&#8221; for the path to the volume / hard drive in that device. If the name of the volume you are sharing in that device has a space, you have to introduce <span class="lang:sh highlight:0 decode:true crayon-inline">\040</span> in the spaces. For example is the name of your share is &#8220;_share with spaces_&#8221; and you want to mount in the route <span class="lang:sh highlight:0 decode:true crayon-inline">/mnt/my share</span> you type.

<pre class="lang:sh decode:true" title="mounting shares with spaces">$ sudo mount -t cifs //192.168.1.2/share\040with\040spaces /mnt/my\040share/
</pre>

Now, if you want to mount any those shares on boot, to make it available all the time for other apps, you have to modify <span class="lang:sh highlight:0 decode:true crayon-inline">fstab</span> file. To do that you have to run this command on the shell.

<pre class="lang:sh decode:true" title="modifying fstab">$ sudo nano /etc/fstab</pre>

And now you add to the end of the file:

<pre class="lang:vim highlight:0 decode:true" title="fstab modification">//192.168.1.2/share\040with\040spaces /mnt/my\040share/ cifs user=youruser,pass=yourpassword,rw,uid=1000,iocharset=utf8,sec=ntlm 0 0</pre>

And as previously you did in nano, to save you press <span class="lang:sh highlight:0 decode:true crayon-inline ">crtl-o</span>  and to exit <span class="lang:sh highlight:0 decode:true crayon-inline ">crtl-x</span> .

You have to also tell the pi to wait until the network it&#8217;s up to boot. For that you have to run again raspi-config.

<pre class="lang:sh decode:true " title="sudo raspi-config">$ sudo raspi-config</pre>

And in boot options you select _Wait for Network at Boot_.

And that&#8217;s it, now you have a Raspberry Pi to play around.