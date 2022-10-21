---
title: 'R 3.5 "Joy in Playing"'
date: 2018-04-28 18:52:30
header:
  overlay_image: /assets/images/blog/2018/4421de17ce2dc4cd3843ba00b224fbe0-header.jpeg
  teaser: /assets/images/blog/2018/4421de17ce2dc4cd3843ba00b224fbe0-header.jpeg
  caption: The joy is (in the) playing
image: /assets/images/blog/2018/4421de17ce2dc4cd3843ba00b224fbe0-header.jpeg
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
{% include figure image_path="/assets/images/blog/2018/4421de17ce2dc4cd3843ba00b224fbe0.jpg" alt="The joy is (in the) playing" caption="The joy is (in the) playing" %}{: .align-center}

Last Monday, 23rd of April, [R was updated](https://stat.ethz.ch/pipermail/r-announce/2018/000628.html) to its version 3.5, codenamed The _Joy in Playing_, which as the rest of the releases, make reference to a Peanuts' cartoon.

Since it's a [minor release](https://semver.org) (3.x) and not just a patch, it's advisable to reinstall all your packages, in some cases, to make then work properly. For example, and in my case, since I've built all of them as a consequence of my [Homebrew install](/blog/2018/01/12/install-r-100-homebrew-edition-with-openblas-openmp-my-version/), some of them where throwing me the following message

```R
Error: package ‘XXXXXXXXXXX’ was installed by an R version with different internals; it needs to be reinstalled for use with this R version
```

So, to reinstall all the packages that haven't been build for your current R build, you can run the following command. (Be careful, rebuild all your packages could take time and resources, so it's recommendable to do it at some moment that you aren't using your machine)

```R
update.packages(ask = F, repos = 'cloud.r-project.org', checkBuild = T)
```

I would recommend to run it twice, since some packages have dependencies and they need to be installed first and I don't know if the command follows a specific installation order to avoid this errors like this —in other words, if the dependencies aren't installed first to this specific release, the installation is going to fail. It doesn't hurt to run the command again since if all the packages were rebuilt with the current R version they aren't going to be reinstalled.

## Java and rJava configuration

In some of my machines I hadn't configured the new Java 10 with the prerelease rJava so [Java 10 can be run properly in R](/blog/2018/03/28/r-and-java-10/). If this is your case remember to run:

```shell
R CMD javareconf
```

so you yield something similar to:

```shell
Java interpreter : /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/bin/java
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
Done.
```

So you can install / build rJava prerelease with the following command

```R
install.packages('rJava', repos = 'http://rforge.net')
```

## devEMF

In another machine I wasn't being able to install devEMF package. You can see the specific error I was getting in this Stack Overflow [question](https://stackoverflow.com/questions/50075549/devemf-package-in-r-3-5-on-macos-doesnt-build/50076667#50076667).

The problem was the [makevars](/blog/2018/01/12/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#setting-the-final-makevars) file, which is crafted to use the [LLVM](https://llvm.org). I commented all of the lines to build the package and all set. Seems that for some reason LLVM isn't supported to build this package in this version of R (or it isn't supported at all).

```
# Remove the comment on -fopenmp for compiling data.table packag`akevars"># Remove the comment on -fopenmp for compiling data.table packagakevars"># Remove the comment on -fopenmp for compiling data.table packag`e
# CC=/usr/local/opt/llvm/bin/clang #-fopenmp
# CXX=/usr/local/opt/llvm/bin/clang++ #-fopenmp
# -O3 should be faster than -O2 (default) level optimisation ..
# CFLAGS=-g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe
# CXXFLAGS=-g -O3 -Wall -pedantic -std=c++11 -mtune=native -pipe
# LDFLAGS=-L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib
# CPPFLAGS=-I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include
```


Remember to uncomment them after you finish the build.

```shell
# Remove the comment on -fopenmp for compiling data.table package
CC=/usr/local/opt/llvm/bin/clang #-fopenmp
CXX=/usr/local/opt/llvm/bin/clang++ #-fopenmp
# -O3 should be faster than -O2 (default) level optimisation ..
CFLAGS=-g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe
CXXFLAGS=-g -O3 -Wall -pedantic -std=c++11 -mtune=native -pipe
LDFLAGS=-L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib
CPPFLAGS=-I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include
```

## data.table

Don't forget that data.table package has also specific [makevars](/blog/2018/01/12/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#setting-the-final-makevars) necessities if you are building it, as you should, with LLVM. Remember that the flag `-fopenmp` has to be present / uncommented in the lines related to C and C++ compilers.

```
CC=/usr/local/opt/llvm/bin/clang -fopenmp
CXX=/usr/local/opt/llvm/bin/clang++ -fopenmp
```

Remember to re-comment or delete the `-fopenmp` after you build data.table.
