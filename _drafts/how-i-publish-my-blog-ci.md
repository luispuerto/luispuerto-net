---
title: "How I publish my blog: Using Continuous Integration with Jekyll"
# date: 2018-11-01 00:00 +00:00
# last_modified_at: 2018-11-11 20:45 +00:00
header: 
 overlay_image: /assets/images/blog/2018/jekyll+travis.png
 teaser: /assets/images/blog/2018/jekyll+travis.png
 caption: Jekyll and Travis logos
 overlay_filter: 0.3
image: /assets/images/blog/2018/jekyll+travis.png
categories: 
 - Technology
 - Personal
tags: [jekyll, blogging, CI, travis]
toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
---

As I've mention [before](/blog/2018/07/23/new-web-in-jekyll/), this blog is build as a [static website](https://en.wikipedia.org/wiki/Static_web_page) with [Jekyll](https://jekyllrb.com), which allow me not to worry about data bases, logins or anything of the sorts when I'm publishing and managing the web. I use the free tier of [GitHub Pages](https://pages.github.com) as hosting. This has its downsides and upsides, the main upside is, it's really convenient and free. On the other hand, I'm limited to [certain Jekyll plugins](https://pages.github.com/versions/) if I want to use GitHub Pages building capabilities. This can be circumnavigate if you use continuous integration to build your site. 

## What is Continuous Integration?

You can read about this in the [Wikipedia](https://en.wikipedia.org/wiki/Continuous_integration), but CI is —summarizing a lot— basically just testing your code before you *integrate* —merge— it with the master branch, so you know that you aren't breaking anything —serious. Usually what you do is *building* your copy and then you merge it with the master branch. However, building is a pain, and even more if you have to do it all the time. For this reason, the CI services were born. 

Usually a CI works as it follows —again simplified: 

- It's usually related / connected to your online repo and when you push commits to the repo the [webhooks](https://en.wikipedia.org/wiki/Webhook) are triggered and CI comes into action. 
- CI download the last commits of your repo to a machine —usually virtual— with a OS you can specify and begin tu run a series of scripts[^1]. 
- If those scripts give an error, so your code can't be *build* report to you as failing and —bad luck— you have to review what you have done because it's not working. 
- If the scripts exit without an error —you're lucky— and now a couple of things can happen. 
  - Just nothing else. You are notify there is not errors in your code —so you are free to merge. 
  - The code is automatically deployed. 
    - It's merged into another branch —usually master. 
    - It's deploy to a server or something else. 

All of these have a lot of sense in GitHub, GitLab and other repo online services where webhooks can be triggered even with a pull request, so you can know beforehand if someone else code is worth to review in an even of a pull request. 

That's more of less the official function of the CI. However, it can do much more than that. For example is Homebrew repos it builds the bottles. It can build software and apps and deploy it to your website. Or like in our case can build your Jekyll site and deploy to GitHub Pages. 

## How? 

There are several options out there as CI services, from [Travis](https://travis-ci.com) —with is one of the most common one— to [Jenkins](https://jenkins.io) or, if you use [GitLab, their own CI](https://about.gitlab.com/product/continuous-integration/). I'm going to talk about Travis, because it's the one I'm using myself. 

### Give access to Travis

First of all, you should go to the Travis website and autorice them to have access to your repos in GitHub. Usually, just login with your GitHub account, they will ask for permission and if you just want to use Travis in all your repos or in some of the repos. I have it set for only the repos I'm interested on because otherwise Travis can be trigged just pushing a commit to GitHub[^2]. 

### Configure your build

Once you have given access to Travis to repo where you have your Jekyll page, you need to configure your build. You configure your build with the file `.travis.yml` in the root of your repo. 

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

You can learn more about how to create your own configuration file on Travis Documentation, but I'll walk you through my config: 

- [`language`](https://docs.travis-ci.com/user/tutorial/#selecting-a-different-programming-language): The language we are going to use in the build. In our case is Ruby, because Jekyll is build in Ruby. 
- [`rvm`](https://docs.travis-ci.com/user/languages/ruby/#Supported-Ruby-Versions): is the version of ruby we want to use. I'm using the last one —at this moment— 2.5.3. 
- [`cache`](https://docs.travis-ci.com/user/caching/): You can cache content that usually don't change. I our case we are caching the bundle install because usually the gems we need don't change that often. 
- [`before_install`](https://docs.travis-ci.com/user/installing-dependencies/): Before we install the gems we are going to need to build our page we need to install the `bundle`. 
- [`gemfile`](https://docs.travis-ci.com/user/languages/ruby/): Location of the gemfile, so Travis can install the gems. 
- [`script`](https://docs.travis-ci.com/user/languages/ruby): This is the script to build. In ruby the default script is your rake file, but you can define anything else here. As you see I have a couple of entries here that run depending on the branch it's pushed. 
  - When I push master I run [Algolia](https://community.algolia.com/jekyll-algolia/github-pages.html), build the site and then past the [htmlproofer](https://github.com/gjtorikian/html-proofer). 
  - When I push develop I just build the site and past the past the [htmlproofer](https://github.com/gjtorikian/html-proofer).

  **Please remember** to build your site with `JEKYLL_ENV=production` because if you don't some features aren't going to show in the final deployment.
  {: .notice--warning}

- [`branches`](https://docs.travis-ci.com/user/customizing-the-build/#building-specific-branches): This define which branches trigger the build. In my case master and develop, but you can add whatever you want. 
- [`env`](https://docs.travis-ci.com/user/customizing-the-build/): You can define environment variables and in my case since I'm using [htmlproofer](https://github.com/gjtorikian/html-proofer) it's [recommended](https://jekyllrb.com/docs/continuous-integration/travis-ci/) to use that environment variable. 
- [`sudo`](https://docs.travis-ci.com/user/tutorial/#selecting-infrastructure-optional): We don't need sudo, since we don't need customizations. 

### Deploy

This is the last part of the config file, which is going to deploy the final product —your website— to your repo again but in a different branch —gh-pages in my case. Travis [has a section](https://docs.travis-ci.com/user/deployment/pages/) in its documentation about deployment in GitHub Pages. 

Just a *couple* of notes: 

- `on`: States the branch is going to trigger the build it's the previous test exit with no errors. In other words, only when you push commits to master travis is going to deploy your site, even if you run travis in other branches. 
- `target_branch`: This is where is going to be deploy your site. In my case I have a branch only for deployments `gh-pages` which I don't even have locally. Be aware the Travis push with `--force` to that branch so it's going to overwrite the history in that branch. 
- `repo`: You can deploy in a different repo if you want, just define username and name of the repo. 
- `local_dir`: Folder from the build that is going to be deployed, `_site` in my case. 
- `keep-history`: If you don't want Travis to push with `--force` you can set this option to true and it'll will keep the history of your branch. 
- `github-token`: You need to provide Travis with a [GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) with the `public_repo` or `repo` scope, depending if you repo is public or private. This token should be private and the best way use it is to configure it in your [repository settings](https://docs.travis-ci.com/user/environment-variables#defining-variables-in-repository-settings) in Travis.
- `skip-cleanup`: You need to set this up as `true` if you don't want Travis to delete everything after it finish, which wind up deploying nothing. 
- `provider`: pages since you are deploying to GitHub Pages.

## One more thing

At this moment, I have my configuration like exactly like that, and right now 







[^1]: These scripts can be whatever you want —more or less.
[^2]: IIRC to trigger Travis you also need to have a `.travis.yml` file in the `root` of your repo but I'm not sure. Anyhow... sometimes I have repos in GitHub that have files `.travis.yml` and I don't want they run in CI. In the end you can do whatever you think it's more convenient for you.

