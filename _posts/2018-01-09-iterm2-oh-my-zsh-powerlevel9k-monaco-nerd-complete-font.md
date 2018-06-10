---
id: 1287
title: iTerm2 + Oh My Zsh + Powerlevel9k + Monaco Nerd Complete Font
date: 2018-01-09T19:15:18+00:00
author: Luis Puerto
layout: 
guid: http://luisspuerto.net/?p=1287
permalink: /2018/01/iterm2-oh-my-zsh-powerlevel9k-monaco-nerd-complete-font/
image: /wp-content/uploads/2017/12/Screen-Shot-2017-12-19-at-11.20.07.png
categories:
  - Professional
  - Technology
tags:
  - how to
  - macOS
  - shell
---
In general, I don&#8217;t use my Mac&#8217;s [Terminal app](https://en.wikipedia.org/wiki/Terminal_(macOS)). Instead, I use [iTem2](https://iterm2.com) with a special configuration, that doesn&#8217;t use [Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)), but [Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh) as a shell, that is a framework to manage [Zsh](https://en.wikipedia.org/wiki/Z_shell) configuration as your shell. This framework allows you to install plugins or configure your prompt, among other cool things. I&#8217;ve also configured iTerm2 to work with a patched Monaco<sup id="fnref-1287-1"><a href="#fn-1287-1" class="jetpack-footnote">1</a></sup> font with the complete collection of [nerd glyphs](https://github.com/ryanoasis/nerd-fonts). The result is more of less what you can see in the featured image in this post, a beautiful and elegant shell that you can configure and enjoy use.

How you can get something similar? Reach this configuration is quite easy. These are the directions:

# Install iTerm2

I would begin installing iTerm2. iTerm2 is just an app similar to Terminal, but with steroids. It has far more options and even have mouse support.

To install iTerm we are going to use [Homebrew](http://luisspuerto.net/2017/11/homebrew/):

<pre class="lang:sh decode:true">$ brew cask install iterm2
</pre>

Now that you have iTerm2 you have to install Oh My Zsh.

# Install Oh My Zsh

To install Oh My Zsh you need to have installed in your system [Git](http://luisspuerto.net/2017/11/set-rstudio-with-homebrews-git/). Usually that is not a problem because Mac comes with its own Git, but remember that [you can update to the last version easily using Homebrew](http://luisspuerto.net/2017/11/set-rstudio-with-homebrews-git/).

However, you can&#8217;t install Oh My Zsh itself using Homebrew, but you can use [cURL](https://en.wikipedia.org/wiki/CURL) or [Wget](https://en.wikipedia.org/wiki/Wget), which probably you have already installed in your system. If you don&#8217;t have any of those, you can install them through Homebrew. To install Oh My Zsh you can run the following commands:

<pre class="lang:sh decode:true" title="Installing Oh My Zsh with curl">$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"</pre>

or

<pre class="lang:sh decode:true" title="Installing Oh My Zsh with wget">$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
</pre>

Oh My Zsh has a autoupdate feature, so don&#8217;t worry about update. From time to time, it&#8217;s going to ask you to check the repo where it&#8217;s stored for updates.

# Installing Powerlevel9K theme

[Powerlevel9K](https://github.com/bhilburn/powerlevel9k) is a Oh My Zsh external theme that gives it that awesome look and the capacity to configure the prompt, yet keep it light. There are literally dozens of themes, whether [included in the Oh My Zsh repo](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes) or [external](https://github.com/robbyrussell/oh-my-zsh/wiki/External-themes) ones, and Powerlevel9K is one of the external ones, so you have to download (clone the repo) and store it on the custom part for the Oh My Zsh configuration folders. To do so, just run the following command in your terminal.

<pre class="lang:sh decode:true" title="Installing Powerlevel9K theme">$ git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k</pre>

&nbsp;

# Configuring Oh My Zsh & PowerLevel9K

When you have Oh My Zsh installed you can begin to configure. In order to do that you have to open the configuration file, which is located in your user folder, with your favorite text editor. In my case I like to use Atom, so I run the following command in the terminal.

<pre class="lang:sh decode:true">$ atom ~/.zshrc</pre>

Bellow you can see my configuration file. The important lines are highlighted.

<pre class="nums:true lang:sh mark:28-73,118-122,151 highlight:0 decode:true " title="My Zsh configuration" data-url="https://gist.githubusercontent.com/luisspuerto/e61e7dd0b6584d02430b71f5dec4185c/raw/8b94e42ca601e112d4708e7f84a8a7b43ba70b04/.zshrc"></pre>

As you can see I have a lot the lines commented with `#` , since I don&#8217;t want to use that config, but I didn&#8217;t lose them. From line 28 to 73, it&#8217;s basically the configuration of the prompt. There are literally dozens of ways to configure the prompt, and you can see some of them [here](https://github.com/bhilburn/powerlevel9k/wiki/Show-Off-Your-Config). Mine is quite similar to [Falkor&#8217;s one](https://github.com/bhilburn/powerlevel9k/wiki/Show-Off-Your-Config#falkors-configuration), but I&#8217;ve edited it a little bit. You can find out more about how to stylizing your prompt and how the configuration variables work [here](https://github.com/bhilburn/powerlevel9k#prompt-customization) and [here](https://github.com/bhilburn/powerlevel9k/wiki/Stylizing-Your-Prompt). 

Don&#8217;t forget to set your theme as Powerlevel9k –line 33 `ZSH_THEME="powerlevel9k/powerlevel9k"`– and also the Powerlevel mode –line 35. The Powerlevel Mode define the type –or the style– of glyphs than are shown. 

You can see also that in the lines 118 &#8211; 122 are the plugins I&#8217;m using and that in the the line 151 I establish a shortcut to access to the configuration through atom just typing `zshconfig`.

# Configuring iTerm2

Finally, you have to configure iTerm2 to use your patched font if you want the glyphs to shown in your prompt

<div id="attachment_1354" style="width: 1040px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.05.13.png"><img class="size-full wp-image-1354" src="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.05.13.png" alt="" width="1030" height="569" srcset="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.05.13.png 1030w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.05.13-300x166.png 300w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.05.13-768x424.png 768w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.05.13-1024x566.png 1024w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.05.13-453x250.png 453w" sizes="(max-width: 1030px) 100vw, 1030px" /></a>
  
  <p class="wp-caption-text">
    iTerm2 font configuration.
  </p>
</div>

If you don&#8217;t want to patch any font, you can download any of the prepatched fonts, and I recommend do it [using Hombrew](https://github.com/ryanoasis/nerd-fonts#option-4-homebrew-fonts). 

<pre class="lang:sh decode:true" title="Installing fonts with Homebrew">$ brew tap caskroom/fonts
$ brew cask install font-meslo-nerd-font #if you want to install Meslo font</pre>

<div id="attachment_1355" style="width: 1012px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.11.31.png"><img class="size-full wp-image-1355" src="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.11.31.png" alt="" width="1002" height="521" srcset="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.11.31.png 1002w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.11.31-300x156.png 300w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.11.31-768x399.png 768w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-02-at-13.11.31-481x250.png 481w" sizes="(max-width: 1002px) 100vw, 1002px" /></a>
  
  <p class="wp-caption-text">
    Color configuration in iTerm2
  </p>
</div>

Finally you can configure the colors in iTerm2. Usually people use of of the presets iTerm have, or the ones you can download. But I have tweaked a little bit the colors and I have my [own configuration](http://luisspuerto.net/wp-content/uploads/2018/01/Luis-Puerto-iTerm.itermcolors.zip).

Now you are ready to use iTerm2 with your new configuration.

# Setting Zsh as your default shell

First you need to check what version, if any, of Zsh you have installed.

<pre class="lang:sh decode:true">$ zsh --version</pre>

in my case and right now my version is 5.4.2, but you can check which is the last version in the [wikipedia page](https://en.wikipedia.org/wiki/Z_shell).

Now you have to check that you have Zsh in your list of authorized shells. You have check running opening the file `/etc/shells` with atom:

<pre class="lang:sh decode:true">$ atom /etc/shells</pre>

You have to see something similar to this.

<pre class="lang:sh mark:10 highlight:0 decode:true" title="/etc/shells"># List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
/usr/local/bin/bash</pre>

If you don&#8217;t have the line 10, add it and save the file.

Now you run the following command to make Zsh your default shell:

<pre class="lang:sh decode:true " title="Zsh as default shell">$ chsh -s $(which zsh)</pre>

# Keeping Terminal with the previous config

Since I have iTerm2 with Oh My Zsh, I like to keep Terminal with Bash. Since we&#8217;ve set as a default terminal Zsh we need to set up manually to use Bash.

Open Terminal and launch the options screen `Cmd + ;`. In the General tap you can find which shell use terminal. Choose command and type `/bin/bash`.

<div id="attachment_1360" style="width: 1305px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54.png"><img class="size-full wp-image-1360" src="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54.png" alt="" width="1295" height="648" srcset="http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54.png 1295w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54-300x150.png 300w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54-768x384.png 768w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54-1024x512.png 1024w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54-1216x608.png 1216w, http://luisspuerto.net/wp-content/uploads/2018/01/Screen-Shot-2018-01-09-at-18.11.54-500x250.png 500w" sizes="(max-width: 1295px) 100vw, 1295px" /></a>
  
  <p class="wp-caption-text">
    Terminal with Bash
  </p>
</div>

Done, now you can enjoy the best of the two worlds. Enjoying a new, more flexible and customizable shell as default, while keeping your old one, just in case you felt nostalgic.

&nbsp;

&nbsp;

<li id="fn-1287-1">
  I was about to upload my patched Monaco font, but then I realize that I can&#8217;t post any modification of the Monaco font since it&#8217;s copyrighted by Apple. However, you can easily patch your copy of the font for your personal use with the <a href="https://github.com/ryanoasis/nerd-fonts#font-patcher">script provided by Nerd Fonts</a>. I faced some problems when I tried to patch it myself, basically related to the height of the patched font, which ended up different than the original font. If this is your case, you can just download <a href="https://en.wikipedia.org/wiki/FontForge">FontForge</a> –<code>brew cask install fontforge</code>– and modify those parameters to be equal to the original ones.&#160;<a href="#fnref-1287-1">&#8617;</a> </fn></footnotes>