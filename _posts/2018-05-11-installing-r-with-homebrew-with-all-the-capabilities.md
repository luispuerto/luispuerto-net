---
title: Installing R with Homebrew with all the capabilities
date: 2018-05-11 11:57:06
header:
  overlay_image: assets/images/blog/2018/R-Homebrew.jpg
  teaser: assets/images/blog/2018/R-Homebrew.jpg
categories:
  - Professional
  - RStats
  - Technology
tags:
  - homebrew
  - how to
  - RSoft
  - RStats
---
As I explained in my [previous post](http://luisspuerto.net/2018/05/homebrews-r-doesnt-have-all-the-capabilities/) if you installed R with Homebrew you have less capabilities than with a R installed from CRAN's binary. But, **can you have all the capabilities while you still use Homebrew to install R? Yes!** However… why you bother to install it with Homebrew at all instead of installing it from CRAN. Well, it's true that CRAN install it's easier. You just have to download the binary and that's it. You can even use Homebrew Cask to install R that way

```sh
$ brew cask install r-app
```

However, if you install that way you can't take advantage of [OpenBlas and OpenMP](http://luisspuerto.net/2018/01/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#openblas-openmp) which really enhance the speed of R processing. Well, you can take advantage of OpenMP with CRAN install if you use the [coatless professor method](http://luisspuerto.net/2018/01/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#openblas-openmp).

## Getting all the capabilities

To make this possible we are going to install/reinstall R, and Cairo, with the formulae [Seth Fore](https://github.com/sethrfore) has in his tap, [sethrfore](https://github.com/sethrfore)/[homebrew-r-srf](https://github.com/sethrfore/homebrew-r-srf). These formulae are basically the same on [brewsci](https://github.com/brewsci)/[homebrew-science](https://github.com/brewsci/homebrew-science), which is a legacy tap (no longer updated), which we all were using to install R with Homebrew before they merge everything to Core. Seth updated both formulae for us, so we can enjoy the last version of R and Cairo while not loosing any capability.

You also have to remember that this instructions are just aimed to reinstall R if you have follow my previous instructions to have a [100% R install with Homebrew](http://luisspuerto.net/2018/01/install-r-100-homebrew-edition-with-openblas-openmp-my-version/). And they would replace this [section](http://luisspuerto.net/2018/01/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#r).

The first thing you have to do is uninstall R and Cairo if you have them installed:

```sh
$ brew uninstall R
$ brew uninstall --ignore-dependencies cairo
```

If you have [Cairo](https://cairographics.org) installed, it's going to protest about dependencies, but don't worry and we are just going to reinstall it in a minute.

When you don't have R and Cairo in your system you can go ahead.

You begin tapping Seth's tap.

```sh
$ brew tap sethrfore/r-srf
```

Actually, you can avoid this step since Seth's formulae have the same name as Homebrew Core ones we are forced to install them using the full name of the tap in combination with the formula name.

If you don't have [Xquartz](https://www.xquartz.org) already installed in your system you can install with:

```sh
$ brew cask install xquartz
```

Now, you can install Cairo.

```s
$ brew install sethrfore/r-srf/cairo
```

When Cairo finish to build you can proceed with R.

```sh
$ brew install sethrfore/r-srf/r --with-openblas --with-java --with-cairo --with-libtiff --with-pango
```

To use the Pango flag `--with-pango` you must have installed in your system [Pango](http://www.pango.org) `brew install pango`.

When it end to build, you can use use `capabilities()` in R console and you have to get something like this:

```r
>capabilities()
       jpeg         png        tiff       tcltk         X11        aqua
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
   http/ftp     sockets      libxml        fifo      cledit       iconv
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
        NLS     profmem       cairo         ICU long.double     libcurl
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
```

To finish I would run:

```sh
$ R CMD javareconf
```

To reconfigure Java on R, just in case.
