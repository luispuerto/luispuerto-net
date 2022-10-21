---
title: Set rStudio with Homebrew's Git
date: 2017-11-05 14:05:53
header:
  overlay_image: assets/images/blog/2018/rstudiobrewgit-1.jpg
  teaser: assets/images/blog/2018/rstudiobrewgit-1.jpg
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

 {% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-11-05-at-12.08.19.png" alt="Apple vs Homebrew's Git version" caption="Apple vs Homebrew's Git version" %}{: .align-left}

Apple ships with their systems a really to use version of Git, but it's usually a bit outdated and you can't update that version —I don't really know what version they have shipped with High Sierra[^1]. This is usually not a really big deal, since, as you seen in the left picture, the versions aren't really far away and most of the people don't use Git in their everyday lives, but if you really need, or want to be, up to-day you are going to need to do some tweaks here and there.

First of all, you need to install the last Git using Homebrew:

```shell
brew install git
```

You are probably going to need to relink by force `-f` your new Git to make the system to use it.

```shell
brew link -f git
```

Know if you ask the system, your version of Git have to be different —superior—  to the one Apple provides. In my case and under El Capitan is like this:

```shell
usr/bin/git --version
git version 2.10.1 (Apple Git-78) # Apple's version
/usr/local/Cellar/git/2.15.0/bin/git --version
git version 2.15.0 # Homebrew's version
git --version
git version 2.15.0 # System's version, now linked to Homebrew's one.
```

If you are user of [rStudio](https://www.rstudio.com), you'll probably know that you can use Git in your rStudio projects, as you should, as in any coding project. rStudio developers, to save us with the fuss of installing and configuring Git —and to save them to explain us how, set up rStudio to use the Apple's Git by default, pointing to `/usr/bin/git`  in the user settings

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2015-11-09-at-4.59.40-PM.png" alt="rStudio's version control options" caption="rStudio's version control options" %}

If you want to use your own downloaded Git you just have to change that value in the settings and that's it. However, it's a little bit more complicated than that in practice. Since we're using Homebrew to manage our apps the directory of the app changes every time you update it to show the version it's storing, just in case you wanted to have more than one version. The path to the directory of Homebrew's Git looks something like this, as you saw before in a code chuck:

```shell
/usr/local/Cellar/git/2.15.0/bin/git
```

If you updated Git using Homebrew, the new path would look something like this.

```shell
/usr/local/Cellar/git/2.XX.X/bin/git
```

Where the `X` are the new version numbers.

To make things easier, Homebrew just create [symbolic links](https://en.wikipedia.org/wiki/Symbolic_link) to the directory `/usr/local/bin/`  to make the system find the app files. That is what you did when you used the command `brew link`  and you had to use the flag `-f`  —force— to overwrite the already existent links to Apple's Git in `usr/bin/git` .

So, you're probably thinking that you just have to set the path to Git executable in rStudio to `/usr/local/bin/git`  and problem fixed. Yes and not. Yes because that is the path you need to end up in settings, but is something you can't make to happen using rStudio interface. Every time you change the path to `/usr/local/bin/git`  using the browse button in the settings you are going to end up with `/usr/local/Cellar/git/2.XX.X/bin/git`  and in consequence when you update Git using Homebrew rStudio isn't going to be able to find the Git executable. In consequence, you will end up without Git support in rStudio. This happens because rStudio interface follows the symbolic link instead of stuck with the route you set with the browse button.

Then… What we should do? You need to open the user setting's file of rStudio located in a hidden directory in your user folder `~/.rstudio/monitored/user-settings/user-settings`  with the following command.

```shell
open ~/.rstudio/monitored/user-settings/user-settings
```

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-11-02-at-17.24.00.png" alt="rStudio user setting's file." caption="rStudio user setting's file." %}

Towards the end of the file, there is a variable called `vcsGitExePath`  that set your personalized options for the path of Git executable. With your rStudio closed, you can change it in the file to this:

```shell
vcsGitExePath=“/usr/local/bin/git"
```

and save the file.

If you open again rStudio and go to your user settings, the version control settings have to look like this:

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-11-05-at-13.47.17-1.png" alt="Version Control Settings on rStudio." caption="Version Control Settings on rStudio." %}{: .align-center}

Now you can update without any problem your Git using Homebrew and it will keep working.

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-11-09-at-18.00.27.png" alt="Git's version in High Sierra" caption="Git's version in High Sierra" %}{: .align-center}


[^1]: I just updated to High Sierra, and I can confirm that the Git's version is a little bit more up to to day, but it isn't the latest.
