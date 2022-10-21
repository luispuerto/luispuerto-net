---
title: How I updated the R formula in Homebrew
date: 2018-03-19 15:35:21
header:
  overlay_image: /assets/images/blog/2018/R-Homebrew.jpg
  teaser: /assets/images/blog/2018/R-Homebrew.jpg
categories:
  - RStats
  - Technology
tags:
  - data science
  - homebrew
  - RStats
---
As I just explained in my [last post](/blog/2018/03/19/updating-to-r-3-4-4-someone-to-lean-on/), the last version of R —3.4.4 "Someone to Lean on"— was rolled out las Thursday and this time I was the one that updated the Homebrew's formula to reflect the new version. I just decided to give it a try and update for first time a Homebrew's formula since no one had updated it yet on Thursday afternoon and this way I would be able to install the new version with [Homebrew](http://brew.sh).

As I've already mentioned, I really encourage anyone to contribute to Homebrew's community and try to keep the formulae update. Homebrew also encourage this, since as more people keep an eye on formula and update them, the better for the community.

## How can you update a Homebrew formula?

The first and foremost important thing is to check if someone already filed a [pull request](https://github.com/Homebrew/homebrew-core/pulls) for that same formulae, or in other words, if that formula is in the process of being updated. If no one is updating that formula, it's opportunity to contribute and update it for the benefit of the community.

Before you begin, you need to update Homebrew to get the last version:

```shell
brew update
```

When you have the last version of Homebrew, you can edit your formula. If you just going to update the formula because there is a new version of the software that the formula installs, the best way to go is to use the script Homebrew provided to update formulae. In the case of R formula —r.rb— the syntax was as follows.

```shell
brew bump-formula-pr --strict R with --url=https://cran.r-project.org/src/base/R-3/R-3.4.4.tar.gz and --sha256=b3e97d2fab7256d1c655c4075934725ba1cd7cb9237240a11bb22ccdad960337
```

In this case, the URL and the sha256 was provided by Peter Dalgaard in the [R 3.4.4 announcement](https://stat.ethz.ch/pipermail/r-announce/2018/000626.html). However, you have to take into account that [https urls are preferred](https://docs.brew.sh/Formula-Cookbook), so I had to [recommit](https://github.com/Homebrew/homebrew-core/pull/25321/commits/3c5e5438e79ccd655b0c5ee1bb4adbae1ddd6702) to follow that guidelines.

If you don't have the sha256, you can calculate it yourself downloading the file and then running the following command that for me was as follows:

```shell
openssl sha -sha256 ~/Downloads/R-3.4.4.tar.gz
```

When you have those two parameters, you are ready to run the command. What the command does is basically create a fork of Homebrew in your account of Github, and then create a branch where it's going to upload the modified formula. Then, it just makes a pull request to [Homebrew](https://github.com/Homebrew)/[homebrew-core](https://github.com/Homebrew/homebrew-core).  If everything is ok, they'll accept your changes and the new formula will be accessible to everyone after they run `$ brew update`. On the other hand, if you have to make modifications in the formula you are submitting in your pull request —as I needed to make— you just do them using [Git](https://git-scm.com/book/en/v2/Git-Branching-Branch-Management).

```shell
cd /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core # go to your local repo for Homebrew
git branch # here you select the branch with the name of the update you are creating
brew edit R # your preferred editor with open
```

Now, you can make the necessary changes and when you are done you just save the file and

```shell
git add R.rb # in this specific case
git commit -m "fixed the previous error"
git push YourForkOfTheRepo TheNameOfTheBranch
```

If you want to test your version of the formula this is easy, you just stay in the branch where the modified formula is and run:

```shell
brew upgrade R
```

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2018-03-15-at-17.33.09.png" alt="Updating R with Homebrew" caption="Updating R with Homebrew" %}

## Use someone else's formula

You can even use this updated formula to [update the R of someone computer](https://docs.brew.sh/FAQ) while they accept your pull request —if you are in a hurry or you are antsy guy. To do so you can use [hub](https://hub.github.com).

```shell
brew install hub
brew update
cd $(brew --repository)
hub pull someone_else
```

Or you can directly install the formula that is their repo in GitHub:

```shell
brew install https://raw.github.com/user/repo/branch/formula.rb
```

In my case:

```shell
brew install https://github.com/luisspuerto/homebrew-core/blob/r-3.4.4/Formula/r.rb
```

Or pull the formula from the pull request:

```shell
brew pull https://github.com/Homebrew/homebrew-core/pull/1234
```

In my case:

```shell
brew pull https://github.com/Homebrew/homebrew-core/pull/25321
```

However, I don't recommend you to do something like this and install the someone's formula unless you really know the person. You never know what it's in the install script. Usually, pull request are resolved within hours and it's much better practice to wait until the Homebrew maintainers review the updated formula and approve it.
