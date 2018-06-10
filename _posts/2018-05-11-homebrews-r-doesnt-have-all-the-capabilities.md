---
id: 1830
title: 'Homebrew&#8217;s R doesn&#8217;t have all the capabilities'
date: 2018-05-11T10:35:32+00:00
author: Luis Puerto
layout: 
guid: http://luisspuerto.net/?p=1830
permalink: /2018/05/homebrews-r-doesnt-have-all-the-capabilities/
wtr-disable-reading-progress:
  - ""
wtr-disable-time-commitment:
  - ""
image: /wp-content/uploads/2018/01/R-Homebrew.jpg
categories:
  - Professional
  - RStats
  - Technology
tags:
  - homebrew
  - RSoft
  - RStats
---
A couple of days ago I just found out that when you install R with Homebrew you don&#8217;t get all the capabilities that the binary from CRAN have. In other words, you have a kind of _second class install_, in some regards, and depending on how you do install and for what you are going to use R.

<pre class="lang:r decode:true" title="Capabilities on R Homebrew:">&gt; capabilities()
       jpeg         png        tiff       tcltk         X11        aqua
      FALSE       FALSE       FALSE       FALSE       FALSE        TRUE
   http/ftp     sockets      libxml        fifo      cledit       iconv
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
        NLS     profmem       cairo         ICU long.double     libcurl
       TRUE        TRUE       FALSE        TRUE        TRUE        TRUE</pre>

<pre class="lang:r decode:true" title="Capabilities on R CRAN">&gt; capabilities()
       jpeg         png        tiff       tcltk         X11        aqua    
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
   http/ftp     sockets      libxml        fifo      cledit       iconv
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
        NLS     profmem       cairo         ICU long.double     libcurl
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE</pre>

How did I notice? Well basically I was about to update the [sm package](https://cran.r-project.org/web/packages/sm/) in one of my computers and R yield an error similar to this one (sorry I didn&#8217;t copy the original one):

<pre class="wrap:true lang:r mark:4 decode:true" title="tcltk error">Error: package or namespace load failed for ‘tcltk’:
 .onLoad failed in loadNamespace() for 'tcltk', details:
  call: fun(libname, pkgname)
  error: Tcl/Tk support is not available on this system
In addition: Warning message:
S3 methods ‘as.character.tclObj’, ‘as.character.tclVar’, ‘as.double.tclObj’, ‘as.integer.tclObj’, ‘as.logical.tclObj’, ‘as.raw.tclObj’, ‘print.tclObj’, ‘[[.tclArray’, ‘[[&lt;-.tclArray’, ‘$.tclArray’, ‘$&lt;-.tclArray’, ‘names.tclArray’, ‘names&lt;-.tclArray’, ‘length.tclArray’, ‘length&lt;-.tclArray’, ‘tclObj.tclVar’, ‘tclObj&lt;-.tclVar’, ‘tclvalue.default’, ‘tclvalue.tclObj’, ‘tclvalue.tclVar’, ‘tclvalue&lt;-.default’, ‘tclvalue&lt;-.tclVar’, ‘close.tkProgressBar’ were declared in NAMESPACE but not found</pre>

As you can see I&#8217;ve highlighted the key line, R doesn&#8217;t have [tcltk](https://en.wikipedia.org/wiki/Tcl) available and running in that system and if you run `capabilities()` in the R console you&#8217;ll probably get something similar to the first code-block. After researching a little bit about the problem [[1](https://discourse.brew.sh/t/r-installs-on-high-sierra-without-tcl-tk-support/1190) & [2](https://discourse.brew.sh/t/r-bottle-options-graphics-capabilities/1785/10)], I found out what I&#8217;ve already said, Homebrew R isn&#8217;t build with those capabilities. Why? Well&#8230;

  1. It seems that those capabilities are optional when you build R from source and build R with those capabilities / options on Homebrew&#8217;s bottle server is &#8220;error prone&#8221;.
  2. To have some of the missing capabilities you must have in your system [X11/XQuartz](https://www.xquartz.org), which isn&#8217;t installed in every Mac, because it&#8217;s not longer provided as macOS basic installation.
  3. You can install X11/XQuartz, but you have to do it with Homebrew Cask, since it can&#8217;t be build from source. As a consequence is a off project dependency and they don&#8217;t want to rely on that.
  4. Homebrew seems to be heading to a option-less direction where the formulas aren&#8217;t going to have any option at all. So if you want to have options or different kind of build they encourage you to have your [own tap](https://docs.brew.sh/How-to-Create-and-Maintain-a-Tap.html).

Whether you agree or not, you have to understand that the maintainers of Homebrew-core took this direction for a reason, which is probably they have already too much work to do. Don&#8217;t forget they maintain the package manager for free. If you disagree, [as I do](https://discourse.brew.sh/t/r-bottle-options-graphics-capabilities/1785/9?u=luisspuerto), at least partly, please be polite. A lot of users have been expressing their disagreement in really bad manners lately and that is not acceptable.

Lucky for us users, there is a solution to the problem. You can always create your own tap of Homebrew and tweak the formula to fit your needs, as maintainers suggest. As a result [someone already did that](https://discourse.brew.sh/t/r-installs-on-high-sierra-without-tcl-tk-support/1190/16?u=luisspuerto) and there is already a tap which you can install R with the same formula we installed R when it was in Homebrew Science, but up to day.

I&#8217;ll explain in [my next post](https://wp.me/p8vFcV-tC) how to install R using that tap.

&nbsp;