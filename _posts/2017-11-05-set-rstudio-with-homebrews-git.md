---
id: 717
title: Set rStudio with Homebrew's Git
date: 2017-11-05T14:05:53+00:00
guid: http://luisspuerto.net/?p=717
permalink: /2017/11/set-rstudio-with-homebrews-git/
image: /wp-content/uploads/2017/11/rstudiobrewgit-1.jpg
categories:
  - Professional
  - RStats
  - Technology
tags:
  - git
  - homebrew
  - how to
  - RSoft
  - RStudio
---
As I'll try to explain in the future, I have a full [Homebrew](https://brew.sh) R install. I also try to use Homebrew to install as much applications and utilities I can because I think it's really handy to be able to install or update just with a simple command in the shell. Although sometimes it give you a little bit of a headache, as this time.

<div id="attachment_722" style="width: 258px" class="wp-caption alignleft">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-12.08.19.png"><img class="size-full wp-image-722" src="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-12.08.19.png" alt="" width="248" height="93" /></a>

  <p class="wp-caption-text">
    Apple vs Homebrew's Git version
  </p>
</div>

Apple ships with their systems a really to use version of Git, but it's usually a bit outdated and you can't update that version —I don't really know what version they have shipped with High Sierra<sup id="fnref-717-1"><a class="jetpack-footnote" href="#fn-717-1">1</a></sup>. This is usually not a really big deal, since, as you seen in the left picture, the versions aren't really far away and most of the people don't use Git in their everyday lives, but if you really need, or want to be, up to-day you are going to need to do some tweaks here and there.

First of all, you need to install the last Git using Homebrew:

```sh 
  $ brew install git 

```

You are probably going to need to relink by force <span class="lang:sh highlight:0 decode:true crayon-inline ">-f</span>  your new Git to make the system to use it.

```sh 
  $ brew link -f git
```

Know if you ask the system, your version of Git have to be different —superior—  to the one Apple provides. In my case and under El Capitan is like this:

```sh 
  $ usr/bin/git --version 
git version 2.10.1 (Apple Git-78) # Apple's version 
$ /usr/local/Cellar/git/2.15.0/bin/git --version 
git version 2.15.0 # Homebrew's version
$ git --version 
git version 2.15.0 # System's version, now linked to Homebrew's one.
```

If you are user of [rStudio](https://www.rstudio.com), you'll probably know that you can use Git in your rStudio projects, as you should, as in any coding project. rStudio developers, to save us with the fuss of installing and configuring Git —and to save them to explain us how, set up rStudio to use the Apple's Git by default, pointing to <span class="lang:sh highlight:0 decode:true crayon-inline ">/usr/bin/git</span>  in the user settings

<div id="attachment_723" style="width: 672px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM.png"><img class=" wp-image-723" src="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM.png" alt="" width="662" height="668" srcset="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM.png 1152w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM-150x150.png 150w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM-297x300.png 297w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM-768x775.png 768w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM-1015x1024.png 1015w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM-1024x1033.png 1024w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM-248x250.png 248w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2015-11-09-at-4.59.40-PM-50x50.png 50w" sizes="(max-width: 662px) 100vw, 662px" /></a>

  <p class="wp-caption-text">
    rStudio's version control options
  </p>
</div>

If you want to use your own downloaded Git you just have to change that value in the settings and that's it. However, it's a little bit more complicated than that in practice. Since we're using Homebrew to manage our apps the directory of the app changes every time you update it to show the version it's storing, just in case you wanted to have more than one version. The path to the directory of Homebrew's Git looks something like this, as you saw before in a code chuck:

```sh 
/usr/local/Cellar/git/2.15.0/bin/git
```

If you updated Git using Homebrew, the new path would look something like this.

```sh 
  /usr/local/Cellar/git/2.XX.X/bin/git
```

Where the <span class="lang:sh highlight:0 decode:true crayon-inline ">X</span>  are the new version numbers.

To make things easier, Homebrew just create [symbolic links](https://en.wikipedia.org/wiki/Symbolic_link) to the directory <span class="lang:sh highlight:0 decode:true crayon-inline">/usr/local/bin/</span>  to make the system find the app files. That is what you did when you used the command <span class="lang:sh highlight:0 decode:true crayon-inline ">brew link</span>  and you had to use the flag <span class="lang:sh highlight:0 decode:true crayon-inline ">-f</span>  —force— to overwrite the already existent links to Apple's Git in <span class="lang:sh highlight:0 decode:true crayon-inline ">usr/bin/git</span> .

So, you're probably thinking that you just have to set the path to Git executable in rStudio to <span class="lang:sh highlight:0 decode:true crayon-inline ">/usr/local/bin/git</span>  and problem fixed. Yes and not. Yes because that is the path you need to end up in settings, but is something you can't make to happen using rStudio interface. Every time you change the path to <span class="lang:sh highlight:0 decode:true crayon-inline ">/usr/local/bin/git</span>  using the browse button in the settings you are going to end up with <span class="lang:sh highlight:0 decode:true crayon-inline ">/usr/local/Cellar/git/2.XX.X/bin/git</span>  and in consequence when you update Git using Homebrew rStudio isn't going to be able to find the Git executable. In consequence, you will end up without Git support in rStudio. This happens because rStudio interface follows the symbolic link instead of stuck with the route you set with the browse button.

Then… What we should do? You need to open the user setting's file of rStudio located in a hidden directory in your user folder <span class="lang:sh highlight:0 decode:true crayon-inline ">~/.rstudio/monitored/user-settings/user-settings</span>  with the following command.

```sh 
  $ open ~/.rstudio/monitored/user-settings/user-settings
```

<div id="attachment_725" style="width: 1349px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00.png"><img class="size-full wp-image-725" src="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00.png" alt="" width="1339" height="777" srcset="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00.png 1339w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00-300x174.png 300w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00-768x446.png 768w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00-1024x594.png 1024w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00-1216x706.png 1216w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-02-at-17.24.00-431x250.png 431w" sizes="(max-width: 1339px) 100vw, 1339px" /></a>

  <p class="wp-caption-text">
    rStudio user setting's file.
  </p>
</div>

Towards the end of the file, there is a variable called <span class="lang:sh highlight:0 decode:true crayon-inline ">vcsGitExePath</span>  that set your personalized options for the path of Git executable. With your rStudio closed, you can change it in the file to this:

```sh 
vcsGitExePath=“/usr/local/bin/git"

```

and save the file.

If you open again rStudio and go to your user settings, the version control settings have to look like this:

<div id="attachment_727" style="width: 724px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-13.47.17-1.png"><img class=" wp-image-727" src="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-13.47.17-1.png" alt="" width="714" height="704" srcset="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-13.47.17-1.png 587w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-13.47.17-1-300x296.png 300w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-13.47.17-1-253x250.png 253w, http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-05-at-13.47.17-1-50x50.png 50w" sizes="(max-width: 714px) 100vw, 714px" /></a>

  <p class="wp-caption-text">
    Version Control Settings on rStudio.
  </p>
</div>

Now you can update without any problem your Git using Homebrew and it will keep working.

<div id="attachment_752" style="width: 272px" class="wp-caption alignright">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-09-at-18.00.27.png"><img class="size-full wp-image-752" src="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-09-at-18.00.27.png" alt="" width="262" height="72" /></a>

  <p class="wp-caption-text">
    Git's version in High Sierra
  </p>
</div>

<li id="fn-717-1">
  I just updated to High Sierra, and I can confirm that the Git's version is a little bit more up to to day, but it isn't the latest. <a href="#fnref-717-1">&#x21a9;</a></fn></footnotes>
