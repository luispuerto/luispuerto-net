---
id: 534
title: Trash location in macOS vs Linux
date: 2017-10-26T19:37:42+00:00
author: Luis Puerto
layout: 
guid: http://luisspuerto.net/?p=534
permalink: /2017/10/trash-location-in-macos-vs-linux/
categories:
  - Professional
  - Technology
tags:
  - linux
  - macOS
  - raspberry pi
  - raspbian
  - shell
---
<div id="attachment_535" style="width: 99px" class="wp-caption alignleft">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-17.59.52.png"><img class="size-full wp-image-535" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-17.59.52.png" alt="" width="89" height="143" /></a>
  
  <p class="wp-caption-text">
    White trash (oh wait!) on macOS
  </p>
</div>

[The other day](http://luisspuerto.net/2017/10/trash-instead-of-rm/), I explained that there is a way to send files and folders to the trash from the the shell. Now, I just found out that there are differences between where is located the trash (it&#8217;s a folder nonetheless) in macOS and Linux.

In macOS, the trash is located in your user&#8217;s folder <span class="lang:sh highlight:0 decode:true crayon-inline ">~/.Trash</span>  and seems that it&#8217;s the only trash accessible by your user, even if you are deleting stuff with <span class="lang:sh highlight:0 decode:true crayon-inline ">rmtrash</span>  and <span class="lang:sh highlight:0 decode:true crayon-inline ">sudo</span> . In other words, everything you send with the <span class="lang:sh highlight:0 decode:true crayon-inline ">rmtrash</span> command is moved to <span class="lang:sh highlight:0 decode:true  crayon-inline ">~/.Trash</span> . However, if you use a <span class="lang:sh highlight:0 decode:true crayon-inline">trash-cli</span> trash-cli command (<span class="lang:sh highlight:0 decode:true  crayon-inline ">trash-put</span>  for example, or just <span class="lang:sh highlight:0 decode:true  crayon-inline ">trash</span> ) you are moving the files to somewhere else that I haven&#8217;t found out yet. Therefore, you have to empty the trash-can with <span class="lang:sh highlight:0 decode:true  crayon-inline ">trash-empty</span>  or restore with <span class="lang:sh highlight:0 decode:true  crayon-inline ">trash-restore</span> . Probably, the answer of to this question (where is the trash-cli&#8217;s can on macOS?) is (or it isn&#8217;t) in the [FreeDesktop.org Trash specifications](http://www.ramendik.ru/docs/trashspec.html). However, I think that it&#8217;s much better to use <span class="lang:sh highlight:0 decode:true  crayon-inline ">rmtrash</span>  on macOS, since it&#8217;s a [much nicer](https://github.com/PhrozenByte/rmtrash) and neat command. And easily to understand.

On another hand, in Linux, you can only use trash-cli (you can&#8217;t get <span class="lang:sh highlight:0 decode:true  crayon-inline ">rmtrash</span>  AFAIK), and I recommend you to install from source, as explained in it&#8217;s [GitHub](https://github.com/andreafrancia/trash-cli) site, because if you install using <span class="lang:sh highlight:0 decode:true  crayon-inline ">apt-get</span>  you are going to get a some kind of an old version without all the commands (<span class="lang:sh highlight:0 decode:true  crayon-inline ">trash-restore</span>  was missing).

In Linux, you have two trashes when you are operating with <span class="lang:sh highlight:0 decode:true  crayon-inline ">trash-cli</span>  on shell. One is your user&#8217;s trash and you can access to it on [this](https://askubuntu.com/questions/102099/where-is-the-trash-folder) path <span class="lang:sh highlight:0 decode:true  crayon-inline ">/home/$USER/.local/share/Trash</span> . There, you can see two folders files and info. I guest that you already know where are the files and where are the info / metadata of the files. However, if you use the command <span class="lang:sh highlight:0 decode:true  crayon-inline ">sudo trash-put</span>  you files are going to be moved to <span class="lang:sh highlight:0 decode:true  crayon-inline ">/root/.local/share/Trash</span> (obviously, or not that obvious as we&#8217;ve seen in macOS).

For the other <span class="lang:sh highlight:0 decode:true  crayon-inline ">trash-cli</span>  commands the fashion is the same and if you wanted to see the folders you trashed with root or restore a file from there, you need to use <span class="lang:sh highlight:0 decode:true  crayon-inline ">sudo</span>  before the command.

Now, you are ready to trash whatever you want.

<div id='gallery-1' class='gallery galleryid-534 gallery-columns-3 gallery-size-thumbnail'>
  <dl class='gallery-item'>
    <dt class='gallery-icon landscape'>
      <a href='http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.40.02.png'><img width="150" height="150" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.40.02-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-1-536" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.40.02-150x150.png 150w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.40.02-480x480.png 480w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.40.02-50x50.png 50w" sizes="(max-width: 150px) 100vw, 150px" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-1-536'>
      Trash-cli on macOS
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon landscape'>
      <a href='http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-19.10.53.png'><img width="150" height="150" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-19.10.53-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-1-540" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-19.10.53-150x150.png 150w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-19.10.53-480x480.png 480w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-19.10.53-50x50.png 50w" sizes="(max-width: 150px) 100vw, 150px" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-1-540'>
      Trash-cli on Linux
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon landscape'>
      <a href='http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.33.15.png'><img width="150" height="150" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.33.15-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-1-542" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.33.15-150x150.png 150w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.33.15-480x480.png 480w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-26-at-18.33.15-50x50.png 50w" sizes="(max-width: 150px) 100vw, 150px" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-1-542'>
      Trash on Raspbian
    </dd>
  </dl>
  
  <br style="clear: both" />
</div>