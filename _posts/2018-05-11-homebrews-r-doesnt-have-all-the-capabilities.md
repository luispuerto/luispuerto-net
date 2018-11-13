---
title: Homebrew's R doesn't have all the capabilities
date: 2018-05-11 10:35:32
header: 
  overlay_image: /assets/images/blog/2018/R-Homebrew.jpg
  teaser: /assets/images/blog/2018/R-Homebrew.jpg
image: /assets/images/blog/2018/R-Homebrew.jpg
categories:
  - Professional
  - RStats
  - Technology
tags:
  - homebrew
  - RSoft
  - RStats
---
A couple of days ago I just found out that when you install R with Homebrew you don't get all the capabilities that the binary from CRAN have. In other words, you have a kind of _second class install_, in some regards, and depending on how you do install and for what you are going to use R.

```R
 >capabilities()

       jpeg         png        tiff       tcltk         X11        aqua
      FALSE       FALSE       FALSE       FALSE       FALSE        TRUE
   http/ftp     sockets      libxml        fifo      cledit       iconv
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
        NLS     profmem       cairo         ICU long.double     libcurl
       TRUE        TRUE       FALSE        TRUE        TRUE        TRUE
```
```R
>capabilities()
       jpeg         png        tiff       tcltk         X11        aqua
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
   http/ftp     sockets      libxml        fifo      cledit       iconv
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
        NLS     profmem       cairo         ICU long.double     libcurl
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
```

How did I notice? Well basically I was about to update the [sm package](https://cran.r-project.org/web/packages/sm/) in one of my computers and R yield an error similar to this one (sorry I didn't copy the original one):

<!-- this code was wrapped-->
```R 
Error: package or namespace load failed for ‘tcltk’:
 .onLoad failed in loadNamespace() for 'tcltk', details:
  call: fun(libname, pkgname)
  error: Tcl/Tk support is not available on this system
In addition: Warning message:
S3 methods ‘as.character.tclObj’, ‘as.character.tclVar’, ‘as.double.tclObj’, ‘as.integer.tclObj’, ‘as.logical.tclObj’, ‘as.raw.tclObj’, ‘print.tclObj’, ‘[[.tclArray’, ‘[[<-.tclArray’, ‘$.tclArray’, ‘$<-.tclArray’, ‘names.tclArray’, ‘names<-.tclArray’, ‘length.tclArray’, ‘length<-.tclArray’, ‘tclObj.tclVar’, ‘tclObj<-.tclVar’, ‘tclvalue.default’, ‘tclvalue.tclObj’, ‘tclvalue.tclVar’, ‘tclvalue<-.default’, ‘tclvalue<-.tclVar’, ‘close.tkProgressBar’ were declared in NAMESPACE but not found
```

As you can see I've highlighted the key line, R doesn't have [tcltk](https://en.wikipedia.org/wiki/Tcl) available and running in that system and if you run `capabilities()` in the R console you'll probably get something similar to the first code-block. After researching a little bit about the problem [[1](https://discourse.brew.sh/t/r-installs-on-high-sierra-without-tcl-tk-support/1190) & [2](https://discourse.brew.sh/t/r-bottle-options-graphics-capabilities/1785/10)], I found out what I've already said, Homebrew R isn't build with those capabilities. Why? Well…

  1. It seems that those capabilities are optional when you build R from source and build R with those capabilities / options on Homebrew's bottle server is "error prone".
  2. To have some of the missing capabilities you must have in your system [X11/XQuartz](https://www.xquartz.org), which isn't installed in every Mac, because it's not longer provided as macOS basic installation.
  3. You can install X11/XQuartz, but you have to do it with Homebrew Cask, since it can't be build from source. As a consequence is a off project dependency and they don't want to rely on that.
  4. Homebrew seems to be heading to a option-less direction where the formulas aren't going to have any option at all. So if you want to have options or different kind of build they encourage you to have your [own tap](https://docs.brew.sh/How-to-Create-and-Maintain-a-Tap.html).

Whether you agree or not, you have to understand that the maintainers of Homebrew-core took this direction for a reason, which is probably they have already too much work to do. Don't forget they maintain the package manager for free. If you disagree, [as I do](https://discourse.brew.sh/t/r-bottle-options-graphics-capabilities/1785/9?u=luisspuerto), at least partly, please be polite. A lot of users have been expressing their disagreement in really bad manners lately and that is not acceptable.

Lucky for us users, there is a solution to the problem. You can always create your own tap of Homebrew and tweak the formula to fit your needs, as maintainers suggest. As a result [someone already did that](https://discourse.brew.sh/t/r-installs-on-high-sierra-without-tcl-tk-support/1190/16?u=luisspuerto) and there is already a tap which you can install R with the same formula we installed R when it was in Homebrew Science, but up to day.

I'll explain in [my next post](https://wp.me/p8vFcV-tC) how to install R using that tap.