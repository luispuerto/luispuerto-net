---
title: Install R 100% Homebrew Edition With OpenBlas & OpenMP — My Version
date: 2018-01-12 20:16:56
header:
  overlay_image: assets/images/blog/2018/R-Homebrew.jpg
  teaser: assets/images/blog/2018/R-Homebrew.jpg
toc: true
toc_icon: chart-line
categories:
  - Professional
  - RStats
  - Technology
tags:
  - homebrew
  - how to
  - optimization
  - RSoft
  - RStats
---

**Update Friday, 10th of May 2018**: If you want to install R with all the capabilities you need to read this [post](/blog/2018/05/11/installing-r-with-homebrew-with-all-the-capabilities/) too, and perhaps this [one](/blog/2018/05/11/homebrews-r-doesnt-have-all-the-capabilities/) too.

**Update Tuesday, 27th of March 2018:** I just found out that seems you don't just need to run `sudo R CMD javareconf` to configure Java an R, at least with the versions of Java 9.0.4 and R 3.4.4.

**Update Thursday, 22nd of March 2018:** I have to add `-fopenmp` to both `clang` and `clang++` variables in my `makevars` to be able to build data.table package correctly. This is not exactly what the [Data Table wiki](https://github.com/Rdatatable/data.table/wiki/Installation#openmp-enabled-compiler-for-mac) recommends. I update the [section about the Data Table Package](/blog/2018/01/12/install-r-100-homebrew-edition-with-openblas-openmp-my-version/#data-table-package) accordingly.

* * *

As you know I'm a big fan of [Homebrew](/archive/tags/homebrew) as a manager of part of the software of my Mac, since it make things easier. There are a lot of guides out there about [how to have a R installation 100% Homebrew](https://www.google.com/search?client=safari&rls=en&q=r+homebrew&ie=UTF-8&oe=UTF-8) and some people, like me, like to have this kind of setup because it's convenient and for the sake of lear a little bit more about how R works in more detail. However, Homebrew setup isn't officially supported by the [R Core Team](https://scholar.google.com/citations?user=yvS1QUEAAAAJ), so if you find problems with your R installation you aren't going to get support from them. Nevertheless, you are going to be able to get support from [Homebrew](https://github.com/Homebrew/homebrew-core) and of course, from the [regular channels to get help for R](https://www.r-project.org/help.html), like the [mail list](https://www.r-project.org/mail.html).

The biggest advantage, besides of the regular advantages of installing something with HomeBrew, is you can create your own version of R, you can compile it, therefore you can compile it with steroids, so you can take advantage of the OpenBlas and OpenMP libraries.

## OpenBLAS & OpenMP

[OpenBLAS](https://en.wikipedia.org/wiki/OpenBLAS) is a open implementation of the BLAS (Basic Linear Algebra Subprograms) API. Basically, it optimizes your processor when you are doing mathematical operations, like when you are using R. It's usually a huge leap in performance when you begin to make complex mathematical operations.

[OpenMP](https://en.wikipedia.org/wiki/OpenMP) is a library for Open Multi-Processing, or in other words, be able to use all the cores of your processor when you are compiling C, C++, and Fortran. If also make R to process faster since some packages are able to use all the cores of your computer after you compile them with OpenMP.

In other words, you are going to increase your performance a lot with this setup, as Mauricio Vargas demonstrate in his last two post ([Why is R slow? some explanations and MKL/OpenBLAS setup to try to fix this](https://www.r-bloggers.com/why-is-r-slow-some-explanations-and-mklopenblas-setup-to-try-to-fix-this/) and Is [Microsoft R Open faster than CRAN R?](http://pacha.hk/blog/2017/12/02/is-microsoft-r-open-faster-than-cran-r/)).

We also did a small test since we wanted to get this setup in one of our computers, a MacBook Air from 2013. So, we used the [MicroBenchmarks](https://cran.r-project.org/web/packages/microbenchmark/index.html) package with the below script (from [Alexej Gossmann's Blog](http://www.alexejgossmann.com/benchmarking_r/)) and we got the following results.

```R
library(microbenchmark)

set.seed(2017)
n <- 10000
p <- 100
X <- matrix(rnorm(n*p), n, p)
y <- X %*% rnorm(p) + rnorm(100)

check_for_equal_coefs <- function(values) {
  tol <- 1e-12
  max_error <- max(c(abs(values[[1]] - values[[2]]),
                     abs(values[[2]] - values[[3]]),
                     abs(values[[1]] - values[[3]])))
  max_error < tol
}

mbm <- microbenchmark("lm" = { b <- lm(y ~ X + 0)$coef },
               "pseudoinverse" = {
                 b <- solve(t(X) %*% X) %*% t(X) %*% y
               },
               "linear system" = {
                 b <- solve(t(X) %*% X, t(X) %*% y)
               },
               check = check_for_equal_coefs)

mbm
```

We got this results on a Macbook Pro 2010:

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2018-05-11-at-10.42.09.png" %}{: .align-center }

```R
# Base R benchmarks results

Unit: milliseconds
          expr      min       lq     mean   median       uq      max neval
            lm 299.5747 319.5861 339.4531 324.8694 331.7090 521.0330   100
 pseudoinverse 326.1059 344.3830 358.1566 351.7829 359.7690 508.8802   100
 linear system 199.0780 206.6064 218.7704 210.2198 218.2907 327.6886   100
```

```R
# R with openblas and LLVM

Unit: milliseconds
          expr       min        lq      mean    median        uq      max neval
            lm 262.22400 272.39873 287.72378 277.65483 286.07772 361.0826   100
 pseudoinverse  60.50899  62.65356  82.10815  70.40881  75.11090 169.7922   100
 linear system  38.01025  39.48672  52.82579  45.81922  49.36025 121.0351   100
```

I really think the results speak for themselves.

## Caveats

Of course there are some problems when you have this kind of install. The first one is the complication of the install process. If it were as simple as install R binaries from CRAN I wouldn't be doing this guide. The second one and more important, **you are going to need to compile the packages you install from now on**, without exception. You aren't going to be able to install the binaries of the packages anymore. This has advantages and disadvantages. The main advantage is that they are going to make use of the libraries you have installed in your your system like OpenBLAS, OpenMP or LLVM, to mention some. However, this means that you are going to need some other libraries to compile and you have to have them correctly linked, like Java or libxml2 or some of the packages aren't going to compile and you aren't going to be able to have it on your system.

In case you get any problem internet is your friend. You can look for the error R is returning when it tries to compile. If you are the first one to get that error you can ask in communities like [Stackoverflow](https://stackoverflow.com) or the mail list for [R help](https://www.r-project.org/mail.html). All of these is going to make you understand R much better and your are going to be a better R user. So take it with patience and consider it like an advance course for R.

Take into account that sometimes even the CRAN install binaries pose problems, mostly with it's link to Java. Before I decided to have this kind of install with R I had in the past multiple problems with Java and rJava package. So nothing is perfect, but you didn't decided to use R because it was simple, did you?

## How to install?

I've used as inspiration for this guide mainly two main sources. On one hand, ~~Bhaskar Karambelar's installation guide~~ (no longer online),   and on the other [Mauricio Vargas' one](http://pacha.hk/blog/2017/01/08/install-r-and-python-via-homebrew/). Bhaskar's one was the first I used, more than 6 months ago, while we were in the United Stated, and really worked well in that moment. Problem with it is, it installs a lot or libraries to program in C/C++ what unless you are a C/C++ programmer you aren't going to use, although you never know. At that moment, I installed everything due to lack of knowledge, but probably right now I wouldn't. It's up to you if you want to install those libraries and programing languages. However, I have more than enough space in my hard drive and I don't mind to have then, perhaps they are going to to be useful in the future. Besides, this has been a way to discover then and know more about C/C++ programing. Mauricio's guide goes more to the point and it just helps you to install a really fast and quick version of R that use OpenMP and OpenBlas.

Through this guide I just want to try to show you how I ended with my installation, that is an updated mixture of both guides.  However, take into account that mine guide is going to be a little bit different, even more taking into account that [I use Zsh](/blog/2018/01/09/iterm2-oh-my-zsh-powerlevel9k-monaco-nerd-complete-font/) as my shell.

### Homebrew

You probably have Homebrew already installed, if you don't, please, [install it](/blog/2017/11/21/homebrew/). Then, I recommend you to connect to the cask tap if you haven't done it already:

```shell
brew tap caskroom/cask # Tap to install regular app with user interface (GUI)
```

As you probably you've noticed, I don't tap a lot of repos that Bhaskar taped. This is mainly because those taps are deprecated and its formulae are now included in the Homebrew Core. I decided not to tap other repos because I'm not going to use them.

I recommend to add the following lines in your Zsh and/or Bash profiles running the following:

```shell
# For zsh
echo '# Setting language and localization variables
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8' >> ~/.zshrc

# For bash
echo '# Setting language and localization variables
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8' >> ~/.bash_profile
```

### Uninstalling previous R install

If you already have R installed I recommend you to uninstall R completely. Before you do it, you perhaps want to do a copy of your installed packages, just make a list because you are going to need to compile all of them with this homebrew install. You can run the following code in R to make a copy of your packages.

```R
# How to list installed packajes

package_matrix <- installed.packages()

package_df <- data.frame(package_matrix)

package_list <- package_df[is.na(package_df$Priority), "Package"]

packages <- as.character(package_list)

write(packages, file = "packages")

save(packages, file = "packages.RData")
```

Now you are going to have a file in your working folder packages.RData that is going to store a variable with a list of all your packages. To reinstall all the packages you just need to load that file in R and run:

```R
install.packages(packages)
```

Now that you have a list of your installed packages you can [delete R from your system](https://cran.r-project.org/doc/manuals/r-release/R-admin.html#Uninstalling-under-macOS). Run the following on terminal:

```shell
sudo rm -rf /Library/Frameworks/R.framework /Applications/R.app /usr/local/bin/R /usr/local/bin/Rscript
```

### XCode Command Line Tools

You need to have installed the [Command Line Tools for XCode](https://developer.apple.com/download/more/). Please be aware that if you already has installed, XCode you probably still need to install the CLT. The best way to know is running the following command in terminal:

```shell
xcode-select --install
```

### C/C++ Compilers and Libraries

Now, you need to install the C/C++ necessary compilers and other useful libraries.

```shell
brew install gcc ccache cmake pkg-config autoconf automake
```

You can

```shell
cd /usr/local/bin
ln -s gcov-7 gcov
ln -s gcc-7 gcc
ln -s g++-7 g++
ln -s cpp-7 cpp
ln -s c++-7 c++
cd ~
```

You can ask fo the versions to check if everything is correctly installed. You have to get something similar to this:

```shell
gcc --version
gcc (Homebrew GCC 7.2.0) 7.2.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

gfortran -v
Using built-in specs.
COLLECT_GCC=gfortran
COLLECT_LTO_WRAPPER=/usr/local/Cellar/gcc/7.2.0/libexec/gcc/x86_64-apple-darwin17.2.0/7.2.0/lto-wrapper
Target: x86_64-apple-darwin17.2.0
Configured with: ../configure --build=x86_64-apple-darwin17.2.0 --prefix=/usr/local/Cellar/gcc/7.2.0 --libdir=/usr/local/Cellar/gcc/7.2.0/lib/gcc/7 --enable-languages=c,c++,objc,obj-c++,fortran --program-suffix=-7 --with-gmp=/usr/local/opt/gmp --with-mpfr=/usr/local/opt/mpfr --with-mpc=/usr/local/opt/libmpc --with-isl=/usr/local/opt/isl --with-system-zlib --enable-checking=release --with-pkgversion='Homebrew GCC 7.2.0' --with-bugurl=https://github.com/Homebrew/homebrew-core/issues --disable-nls --with-native-system-header-dir=/usr/include --with-sysroot=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.13.sdk
Thread model: posix
gcc version 7.2.0 (Homebrew GCC 7.2.0)

ccache --v
ccache version 3.3.4
```

You can check also if the OpenMP from GCC is working running the following on terminal:

```shell
cat > omp-test.c <<"END"
#include <omp.h>
#include <stdio.h>
int main() {
    #pragma omp parallel
    printf("Hello from thread %d, nthreads %d\n", omp_get_thread_num(), omp_get_num_threads());
}
END
gcc -fopenmp -o omp-test omp-test.c
./omp-test
```

And you should get something similar to:

```shell
Hello from thread 1, nthreads 8
Hello from thread 6, nthreads 8
Hello from thread 4, nthreads 8
Hello from thread 2, nthreads 8
Hello from thread 5, nthreads 8
Hello from thread 0, nthreads 8
Hello from thread 3, nthreads 8
Hello from thread 7, nthreads 8
```

### Miscellaneous graphical libraries -optional

```shell
brew install freetype fontconfig pixman gettext
```

Some of these libraries aren't strictly necessary for R, but they are to install other related apps like QGIS, GRASS or PostGIS. I think that if you don't want to install then you don't need to do it right now, since that software install its on dependencies

### SSL/SSH Libraries -optional {#ssl-ssh-libs}

If you already have Git you probably have OpenSSL, the other two are optional.

```shell
brew install openssl libressl libssh2
```

```shell
/usr/local/opt/openssl/bin/openssl version
OpenSSL 1.0.2n  7 Dec 2017

/usr/local/opt/libressl/bin/openssl version
LibreSSL 2.2.7
```

### Libxml2

It's highly recomendable to install this library since it's somehow necessary to install some packages depending on the version of your macOS system. It's really small (10 mb) so you are losing nothing installing it.

```shell
brew install libxml2
brew link libxml2 --force
```

### Boost -optional

[Boost](http://www.boost.org) is one of those libraries that you only install if you program in C/C++. If you want to install it you need to have Libxml2 installed and then proceed as following:

```shell
brew install icu4c libiconv libxslt
brew install boost --with-icu4c --without-single
```

Then you can test if it's correctly installed

```shell
cat > first.cpp <<END
#include<iostream>
#include<boost/any.hpp>
int main()
{
    boost::any a(5);
    a = 1.61803;
    std::cout << boost::any_cast<double>(a) << std::endl;
}
END
clang++ -I/usr/local/include -L/usr/local/lib  -o first first.cpp
./first
```

```shell
1.61803
```

```shell
cat > second.cpp <<END
#include<iostream>
#include <boost/filesystem.hpp>
int main()
{
    boost::filesystem::path full_path( boost::filesystem::current_path() );
    if ( boost::filesystem::exists( "second.cpp" ) )
    {
        std::cout << "Found second.cpp file in " << full_path << std::endl;
    } else {
        std::cerr << "Argh!, Something not working" << std::endl;
        return 1;
    }
}
END
clang++ -I/usr/local/include -L/usr/local/lib  -o second second.cpp \
    -lboost_filesystem-mt -lboost_system-mt
./second
```

```shell
Found second.cpp file in "/Users/brewmaster"
```

### GPG & Git

I've already explained how to install [GPG in a previous post](/blog/2017/11/04/installing-pgp-signing-for-git-on-macos/) to use it with [Git](/blog/2017/11/05/set-rstudio-with-homebrews-git/). How to install Git was also [explained](/blog/2017/11/05/set-rstudio-with-homebrews-git/).

### X-Server

You are going to probably need X-Server down the road.

```shell
brew cask install xquartz
```

### Latex

[Latex](https://en.wikipedia.org/wiki/LaTeX) is a set of applications and libraries to be able to write beautiful mathematical formulas and documents, mainly. But can be use to write any kind of documents.

```shell
brew cask install mactex
```

### Java

If you don't have [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) installed it's a good moment to do so and to do it with Homebrew.

```shell
brew cask install java
```

```shell
java -version
java version "9.0.1"
Java(TM) SE Runtime Environment (build 9.0.1+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, mixed mode)
```

### Python

It's recommended to install [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) 2 and 3 as a complement to R although R itself doesn't use it.

```shell
brew install python
sudo easy_install pip
pip install --upgrade pip setuptools
pip install markdown rpy2
python -V # checking the version
Python 2.7.10
```

rply2 is probably to give you an error untill you install R. You can try to install it right now and if it give you the error install again lately.

```shell
brew install python3
pip3 install --upgrade pip setuptools wheel
python3 -V # Checking the version
Python 3.6.4
```

### R & related

We are going to install some things before we install R itself. [Pandoc](https://en.wikipedia.org/wiki/Pandoc) is really useful when you have R to convert documents in different formats. [Cairo](https://en.wikipedia.org/wiki/Cairo_(graphics)) is a graphical library that can be use for in R and it's need for [QGIS](https://en.wikipedia.org/wiki/QGIS). Libsvg and librsvg are optional

**Important!**: If you want to have R with all the capabilities you need to install Cairo with the instructions in this [post](/blog/2018/05/11/installing-r-with-homebrew-with-all-the-capabilities/).

```shell
brew install pandoc cairo libsvg librsvg
```

#### OpenBLAS

Let's install OpenBLAS, this is one of the key pieces of this installation.

```shell
brew install openblas --with-openmp
```

Now you can test if OpenBlas has been correctly installed.

```shell
cat > test-openblas.c <<"END"
#include <cblas.h>
#include <stdio.h>

void main()
{
  int i=0;
  double A[6] = {1.0,2.0,1.0,-3.0,4.0,-1.0};
  double B[6] = {1.0,2.0,1.0,-3.0,4.0,-1.0};
  double C[9] = {.5,.5,.5,.5,.5,.5,.5,.5,.5};
  cblas_dgemm(CblasColMajor, CblasNoTrans, CblasTrans,
      3,3,2,1,A, 3, B, 3,2,C,3);

  for(i=0; i<9; i++)
    printf("%lf ", C[i]);
  printf("\n");
}
END

clang -L/usr/local/opt/openblas/lib \
    -I/usr/local/opt/openblas/include \
    -lopenblas -lpthread \
    -o test-openblas test-openblas.c

./test-openblas
```

```shell
11.000000 -9.000000 5.000000 -9.000000 21.000000 -1.000000 5.000000 -1.000000 3.000000
```

#### Armadillo and other libraries -optional

Now, you can also install, if you want, [Armadillo](https://en.wikipedia.org/wiki/Armadillo_(C%2B%2B_library)), which is other library that it's useful if you program in C/C++ and take advantage of OpenBLAS.

```shell
brew install eigen armadillo v8-315
brew link v8-315 --force
```

You can test Armadillo with the following code since the new Armadillo doesn't provide examples, or at least I haven't found them.

```shell
cat > example1.cpp <<END

#include

#include "armadillo"

using namespace arma;

using namespace std;

int main(int argc, char** argv)

  {

  cout << "Armadillo version: " << arma_version::as_string() << endl;

  // directly specify the matrix size (elements are uninitialised)

  mat A(2,3);

  // .n_rows = number of rows    (read only)

  // .n_cols = number of columns (read only)

  cout << "A.n_rows = " << A.n_rows << endl;

  cout << "A.n_cols = " << A.n_cols << endl;

  // directly access an element (indexing starts at 0)

  A(1,2) = 456.0;

  A.print("A:");

  // scalars are treated as a 1x1 matrix,

  // hence the code below will set A to have a size of 1x1

  A = 5.0;

  A.print("A:");

  // if you want a matrix with all elements set to a particular value

  // the .fill() member function can be used

  A.set_size(3,3);

  A.fill(5.0);

  A.print("A:");

  mat B;

  // endr indicates "end of row"

  B << 0.555950 << 0.274690 << 0.540605 << 0.798938 << endr

    << 0.108929 << 0.830123 << 0.891726 << 0.895283 << endr

    << 0.948014 << 0.973234 << 0.216504 << 0.883152 << endr

    << 0.023787 << 0.675382 << 0.231751 << 0.450332 << endr;

  // print to the cout stream

  // with an optional string before the contents of the matrix

  B.print("B:");

  // the << operator can also be used to print the matrix

  // to an arbitrary stream (cout in this case)

  cout << "B:" << endl << B << endl;

  // save to disk

  B.save("B.txt", raw_ascii);

  // load from disk

  mat C;

  C.load("B.txt");

  C += 2.0 * B;

  C.print("C:");

  // submatrix types:

  //

  // .submat(first_row, first_column, last_row, last_column)

  // .row(row_number)

  // .col(column_number)

  // .cols(first_column, last_column)

  // .rows(first_row, last_row)

  cout << "C.submat(0,0,3,1) =" << endl;

  cout << C.submat(0,0,3,1) << endl;

  // generate the identity matrix

  mat D = eye<mat>(4,4);

  D.submat(0,0,3,1) = C.cols(1,2);

  D.print("D:");

  // transpose

  cout << "trans(B) =" << endl;

  cout << trans(B) << endl;

  // maximum from each column (traverse along rows)

  cout << "max(B) =" << endl;

  cout << max(B) << endl;

  // maximum from each row (traverse along columns)

  cout << "max(B,1) =" << endl;

  cout << max(B,1) << endl;

  // maximum value in B

  cout << "max(max(B)) = " << max(max(B)) << endl;

  // sum of each column (traverse along rows)

  cout << "sum(B) =" << endl;

  cout << sum(B) << endl;

  // sum of each row (traverse along columns)

  cout << "sum(B,1) =" << endl;

  cout << sum(B,1) << endl;

  // sum of all elements

  cout << "sum(sum(B)) = " << sum(sum(B)) << endl;

  cout << "accu(B)     = " << accu(B) << endl;

  // trace = sum along diagonal

  cout << "trace(B)    = " << trace(B) << endl;

  // random matrix -- values are uniformly distributed in the [0,1] interval

  mat E = randu<mat>(4,4);

  E.print("E:");

  cout << endl;

  // row vectors are treated like a matrix with one row

  rowvec r;

  r << 0.59499 << 0.88807 << 0.88532 << 0.19968;

  r.print("r:");

  // column vectors are treated like a matrix with one column

  colvec q;

  q << 0.81114 << 0.06256 << 0.95989 << 0.73628;

  q.print("q:");

  // dot or inner product

  cout << "as_scalar(rq) = " << as_scalar(rq) << endl;

  // outer product

  cout << "q*r =" << endl;

  cout << q*r << endl;

  // multiply-and-accumulate operation

  // (no temporary matrices are created)

  cout << "accu(B % C) = " << accu(B % C) << endl;

  // sum of three matrices (no temporary matrices are created)

  mat F = B + C + D;

  F.print("F:");

  // imat specifies an integer matrix

  imat AA;

  imat BB;

  AA << 1 << 2 << 3 << endr << 4 << 5 << 6 << endr << 7 << 8 << 9;

  BB << 3 << 2 << 1 << endr << 6 << 5 << 4 << endr << 9 << 8 << 7;

  // comparison of matrices (element-wise)

  // output of a relational operator is a umat

  umat ZZ = (AA >= BB);

  ZZ.print("ZZ =");

  // 2D field of arbitrary length row vectors

  // (fields can also store abitrary objects, e.g. instances of std::string)

  field<rowvec> xyz(3,2);

  xyz(0,0) = randu(1,2);

  xyz(1,0) = randu(1,3);

  xyz(2,0) = randu(1,4);

  xyz(0,1) = randu(1,5);

  xyz(1,1) = randu(1,6);

  xyz(2,1) = randu(1,7);

  cout << "xyz:" << endl;

  cout << xyz << endl;

  // cubes ("3D matrices")

  cube Q( B.n_rows, B.n_cols, 2 );

  Q.slice(0) = B;

  Q.slice(1) = 2.0 * B;

  Q.print("Q:");

  return 0;

  }

END

clang++   -O2   -o example1  example1.cpp  -larmadillo -framework Accelerate

./example1
```

You are going to get something like:

```shell
Armadillo version: 8.300.3 (Tropical Shenanigans)
A.n_rows = 2
A.n_cols = 3
A:
  1.8965e-256  6.9531e-310  6.9531e-310
  6.9531e-310  4.9407e-324   4.5600e+02
A:
   5.0000
A:
   5.0000   5.0000   5.0000
   5.0000   5.0000   5.0000
   5.0000   5.0000   5.0000
B:
   0.5560   0.2747   0.5406   0.7989
   0.1089   0.8301   0.8917   0.8953
   0.9480   0.9732   0.2165   0.8832
   0.0238   0.6754   0.2318   0.4503
B:
   0.5560   0.2747   0.5406   0.7989
   0.1089   0.8301   0.8917   0.8953
   0.9480   0.9732   0.2165   0.8832
   0.0238   0.6754   0.2318   0.4503

C:
   1.6679   0.8241   1.6218   2.3968
   0.3268   2.4904   2.6752   2.6858
   2.8440   2.9197   0.6495   2.6495
   0.0714   2.0261   0.6953   1.3510
C.submat(0,0,3,1) =
   1.6679   0.8241
   0.3268   2.4904
   2.8440   2.9197
   0.0714   2.0261

D:
   0.8241   1.6218        0        0
   2.4904   2.6752        0        0
   2.9197   0.6495   1.0000        0
   2.0261   0.6953        0   1.0000
trans(B) =
   0.5560   0.1089   0.9480   0.0238
   0.2747   0.8301   0.9732   0.6754
   0.5406   0.8917   0.2165   0.2318
   0.7989   0.8953   0.8832   0.4503

max(B) =
   0.9480   0.9732   0.8917   0.8953

max(B,1) =
   0.7989
   0.8953
   0.9732
   0.6754

max(max(B)) = 0.973234
sum(B) =
   1.6367   2.7534   1.8806   3.0277

sum(B,1) =
   2.1702
   2.7261
   3.0209
   1.3813

sum(sum(B)) = 9.2984
accu(B)     = 9.2984
trace(B)    = 2.05291
E:
   7.8264e-06   5.3277e-01   6.7930e-01   8.3097e-01
   1.3154e-01   2.1896e-01   9.3469e-01   3.4572e-02
   7.5561e-01   4.7045e-02   3.8350e-01   5.3462e-02
   4.5865e-01   6.7886e-01   5.1942e-01   5.2970e-01

r:
   0.5950   0.8881   0.8853   0.1997
q:
   0.8111
   0.0626
   0.9599
   0.7363
as_scalar(r*q) = 1.53501
q*r =
   0.4826   0.7203   0.7181   0.1620
   0.0372   0.0556   0.0554   0.0125
   0.5711   0.8524   0.8498   0.1917
   0.4381   0.6539   0.6518   0.1470

accu(B % C) = 20.9962
F:
   3.0479   2.7206   2.1624   3.1958
   2.9261   5.9957   3.5669   3.5811
   6.7118   4.5424   1.8660   3.5326
   2.1213   3.3968   0.9270   2.8013
ZZ =
        0        1        1
        0        1        1
        0        1        1
xyz:
[field column 0]
   0.6711   0.0077

   0.3834   0.0668   0.4175

   0.6868   0.5890   0.9304   0.8462


[field column 1]
   0.5269   0.0920   0.6539   0.4160   0.7012

   0.9103   0.7622   0.2625   0.0475   0.7361   0.3282

   0.6326   0.7564   0.9910   0.3653   0.2470   0.9826   0.7227



Q:
[cube slice 0]
   0.5560   0.2747   0.5406   0.7989
   0.1089   0.8301   0.8917   0.8953
   0.9480   0.9732   0.2165   0.8832
   0.0238   0.6754   0.2318   0.4503

[cube slice 1]
   1.1119   0.5494   1.0812   1.5979
   0.2179   1.6602   1.7835   1.7906
   1.8960   1.9465   0.4330   1.7663
   0.0476   1.3508   0.4635   0.9007
```

You can also test V8

```shell
echo 'quit()' | v8
V8 version 3.15.11.18 [sample shell]
```

#### R

Important!: If you want to have R with all the capabilities you have to follow the instructions in this [post](/blog/2018/05/11/installing-r-with-homebrew-with-all-the-capabilities/), then you can continue.

Let's finally install R.

```shell
brew install R --with-openblas --with-java
```

Then if you are using english (american english) as your main language I recommend you to run the following:

```shell
defaults write org.R-project.R force.LANG en_US.UTF-8
```

#### Java9+R

First you have to insert the following line in your Zsh and/or Bash profiles.

```shell
# For zsh
echo '# Setting $JAVA_HOME
export JAVA_HOME="$(/usr/libexec/java_home)"' >> ~/.zshrc

# For bash
echo '# Setting $JAVA_HOME
export JAVA_HOME="$(/usr/libexec/java_home)"' >> ~/.bash_profile
```

And then run the following command in the terminal:

```shell
sudo R CMD javareconf JAVA_CPPFLAGS='-I/System/Library/Frameworks/JavaVM.framework/Headers -I/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/' # this is a specific command for Java 9.0.1
```

It seems that with Java 9.0.4 and R 3.4.4 you can run instead just:

```shell
sudo R CMD javareconf
```

or perhaps:

```shell
sudo R CMD javareconf JAVA_CPPFLAGS='-I/$JAVA_HOME'
```

You have to get something similar to this:

```shell
/usr/local/Cellar/r/3.4.3/lib/R/bin/javareconf: line 66: -I/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/: No such file or directory
Java interpreter : /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/bin/java
Java version     : 9.0.1
Java home path   : /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home
Java compiler    : /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/bin/javac
Java headers gen.: /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/bin/javah
Java archive tool: /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/bin/jar
Non-system Java on macOS

trying to compile and link a JNI program
detected JNI cpp flags    : -I$(JAVA_HOME)/include -I$(JAVA_HOME)/include/darwin
detected JNI linker flags : -L$(JAVA_HOME)/lib/server -ljvm
/usr/local/opt/llvm/bin/clang -I/usr/local/Cellar/r/3.4.3/lib/R/include -DNDEBUG -I/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/include -I/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/include/darwin  -I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include   -fPIC  -g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe -c conftest.c -o conftest.o
/usr/local/opt/llvm/bin/clang -dynamiclib -Wl,-headerpad_max_install_names -undefined dynamic_lookup -single_module -multiply_defined suppress -L/usr/local/Cellar/r/3.4.3/lib/R/lib -L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib -o conftest.so conftest.o -L/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/lib/server -ljvm -L/usr/local/Cellar/r/3.4.3/lib/R/lib -lR -lintl -Wl,-framework -Wl,CoreFoundation


JAVA_HOME        : /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home
Java library path: $(JAVA_HOME)/lib/server
JNI cpp flags    : -I$(JAVA_HOME)/include -I$(JAVA_HOME)/include/darwin
JNI linker flags : -L$(JAVA_HOME)/lib/server -ljvm
Updating Java configuration in /usr/local/Cellar/r/3.4.3/lib/R
Done.
```

#### Folder for R Packages

Let's create our own folder to store the installed packages for R. This way R, or us, doesn't have to move all the packages every time we install a new R version. Run the following in terminal.

```shell
mkdir -p $HOME/Library/R/3.x/library
cat > $HOME/.Renviron <<END
R_LIBS_USER=$HOME/Library/R/3.x/library
END
```

You should also add this variable to your zsh and/or bash profiles.

```shell
  # For zsh
echo 'export R_LIBS_USER=$HOME/Library/R/3.x/library' >> ~/.zshrc

# For bash
echo 'export R_LIBS_USER=$HOME/Library/R/3.x/library' >> ~/.bash_profile
```

#### LLVM

[LLVM](https://en.wikipedia.org/wiki/LLVM) or _Low Level Virtual Machine_ is a library that allow us to compile faster some R packages using OpenMP and also make that those packages use OpenMP when we are normally using R. To install it you run on your terminal the following:

```shell
brew install llvm
```

Then insert the LLVM location to your path in your Zsh and/or Bash profiles:

```shell
# For zsh
echo 'export PATH=/usr/local/opt/llvm/bin:$PATH' >> ~/.zshrc

# For bash
echo 'export PATH=/usr/local/opt/llvm/bin:$PATH' >> ~/.bash_profile
```

#### Data Table Package

The package [Data Table](https://github.com/Rdatatable/data.table/wiki) need a [specific](https://github.com/Rdatatable/data.table/wiki/Installation#openmp-enabled-compiler-for-mac) [makevars](https://www.rdocumentation.org/packages/tools/versions/3.4.3/topics/makevars) file. Makevars file is the file that tells R how and with what libraries it has to compile the packages we download from source. So we are going to install Data Table first, with that specific configuration and then set the final makevars file.

```shell
mkdir ~/.R
echo "CC=/usr/local/opt/llvm/bin/clang -fopenmp
CXX=/usr/local/opt/llvm/bin/clang++ -fopenmp
# -O3 should be faster than -O2 (default) level optimisation ..
CFLAGS=-g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe
CXXFLAGS=-g -O3 -Wall -pedantic -std=c++11 -mtune=native -pipe
LDFLAGS=-L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib
CPPFLAGS=-I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include" >> ~/.R/Makevars
```

Now we can install Data Table package on terminal. To do so just run on terminal:

```shell
R --vanilla << EOF
install.packages('data.table', repos='http://cran.us.r-project.org')
q()
EOF
```

#### Setting the final Makevars

First, we delete the previous makevars file.

```shell
rm ~/.R/Makevars
```

Set the final Makevars file.

```shell
echo "CC=/usr/local/opt/llvm/bin/clang
CXX=/usr/local/opt/llvm/bin/clang++
# -O3 should be faster than -O2 (default) level optimisation ..
CFLAGS=-g -O3 -Wall -pedantic -std=gnu99 -mtune=native -pipe
CXXFLAGS=-g -O3 -Wall -pedantic -std=c++11 -mtune=native -pipe
LDFLAGS=-L/usr/local/opt/gettext/lib -L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib
CPPFLAGS=-I/usr/local/opt/gettext/include -I/usr/local/opt/llvm/include" >> ~/.R/Makevars
```

As you probably have noticed the change is just the `-fopenmp` flag on the second line. In case you have to reinstall, or update Data Table, you just have to add that flag and then delete it. Really easy and you can even do it from RStudio.

### RStudio

When you install R from Homebrew and you compile it, you don't have anymore the R shell as an application on your Applications folder. But you can install any other graphical interface like [RStudio](https://www.rstudio.com). To do it you just run in your terminal:

```shell
brew cask install rstudio
```

Usually RStudio is able to recognize R install and you don't need to do anything else.

### Additional related languages -optional

You can also install some additional related languages like:

#### Node.js {#node-js}

From Wikipedia:

> **[Node.js](https://en.wikipedia.org/wiki/Node.js)** is an [open-source](https://en.wikipedia.org/wiki/Open-source_software), [cross-platform](https://en.wikipedia.org/wiki/Cross-platform) [JavaScript](https://en.wikipedia.org/wiki/JavaScript) [run-time environment](https://en.wikipedia.org/wiki/Runtime_system) that executes JavaScript code outside the browser.

To install it just run:

```shell
brew install node phantomjs casperjs
```

#### Scala

From Wikipedia:

> **[Scala](https://en.wikipedia.org/wiki/Scala_(programming_language))** ([/ˈskɑːlɑː/](https://en.wikipedia.org/wiki/Help:IPA/English) [*SKAH-lah*](https://en.wikipedia.org/wiki/Help:Pronunciation_respelling_key))[[9\]](https://en.wikipedia.org/wiki/Scala_(programming_language)#cite_note-9) is a [general-purpose](https://en.wikipedia.org/wiki/General-purpose_programming_language) [programming language](https://en.wikipedia.org/wiki/Programming_language) providing support for [functional programming](https://en.wikipedia.org/wiki/Functional_programming) and a strong [static](https://en.wikipedia.org/wiki/Static_typing) [type system](https://en.wikipedia.org/wiki/Type_system). Designed to be concise,[[10\]](https://en.wikipedia.org/wiki/Scala_(programming_language)#cite_note-10) many of Scala's design decisions aimed to address [criticisms of Java](https://en.wikipedia.org/wiki/Criticism_of_Java).[[8\]](https://en.wikipedia.org/wiki/Scala_(programming_language)#cite_note-overview-8)

To install it just run in your terminal:

```shell
brew install scala
```

#### golang

From Wikipedia:

>**[Go](https://en.wikipedia.org/wiki/Go_(programming_language))** (often referred to as **Golang**) is a [programming language](https://en.wikipedia.org/wiki/Programming_language) created at [Google](https://en.wikipedia.org/wiki/Google)[[12\]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-techcrunch-12) in 2009 by Robert Griesemer, [Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike), and [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson).[[10\]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-langfaq-10) Go is a [statically typed](https://en.wikipedia.org/wiki/Static_typing), [compiled](https://en.wikipedia.org/wiki/Compiled_language) language in the tradition of [C](https://en.wikipedia.org/wiki/C_(programming_language)), with [memory safety](https://en.wikipedia.org/wiki/Memory_safety), [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)), [structural typing](https://en.wikipedia.org/wiki/Structural_type_system),[[3\]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-structural_typing-3) and [CSP](https://en.wikipedia.org/wiki/Communicating_sequential_processes)-style [concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science)).[[13\]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-boldly-13)

To install it just run in your terminal:

```shell
brew install golang
```

You need to modify your zsh and/or bash profile like the following

```shell
# For zsh
echo '## Path for Golang
export GOPATH=$HOME/golang
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin' >> ~/.zshrc

# For bash
echo '## Path for Golang
export GOPATH=$HOME/golang
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin' >> ~/.bash_profile
```

### Some GIS Libraries & Soft -optional

You can also install some GIS libraries. This libraries could be mandatory if you are going to install geographical packages:

```shell
brew tap osgeo/osgeo4mac # Tap for geospatial software
brew install postgresql geos proj
brew install gdal2 --with-complete --with-opencl --with-armadillo --with-unsupported --with-libkml --with-postgresql
brew install postgis --with-gui
```

### Shell Profiles

You've been adding things to your Zsh and/or Bash profiles. I recommend you to make those profiles tidy, it's going to be easier to modify things in the future.

This is how I have then:

```shell
# Setting language and localization variables
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

# Setting $JAVA_HOME
export JAVA_HOME="$(/usr/libexec/java_home)"

# Setting $PATH
## If you come from bash you might have to change your $PATH.
export PATH=$HOME/bin:$PATH
export PATH=/usr/local/sbin:$PATH
# export PATH=/usr/local/bin:$PATH
## Path for LLVM install
export PATH=/usr/local/opt/llvm/bin:$PATH
## Path for Golang
export GOPATH=$HOME/golang
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin

# Setting R variables
export R_LIBS_USER=$HOME/Library/R/3.x/library
```

You can see those files running the following:

```shell
# If you have atom.
# zsh
atom ~/.zshrc
# bash
atom ~/.bash_profile

# If you don't have atom
#zsh
open -a TextEdit ~/.zshrc
#bash
open -a TextEdit ~/.bash_profile
```

## The End

Now you can begin to use your new R.
