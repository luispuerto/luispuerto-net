---
id: 483
title: trash instead of rm
date: 2017-10-19T17:33:24+00:00
author: Luis Puerto
layout: 
guid: http://luisspuerto.net/?p=483
permalink: /2017/10/trash-instead-of-rm/
image: /wp-content/uploads/2017/10/mac-1.jpg
categories:
  - Professional
  - Technology
tags:
  - how to
  - linux
  - macOS
  - shell
---
I just discovered that there is a way to send files to the trash instead of just delete them using the shell and I think it&#8217;s really useful when you don&#8217;t want to totally discard something to the oblivion. I found out about this because I was working on the shell and I needed to remove some files to check if app could work without them. Move or rename them was an option, but I thought that perhaps there were a more efficient way to that and thought that send then the trash was the most efficient way, since from there they could be deleted forever or restored. think

I thought that perhaps the command <span class="lang:sh highlight:0 decode:true crayon-inline ">rm</span>   could have some flag or option that send the files or directories you wish to remove to the trash instead of delete immediately them. However, after read a little bit about the topic, I realized that use rm command is a bad idea and practice to send files to the trash.

You can read a little bit more about it [here](https://unix.stackexchange.com/questions/42757/make-rm-move-to-trash) and [here](https://apple.stackexchange.com/questions/50844/how-to-move-files-to-trash-from-command-line), but to summarize a little bit, it&#8217;s basically not safe. You can get use to use <span class="lang:sh decode:true crayon-inline ">rm</span>   to send thing to the trash and if for a moment you are on other computer or with other username you can delete things permanently.

For that reason you can install other commands like [trash-cli](https://github.com/andreafrancia/trash-cli), and in macOS you can also install [rmtrash](https://github.com/PhrozenByte/rmtrash). On macOS you can install both things using [homebrew](https://brew.sh) and in Linux with <span class="lang:sh highlight:0 decode:true crayon-inline ">apt-get</span>  .

<pre class="lang:sh decode:true" title="trash-cli and rmtrash install"># On Mac you can install with Brew
$ brew install trash-cli
$ brew install rmtrash

# On linux you can install with apt-get
$ sudo apt-get install trash-cli</pre>

On the previous links about the commands you have all the options you can implement in both commands.

Happy _trashing_.

PS/ You have to be aware that the shell trash is not the same you can see in your desktop or at least I could find them there. Perhaps, further config is needed.

<div id="attachment_519" style="width: 972px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-11.10.12.png"><img class="size-full wp-image-519" src="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-11.10.12.png" alt="" width="962" height="757" srcset="http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-11.10.12.png 962w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-11.10.12-300x236.png 300w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-11.10.12-768x604.png 768w, http://luisspuerto.net/wp-content/uploads/2017/10/Screen-Shot-2017-10-17-at-11.10.12-318x250.png 318w" sizes="(max-width: 962px) 100vw, 962px" /></a>
  
  <p class="wp-caption-text">
    Trashing files.
  </p>
</div>