---
title: Updating Homebrew, R and Python pip packages + Ruby Gems + macOS
date: 2018-03-22 13:16:47
header:
  overlay_image: assets/images/blog/2018/Homebrew-R-Python-Ruby-macOS.png
  teaser: assets/images/blog/2018/Homebrew-R-Python-Ruby-macOS.png
categories:
  - RStats
  - Technology
tags:
  - app-store
  - homebrew
  - packages
  - python
  - RStats
  - rubygems
---
**Update Monday 2nd of April 2018**: I decided to add to this post how to update the RubyGems since they are a key feature of [Jekyll](/blog/2018/03/04/jekyll/).

**Update Thursday, 19th of April 2018 11.00 UTC+3**: I added info about how to update on the CLI the App Store Apps and macOS software updates.

* * *

In the [post about Homebrew](/blog/2017/11/21/homebrew/) I think I've already posted how I update Homebrew packages with a single comment in the terminal. However these days I update more software via terminal, mainly packages, because it's really handy run just a couple of commands and then see how the software it's updated automatically and easily.

## Homebrew packages

To update Homebrew packages I run:

```shell
brew update && brew upgrade && brew cleanup && brew prune && brew cu -ay && brew cask cleanup
```

  * `brew update` updates Homebrew itself and download the last version of the formulae
  * `brew upgrade` updates the packages you have installed that have new formulae.
  * `brew cleanup` cleans the cache of old versions of packages.
  * `brew prune` clean the old symbolic links form \`/usr/bin/\`.
  * `brew cu -ay` uses [buo](https://github.com/buo)/[homebrew-cask-upgrade](https://github.com/buo/homebrew-cask-upgrade) to update casks. `-ay` flag is all and yes update all outdated apps.
  * `brew cask cleanup` clean the old caches of the updated apps.

If for some reason you don't want to update an specific package in Homebrew, you can _pin _in to an specific version or the current version.

```shell
brew pin <formulae>
```

## R packages

To update R packages I run:

```shell
# to see what are the old packages
Rscript --vanilla -e "old.packages(repos = 'cloud.r-project.org')"
# to directly update
Rscript --vanilla -e "update.packages(ask = F, repos = 'cloud.r-project.org')"
```

Update R packages on terminal is done using the command `Rscript` that allows us to send commands to R using the shell. I use the flag `--vanilla` that combine `--no-save`, `--no-restore`, `--no-site-file``--no-init-file` and `--no-environ`. In other words a way to load R faster and with a standard configuration.

I like to run `old.packages` first because I like to see a list first of the packages I'm going to update and then update. I do this because some packages —data.table— need a different `makevars` than the rest of the packages so just in case I needed to [change](/blog/2018/01/12/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#data-table-package) the `makevars` and rebuild that package.

I really think it would be cool to be able also _pin _packages in R, but I haven't found any way to do so at system wide level. You can do easily at project level with the package [Packrat](https://rstudio.github.io/packrat/). However, I really think it would be really nice to have email notifications when new versions of packages hit CRAN repository and some function to pin packages to the current version.

## Python pip

To update all the packages from pip and pip itself I run:

```shell
pip install --upgrade pip && pip freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U
pip3 install --upgrade pip && pip3 freeze --local | grep -v '^-e' | cut -d = -f 1  | xargs -n1 pip3 install -U
```

I took the idea from [here](https://stackoverflow.com/questions/2720014/upgrading-all-packages-with-pip).

pip is the package manager for packages written in Python. You can read a little bit more on the [Wikipedia](https://en.wikipedia.org/wiki/Pip_(package_manager)).

## RubyGems

[RubyGems](https://en.wikipedia.org/wiki/RubyGems) is a package manager for Gems that are packages written in Ruby language. [Homebrew](/blog/2017/11/21/homebrew/) is written in Ruby and [Jekyll](/blog/2018/03/04/jekyll/) too, the latter make use of several RubyGems for functionalities and extensions.

To update the RubyGems you run the following command:

```shell
gem update
```

## App Store and macOS software updates

Although these ones are really easy and you can update them just using the Mac App Store app in your Mac, it's possible to trigger the update checking and the update itself through CLI. macOS has a command that allow you to make this happen `softwareupdate`. You can run it like this:

```shell
softwareupdate -l ## to list all the updates
softwareupdate -i ## to install updates
softwareupdate -ia ## to install all updates
softwareupdate -iR ## to automatically restart if necessary by the update
softwareupdate -ir ## install only the recommended updates
softwareupdate -d ## only download the updates
```

As you see it's quite thorough and you can see more options with running `man softwareupdate`.

If you want something more complex you can install [mas-cli](https://github.com/mas-cli/mas), which is a Mac App Store command line interface. Install you just run:

```shell
brew install mas
```

Then, you can run in your command line the following to update:

```shell
mas list ## List your Mac App Store apps
mas search <app> ## Search for an app
mas install <app-number> ## Install an specific app
mas outdated ## shows the outdated apps
mas upgrade ## Upgrade all your apps
mas upgrade <app-number> ## Upgrade an specific app
```
