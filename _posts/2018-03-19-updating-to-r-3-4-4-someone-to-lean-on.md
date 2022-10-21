---
title: 'Updating to R 3.4.4 "Someone to Lean On"'
date: 2018-03-19 11:56:02
header:
  overlay_image: /assets/images/blog/2018/someoonetoleanon.jpg
  teaser: /assets/images/blog/2018/someoonetoleanon.jpg
categories:
  - Professional
  - RStats
  - Technology
tags:
  - data science
  - homebrew
  - RStats
---
~~Just today yesterday~~ Last Thursday —took me more that I expected to have a moment to ~~write~~ finish this post— R was updated to 3.4.4 and this new released is called "Someone to Lean On", which is —[as all the rest are](http://livefreeordichotomize.com/2017/09/28/r-release-names/)— a reference to Peanuts comic. These are the released notes:

> NEW FEATURES:
>
>   * `Sys.timezone()` tries more heuristics on Unix-alikes and so is more likely to succeed (especially on Linux).  For the slowest method, a warning is given recommending that TZ is set to avoid the search.
>   * The version of LAPACK included in the sources has been updated to 3.8.0 (for the routines used by R, a very minor bug-fix change).
>   * `parallel::detectCores(logical = FALSE)` is ignored on Linux systems, since the information is not available with virtualized OSes.
>
> INSTALLATION on a UNIX-ALIKE:
>
>   * Configure will use pkg-config to find the flags to link to jpeg if available (as it should be for the recently-released jpeg-9c and libjpeg-turbo).  (This amends the code added in R 3.3.0 as the module name in jpeg-9c is not what that tested for.)
>
> DEPRECATED AND DEFUNCT:
>
>   * `Sys.timezone(location = FALSE)` (which was a stop-gap measure for Windows long ago) is deprecated.  It no longer returns the value of environment variable TZ (usually a location).
>   * Legacy support of make macros such as CXX1X is formally deprecated: use the CXX11 forms instead.
>   * BUG FIXES:
>   * `power.prop.test()` now warns when it cannot solve the problem, typically because of impossible constraints. (PR#17345)
>   * `removeSource()` no longer erroneously removes NULL in certain cases, thanks to D'enes T'oth.
>   * ``nls(`NO [mol/l]` ~ f(t))`` and `nls(y ~ a)` now work.  (Partly from PR#17367)
>   * R CMD build checks for GNU cp rather than assuming Linux has it. (PR#17370 says &#8216;Alpine Linux' does not.)
>   * Non-UTF-8 multibyte character handling fixed more permanently  (PR#16732).
>   * `sum(, )` is more consistent. (PR#17372)
>   * `rf() and rbeta()` now also work correctly when ncp is not scalar, notably when (partly) NA.  (PR#17375)
>   * `R CMD INSTALL` now correctly sets C++ compiler flags when all source files are in sub-directories of src.

If you are on macOS and you want to enjoy the improvements and the bug fixings, you just can download the binary from [CRAN](https://cran.r-project.org/bin/macosx/) or if you are a Homebrew user you just can update with the following command

```shell
brew update && brew upgrade
```

This time the binaries for macOS came almost as the same time as the binaries for the rest of the platforms and I really welcome the change and the diligence. Sometimes I feel as a macOS user a second class user in some open source projects, since they release the new versions a little bit later on macOS than in the rest of the platforms. However, I really appreciate the work that all these people put in develop R and to bring to it to the macOS ecosystem. I acknowledge that they are doing a non-payed job and they are doing it for the community of users, which is incredible remarkable.

On the other hand, this time Homebrew was the one falling back a little bit, and on Thursday afternoon the formula to install R hadn't been updated yet. So I just, just decided to update myself and make my first contribution to the Homebrew project. Update a Homebrew formula isn't rocket science, and they even have [a script](https://github.com/Homebrew/homebrew-core/blob/master/CONTRIBUTING.md#submit-a-version-upgrade-for-the-foo-formula) to make things easier, so I really encourage you to update Homebrew formula and contribute to the community. In the [next post](/blog/2018/03/19/how-i-updated-the-r-formula-in-homebrew/) I'll try to explain the procedure of how to update a Homebrew formula.

Happy data analysis with the new R.
