---
title: "How I publish my blog: Using Continuous Integration with Jekyll"
date: 2018-11-26 08:07 +00:00
# last_modified_at: 2018-11-11 20:45 +00:00
header: 
 overlay_image: /assets/images/blog/2018/jekyll+travis.png
 teaser: /assets/images/blog/2018/jekyll+travis.png
 caption: Jekyll and Travis logos
 overlay_filter: 0.3
image: /assets/images/blog/2018/jekyll+travis.png
categories: [Technology, Personal]
tags: [jekyll, blogging, CI, travis]
twitter: 
  image: /assets/images/blog/2018/jekyll+travis.png
#   hashtags: [tag1, tag2] 
toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
---

As I've mentioned [before](/blog/2018/07/23/new-web-in-jekyll/), this blog is build as a [static website](https://en.wikipedia.org/wiki/Static_web_page) with [Jekyll](https://jekyllrb.com), which allow me not to worry about data bases, logins or anything of the sorts when I'm publishing and managing the web. I use the free tier of [GitHub Pages](https://pages.github.com) as hosting, which has its downsides and upsides. The main upside is, it's really convenient and free. On the other hand, I'm limited to [certain Jekyll plugins](https://pages.github.com/versions/) if I want to use GitHub Pages building capabilities. This can be circumnavigate if you use CI (Continuous Integration) to build your site. 

## What is Continuous Integration?

You can read more about this in the [Wikipedia](https://en.wikipedia.org/wiki/Continuous_integration) —and in [Travis documentation](https://docs.travis-ci.com/user/for-beginners/#what-is-continuous-integration-ci)— but CI is basically —and I'm summarizing a lot here— just testing your code before you *integrate* —merge— it with the master branch, so you know that you aren't breaking anything —serious. Usually what you do is *building* your copy and then you merge it with the `master` branch. However, building is a pain, and even more if you have to do it all the time —continuously. For this reason, the CI services were born. 

Usually a CI works as it follows —again simplified: 

1. It's usually related / connected to your online repo and when you push commits to the repo the [webhooks](https://en.wikipedia.org/wiki/Webhook) are triggered and CI comes into action. 
2. CI download the last commits of your repo to a machine —usually virtual— with an OS you can specify and begin tu run a series of scripts[^1] to test and to build your software from the last commit you've pushed. 
3. If those scripts give an error, so your code can't be *build*, report to you as failing and —bad luck— you have to review what you have done because it's not working. 
4.  If the scripts exit without an error —you're lucky— and now a couple of things can happen. 
    - Just nothing else. You are notify there is not errors in your code —so you are free to merge. 
    - The code is automatically deployed. 
      - It's merged into another branch —usually `master`. 
      - It's deploy to a server or something else. 

All of these steps have a lot of sense in GitHub, GitLab and other repo online services where webhooks can be triggered even with a pull request, so you can know beforehand if someone else code is worth to review in an even of a pull request. 

That's more of less the official function of the CI. However, it can do much more than that. For example, in the Homebrew repos/taps it builds the bottles and deploys them to a hosting so you can latter download them. It can build software and apps and deploy it to your website. Or like in our case can build your Jekyll site and deploy to GitHub Pages. 

## How? 

There are several options out there as CI services, from [Travis](https://travis-ci.com) —with is one of the most common one— to [Jenkins](https://jenkins.io) or, if you use [GitLab, their own CI](https://about.gitlab.com/product/continuous-integration/). I'm going to talk about Travis, because it's the one I'm using myself and usually is the easier option. You can use Travis for free on the condition [your project is open source](https://travis-ci.com/plans). 

### Give access to Travis

First of all, you should go to the Travis website and authorize them to have access to your repos in GitHub. Usually, you just need to login with your GitHub account; they will ask for permission and if you just want to use Travis in all your repos or in just some of the repos. I have it set for only the repos I'm interested on because otherwise Travis can be trigged just pushing a commit to GitHub[^2]. 

### Configure your build

Once you have given access to Travis to the repo where you have your Jekyll page, you need to configure your build. You configure your build with the file `.travis.yml` in the root of your repo. 

Here is mine right now: 

```ruby
# .travis.yml
# This file should be at the root of your project
#
language: ruby
rvm:
 - 2.5.3
cache:
  bundler: true
  # directories:
  #   - $TRAVIS_BUILD_DIR/tmp/.htmlproofer
before_install:
  - gem install bundler
gemfile: Gemfile
script:
  - if [ "$TRAVIS_BRANCH" = "master" ]; then
      bundle exec jekyll algolia; 
      JEKYLL_ENV=production bundle exec jekyll build; 
      bundle exec htmlproofer ./_site --alt_ignore "/.*/" --allow_hash_href --http-status-ignore 999 --disable-external;
      fi 
  - if [ "$TRAVIS_BRANCH" = "develop" ]; then
      JEKYLL_ENV=production bundle exec jekyll build; 
      bundle exec htmlproofer ./_site --alt_ignore "/.*/" --allow_hash_href --http-status-ignore 999 --disable-external;
      fi 
branches:
  only:
    # Change this to gh-pages if you're deploying using the gh-pages branch
    - master
    - develop
env:
  global:
  - NOKOGIRI_USE_SYSTEM_LIBRARIES=true # speeds up installation of html-proofer

sudo: false # route your build to the container-based infrastructure for a faster build

deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_TOKEN  # Set in the settings page of your repository, as a secure variable
  keep-history: false
  local_dir: _site
  # repo: luispuerto/luispuerto.net # in case I wanted to build my page in other repo
  target-branch: gh-pages
  on:
    branch: master
```

You can learn more about how to create your own configuration file on [Travis Documentation](https://docs.travis-ci.com), but I'll walk you through my config: 

- [`language`](https://docs.travis-ci.com/user/tutorial/#selecting-a-different-programming-language): The language we are going to use in the build. In our case is Ruby, because Jekyll is build in Ruby. 
- [`rvm`](https://docs.travis-ci.com/user/languages/ruby/#Supported-Ruby-Versions): is the version of ruby we want to use. I'm using the last one —at this moment— 2.5.3. I don't know if you can set it up to use the last stable one, but some people argue that it isn't a good idea and you should set the version so it match the one in your machine —which has sense
- [`cache`](https://docs.travis-ci.com/user/caching/): You can cache content that usually don't change. In our case we are caching the bundle install because usually the gems we need don't change that often. 
- [`before_install`](https://docs.travis-ci.com/user/installing-dependencies/): Before we install the gems we are going to need to build our page we need to install the [`bundle`](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/). 
- [`gemfile`](https://docs.travis-ci.com/user/languages/ruby/): Location of the gemfile, so Travis knows what gems should install. 
- [`script`](https://docs.travis-ci.com/user/languages/ruby): This is the script to build. In ruby the default script is your rake file, but you can define anything else here —like a shell script which is much more simple. As you see I have a couple of entries here that run depending on the branch it's pushed. 
  - When I push `master` I run [Algolia](https://community.algolia.com/jekyll-algolia/github-pages.html), build the site and then past the [htmlproofer](https://github.com/gjtorikian/html-proofer). 
  - When I push `develop` I just build the site and past the [htmlproofer](https://github.com/gjtorikian/html-proofer).  
  
  **Please remember** to build your site with `JEKYLL_ENV=production` because if you don't some features aren't going to show in the final deployment.
  {: .notice--warning}
- [`branches`](https://docs.travis-ci.com/user/customizing-the-build/#building-specific-branches): This define which branches trigger the build. In my case `master` and `develop`, but you can add/remove whatever you want. 
- [`env`](https://docs.travis-ci.com/user/customizing-the-build/): You can define environment variables and in my case since I'm using [htmlproofer](https://github.com/gjtorikian/html-proofer) it's [recommended](https://jekyllrb.com/docs/continuous-integration/travis-ci/) to use `NOKOGIRI_USE_SYSTEM_LIBRARIES` as environment variable. 
- [`sudo`](https://docs.travis-ci.com/user/tutorial/#selecting-infrastructure-optional): We don't need sudo, since we don't need customizations. 

### Deploy

This is the last part of the config file, which is going to deploy the final built product —your website— to your repo again but in a different branch —`gh-pages` in my case. Travis [has a section](https://docs.travis-ci.com/user/deployment/pages/) in its documentation about deployment in GitHub Pages. 

Just a *couple* of notes: 

- `on`: States the branch is going to trigger the build it's the previous test exit with no errors. In other words, only when you push commits to `master` Travis is going to deploy your site. If you run Travis in other branches the deployment isn't going to happen. 
- `target_branch`: This is where is going to be deployed your site. In my case I have a branch only for deployments `gh-pages` which I don't even have locally. Be aware that Travis push with `--force` to that branch so it's going to overwrite the history in that branch leaving just one commit.
- `repo`: You can deploy in a different repo if you want, just define username and name of the repo. 
- `local_dir`: Folder from the build that is needed to be deployed, `_site` in my case. 
- `keep-history`: If you don't want Travis to push with `--force` you can set this option to true and it'll will keep the history of your branch. 
- `github-token`: You need to provide Travis with a [GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) with the `public_repo` or `repo` scope, depending if you repo is public or private. This token should be private and the best way use it is to configure it in your [repository settings](https://docs.travis-ci.com/user/environment-variables#defining-variables-in-repository-settings) in Travis. If you don't provide the token correctly, Travis isn't going to be able to deploy.
- `skip-cleanup`: You need to set this up as `true` if you don't want Travis to delete everything after it finish the built, which wind up deploying nothing. 
- `provider`: pages since you are deploying to GitHub Pages.

## Why?

Perhaps you are wondering why perform the building in your different branches and so. Well, have three permanent branches: `master`, `develop` and `writing`. I usually work on `develop` or `writing` and then I merge to `master`. I also use [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) for making changes in the source code of the site, and I'm thinking to try to implement it also for writing post and pages. 

In other words, I think it's a good idea to test that everything add up before I merge to `master` when I'm working on features. Writing, it's much more simple, and usually the errors are just that some link isn't correctly set, which is easy to fix. I sometimes use [htmlproofer](https://github.com/gjtorikian/html-proofer) locally when I'm writing so I know everything is OK before I merge to `master`. 

## LFS?

At this moment, I have my configuration exactly like you can see above. However, while I was writing this post, I just discovered that you can use [LFS with Travis](https://docs.travis-ci.com/user/customizing-the-build#git-lfs) —of course you can, why wouldn't you? You [can't use LFS with GitHub Pages](https://github.com/git-lfs/git-lfs/issues/791#issuecomment-151318020) though, but I wonder if you implement the build of your site with Travis while using LFS, you'll make it possible. Anyhow, you need to take into account the [bandwidth limitations of LFS](https://help.github.com/articles/about-storage-and-bandwidth-usage/) and it's probably that, every time you build with Travis you'll use part of your bandwidth. 

I'll try to write a post in the near future about [what is LFS (Large File Storage)](https://git-lfs.github.com), but summing it up a lot, it's just a system that allow you to manage better large binary files with Git, storing them in alternative storage instead of just tracking them with Git. 







[^1]: These scripts can be whatever you want —more or less.
[^2]: IIRC to trigger Travis you also need to have a `.travis.yml` file in the `root` of your repo but I'm not sure. Anyhow... sometimes I have repos in GitHub that have files `.travis.yml` and I don't want they run in CI. In the end you can do whatever you think it's more convenient for you. I don't think it's wise to try to build every time you are pushing to your repo and then receive an email of *your build is failing!*. 

