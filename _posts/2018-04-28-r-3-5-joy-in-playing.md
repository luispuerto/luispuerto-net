---
id: 1800
title: 'R 3.5 &#8220;Joy in Playing&#8221;'
date: 2018-04-28T18:52:30+00:00
author: Luis Puerto
layout: 
guid: http://luisspuerto.net/?p=1800
permalink: /2018/04/r-3-5-joy-in-playing/
wtr-disable-reading-progress:
  - ""
wtr-disable-time-commitment:
  - ""
image: /wp-content/uploads/2018/04/4421de17ce2dc4cd3843ba00b224fbe0.jpg
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
<div id="attachment_1801" style="width: 510px" class="wp-caption aligncenter">
  <a href="http://luisspuerto.net/wp-content/uploads/2018/04/4421de17ce2dc4cd3843ba00b224fbe0.jpg"><img class="wp-image-1801 size-full" src="http://luisspuerto.net/wp-content/uploads/2018/04/4421de17ce2dc4cd3843ba00b224fbe0.jpg" alt="" width="500" height="448" srcset="http://luisspuerto.net/wp-content/uploads/2018/04/4421de17ce2dc4cd3843ba00b224fbe0.jpg 500w, http://luisspuerto.net/wp-content/uploads/2018/04/4421de17ce2dc4cd3843ba00b224fbe0-300x269.jpg 300w, http://luisspuerto.net/wp-content/uploads/2018/04/4421de17ce2dc4cd3843ba00b224fbe0-279x250.jpg 279w" sizes="(max-width: 500px) 100vw, 500px" /></a>
  
  <p class="wp-caption-text">
    The joy is (in the) playing
  </p>
</div>

Last Monday, 23rd of April, [R was updated](https://stat.ethz.ch/pipermail/r-announce/2018/000628.html) to its version 3.5, codenamed The _Joy in Playing_, which as the rest of the releases, make reference to a Peanuts&#8217; cartoon.

Since it&#8217;s a [minor release](https://semver.org) (3.x) and not just a patch, it&#8217;s advisable to reinstall all your packages, in some cases, to make then work properly. For example, and in my case, since I&#8217;ve built all of them as a consequence of my [Homebrew install](http://luisspuerto.net/2018/01/install-r-100-homebrew-edition-with-openblas-openmp-my-version/), some of them where throwing me the following message

<pre class="wrap:true lang:r decode:true">Error: package ‘XXXXXXXXXXX’ was installed by an R version with different internals; it needs to be reinstalled for use with this R version</pre>

So, to reinstall all the packages that haven&#8217;t been build for your current R build, you can run the following command. (Be careful, rebuild all your packages could take time and resources, so it&#8217;s recommendable to do it at some moment that you aren&#8217;t using your machine)

<pre class="lang:r decode:true ">update.packages(ask = F, repos = 'cloud.r-project.org', checkBuild = T)</pre>

I would recommend to run it twice, since some packages have dependencies and they need to be installed first and I don&#8217;t know if the command follows a specific installation order to avoid this errors like this –in other words, if the dependencies aren&#8217;t installed first to this specific release, the installation is going to fail. It doesn&#8217;t hurt to run the command again since if all the packages were rebuilt with the current R version they aren&#8217;t going to be reinstalled.

# Java and rJava configuration

In some of my machines I hadn&#8217;t configured the new Java 10 with the prerelease rJava so [Java 10 can be run properly in R](http://luisspuerto.net/2018/03/r-and-java-10/). If this is your case remember to run:

<pre class="lang:default decode:true">$ R CMD javareconf</pre>

so you yield something similar to:

<pre class="lang:default decode:true">Java interpreter : /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/bin/java
Java version     : 10.0.1
Java home path   : /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home
Java compiler    : /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/bin/javac
Java headers gen.: /usr/bin/javah
Java archive tool: /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/bin/jar

trying to compile and link a JNI program
detected JNI cpp flags    : -I$(JAVA_HOME)/include -I$(JAVA_HOME)/include/darwin
detected JNI linker flags : -L$(JAVA_HOME)/lib/server -ljvm
/usr/local/opt/llvm/bin/clang  -I"/usr/local/Cellar/r/3.5.0/lib/R/include" -DNDEBUG -I/Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/include -I/Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/include/darwin  -I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include   -fPIC  -g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe -c conftest.c -o conftest.o
/usr/local/opt/llvm/bin/clang -dynamiclib -Wl,-headerpad_max_install_names -undefined dynamic_lookup -single_module -multiply_defined suppress -L/usr/local/Cellar/r/3.5.0/lib/R/lib -L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib -o conftest.so conftest.o -L/Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/lib/server -ljvm -L/usr/local/Cellar/r/3.5.0/lib/R/lib -lR -lintl -Wl,-framework -Wl,CoreFoundation


JAVA_HOME        : /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home
Java library path: $(JAVA_HOME)/lib/server
JNI cpp flags    : -I$(JAVA_HOME)/include -I$(JAVA_HOME)/include/darwin
JNI linker flags : -L$(JAVA_HOME)/lib/server -ljvm
Updating Java configuration in /usr/local/Cellar/r/3.5.0/lib/R
Done.</pre>

So you can install / build rJava prerelease with the following command

<pre class="lang:default decode:true">install.packages('rJava', repos = 'http://rforge.net')</pre>

# devEMF

In another machine I wasn&#8217;t being able to install devEMF package. You can see the specific error I was getting in this Stack Overflow [question](https://stackoverflow.com/questions/50075549/devemf-package-in-r-3-5-on-macos-doesnt-build/50076667#50076667).

The problem was the [makevars](http://luisspuerto.net/2018/01/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#setting-the-final-makevars) file, which is crafted to use the [LLVM](https://llvm.org). I commented all of the lines to build the package and all set. Seems that for some reason LLVM isn&#8217;t supported to build this package in this version of R (or it isn&#8217;t supported at all).

<pre class="lang:default decode:true" title="makevars"># Remove the comment on -fopenmp for compiling data.table package
# CC=/usr/local/opt/llvm/bin/clang #-fopenmp
# CXX=/usr/local/opt/llvm/bin/clang++ #-fopenmp
# -O3 should be faster than -O2 (default) level optimisation ..
# CFLAGS=-g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe
# CXXFLAGS=-g -O3 -Wall -pedantic -std=c++11 -mtune=native -pipe
# LDFLAGS=-L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib
# CPPFLAGS=-I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include</pre>

Remember to uncomment them after you finish the build.

<pre class="lang:default decode:true"># Remove the comment on -fopenmp for compiling data.table package
CC=/usr/local/opt/llvm/bin/clang #-fopenmp
CXX=/usr/local/opt/llvm/bin/clang++ #-fopenmp
# -O3 should be faster than -O2 (default) level optimisation ..
CFLAGS=-g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe
CXXFLAGS=-g -O3 -Wall -pedantic -std=c++11 -mtune=native -pipe
LDFLAGS=-L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib
CPPFLAGS=-I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include</pre>

# data.table

Don&#8217;t forget that data.table package has also specific [makevars](http://luisspuerto.net/2018/01/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#setting-the-final-makevars) necessities if you are building it, as you should, with LLVM. Remember that the flag `-fopenmp` has to be present / uncommented in the lines related to C and C++ compilers.

<pre class="lang:default decode:true ">CC=/usr/local/opt/llvm/bin/clang -fopenmp
CXX=/usr/local/opt/llvm/bin/clang++ -fopenmp</pre>

Remember to re-comment or delete the `-fopenmp` after you build data.table.