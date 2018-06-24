---
id: 1433
title: Convert your Pi in an AirPlay receiver
date: 2018-01-18T14:50:50+00:00
guid: http://luisspuerto.net/?p=1433
permalink: /2018/01/convert-you-pi-in-an-airplay-receiver/
image: /wp-content/uploads/2018/01/speakers-raspberry.jpg
categories:
  - Personal
  - Technology
tags:
  - macOS
  - raspberry pi
---
I recently needed to convert my Raspberry Pi 3 into a AirPlay receiver because I didn&#8217;t have any of my [AirPort Express](https://www.apple.com/lae/airport-express/) at hand and I wanted to play just music –not the entire collection of sounds my computer produce– on one of my old [Hi-Fi Self Stereo System](https://en.wikipedia.org/wiki/Shelf_stereo). So I searched on the internet and saw that it was totally possible to do it and I try it. I used as a source this [article](https://pimylifeup.com/raspberry-pi-airplay-receiver/) and I want share with you what steps I specifically followed.

# Preliminaries

Let&#8217;s download some packages we are going to need as dependencies.

<pre class="lang:sh decode:true" title="Update the system and download some dependencies">$ sudo apt-get update && upgrade</pre>

<pre class="lang:sh decode:true" title="Update the system and download some dependencies">$ sudo apt-get install autoconf libtool libdaemon-dev libasound2-dev libpopt-dev libconfig-dev avahi-daemon libavahi-client-dev libssl-dev</pre>

# Shairport sync

To convert our Pi in a AirPlay device we need a software that is call <a href="https://github.com/mikebrady/shairport-sync" target="_blank" rel="noopener">Shairport Sync</a>. We are going to need to compile it for our system, so we need to download the source code and then compile. To download it just run on the Pi&#8217;s terminal:

<pre class="lang:sh decode:true" title="Download Shairport sync">$ cd ~/Downloads # I like to download my soft in Downloads folder.</pre>

<pre class="lang:sh decode:true" title="Download Shairport sync">$ git clone https://github.com/mikebrady/shairport-sync.git</pre>

## Compile

Now that we have download the shairport-sync repository to our Pi, we can proceeded to compile it. First or all we need to move to the Shairport Sync folder and then we can proceed to configure the compilation process.

<pre class="lang:sh decode:true" title="Configure compilation of Sharirport Sync">$ cd ~/Downloads/shairport-sync</pre>

<pre class="lang:sh decode:true" title="Configure compilation of Sharirport Sync">$ autoreconf -i -f</pre>

<pre class="lang:sh decode:true" title="Configure compilation of Sharirport Sync">$ ./configure --with-alsa --with-avahi --with-ssl=openssl --with-systemd --with-metadata</pre>

Now that we have configured it, we can proceded to compile and install it.

<pre class="lang:sh decode:true" title="Compile and install">$ make</pre>

<pre class="lang:sh decode:true" title="Compile and install">$ sudo make install</pre>

## Final config

When the config and installing process finish we can proceed to configure the software to use it. The main thing we need to do is create a group of users that can access to the hardware and then create a user on that group that will use the software.

<pre class="lang:sh decode:true" title="Creating a group and adding a user">$ sudo groupadd -r shairport-sync &gt;/dev/null</pre>

<pre class="lang:sh decode:true" title="Creating a group and adding a user">$ sudo useradd -r -M -g shairport-sync -s /usr/bin/nologin -G audio shairport-sync &gt;/dev/null</pre>

We also need to set up shairport sync to start on startup.

<pre class="lang:sh decode:true" title="Set to start on boot">$ sudo systemctl enable shairport-sync</pre>

If we want to start right away to using it we can manually start it with the following command.

<pre class="lang:sh decode:true" title="Starting Shairport Sync">$ sudo service shairport-sync start</pre>

You are going to be able to find the Pi among the devices that offer AirPlay from your iOS device or your Mac. The name of the device is going to be the hostname you set up for your Pi using the [raspi-config interface](https://pimylifeup.com/raspi-config-tool/), if you haven&#8217;t changed it, it&#8217;s going to be `RaspberryPi`.

# Tune up

<div id="attachment_1443" style="width: 1034px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2018/01/SSF_WMD_Modules_-_2014_NAMM_Show.jpg"><img class="size-full wp-image-1443" src="http://luisspuerto.net/wp-content/uploads/2018/01/SSF_WMD_Modules_-_2014_NAMM_Show.jpg" alt="" width="1024" height="682" srcset="http://luisspuerto.net/wp-content/uploads/2018/01/SSF_WMD_Modules_-_2014_NAMM_Show.jpg 1024w, http://luisspuerto.net/wp-content/uploads/2018/01/SSF_WMD_Modules_-_2014_NAMM_Show-300x200.jpg 300w, http://luisspuerto.net/wp-content/uploads/2018/01/SSF_WMD_Modules_-_2014_NAMM_Show-768x512.jpg 768w, http://luisspuerto.net/wp-content/uploads/2018/01/SSF_WMD_Modules_-_2014_NAMM_Show-375x250.jpg 375w" sizes="(max-width: 1024px) 100vw, 1024px" /></a>

  <p class="wp-caption-text">
    Tuning-up audio. Source: <a href="https://commons.wikimedia.org/wiki/File:SSF_WMD_Modules_-_2014_NAMM_Show.jpg">Wikimedia Commons</a>
  </p>
</div>

If you try to use your Pi as a AirPort Express right away you probably going find out that the sound quality leaves a lot to be desired –very low and quite distorted. However, you can do some tune-ups in the settings to improve this, but don&#8217;t thing we are going to be able to make it sound like a real AirPort Express.

## Update Raspberry Pi Firmware

One of the first things we can do to improve the quality of the sound is to update the sound driver of the Pi. To do it, you need to update the firmware of the Pi. You can read more about this in [this forum thread](https://www.raspberrypi.org/forums/viewtopic.php?t=136445). Please, keep in mind that while you are updating the firmware you must not lose power in the Pi. To update the firmware you run the following command:

<pre class="lang:sh decode:true" title="Updating Pi's firmware">$ sudo rpi-update</pre>

Once the update process finish you need to turn off the Raspberry and take the SD card out of it and insert the card in a computer card reader. We are going to modify the boot config file of the Pi and and to do it we are going to open with a text editor the config file that it&#8217;s located on `/boot/config.txt`. Then you need to add the following variable to the end of the file:

<pre class="lang:sh decode:true" title="Booting audio variable">audio_pwm_mode=2</pre>

Save the file, put the card back on the Pi and turn it on.

## Audio jack as main audio

Now, we also need to set up the audio jack as the main audio output. Just run the following command:

<pre class="lang:sh decode:true" title="Jack audio as main output">$ amixer cset numid=3 1</pre>

## Shairport Sync db range

There is a final setting we need to configure and it&#8217;s related to the audio range in which Shairport Sync operates. To do it we need to open the config file of Shariport Sync, therefore run the following command on the Pi&#8217;s terminal to open it with [Nano](https://en.wikipedia.org/wiki/GNU_nano).

<pre class="lang:sh decode:true" title="Open Shairport Sync config file">$ sudo nano /usr/local/etc/shairport-sync.conf</pre>

Then we need to look for the following variable:

<pre class="highlight:0 decode:true ">//      volume_range_db = 60 ;</pre>

and change it to

<pre class="highlight:0 decode:true">volume_range_db = 30;</pre>

Save and close Nano, `Ctrl + X` then `Y` and finally press `return`.

You need to reboot the Pi to make sure that new configuration is properly loaded.

<pre class="lang:sh decode:true " title="Rebooting the Pi">$ sudo reboot</pre>

# Bottom line

As you are going to soon find out the quality of sound isn&#8217;t exactly the same as with an AirPort Express, but this tweak allow you to use your Pi as one. It can be handy if you are traveling, you have your Pi with you, and you want to plug your music into some audio device or speakers.

If you wand to use your Raspberry for to play audio there are other options out there. One is [RuneAudio](http://www.runeaudio.com) which is a dedicated OS to play music on the Pi and the other is to install [Kodi](https://kodi.tv) since [Kodi can play AirPlay](http://kodi.wiki/view/AirPlay).

<div id="attachment_1441" style="width: 730px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2018/01/Enable-Airplay-Kodi-Featured.jpg"><img class="size-full wp-image-1441" src="http://luisspuerto.net/wp-content/uploads/2018/01/Enable-Airplay-Kodi-Featured.jpg" alt="" width="720" height="405" srcset="http://luisspuerto.net/wp-content/uploads/2018/01/Enable-Airplay-Kodi-Featured.jpg 720w, http://luisspuerto.net/wp-content/uploads/2018/01/Enable-Airplay-Kodi-Featured-300x169.jpg 300w, http://luisspuerto.net/wp-content/uploads/2018/01/Enable-Airplay-Kodi-Featured-444x250.jpg 444w" sizes="(max-width: 720px) 100vw, 720px" /></a>

  <p class="wp-caption-text">
    Activating Airplay in Kodi
  </p>
</div>
