source "https://rubygems.org"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!

# gem "github-pages", group: :jekyll_plugins

# To upgrade, run `bundle update`.

# gem "minimal-mistakes-jekyll"
# The following plugins are automatically loaded by the theme-gem:
# group :mmistakes_plugins do
  # gem "jekyll-paginate" # included in github-pages gem.
  # gem "jekyll-sitemap" # included in github-pages gem.
  # gem "jekyll-gist" # included in github-pages gem.
  # gem "jekyll-feed" # included in github-pages gem & disable because I don't use it. 
  # gem "jemoji"  # included in github-pages gem.
  # gem "jekyll-data" # not needed out of the theme-gem. 
# end

# gem "github-pages"
# The following plugins are automatically loaded by the github-pages gem.
group :github_pages_plugins do
  # gem "jekyll" # I'm using other version of Jekyll. See bellow. 
  gem "jekyll-sass-converter"
  gem "kramdown"
  gem "jekyll-commonmark-ghpages"
  gem "liquid"
  gem "rouge"
  gem "github-pages-health-check"
  gem "jekyll-redirect-from"
  gem "jekyll-sitemap" # included in minimal-mistakes gem.
  # gem "jekyll-feed" # included in minimal-mistakes gem & disable because I don't use it.
  gem "jekyll-gist" # included in minimal-mistakes gem.
  gem "jekyll-paginate" # included in minimal-mistakes gem.
  gem "jekyll-coffeescript"
  gem "jekyll-seo-tag" #, github: "jekyll/jekyll-seo-tag", branch: "jekyll-cache"
  gem "jekyll-github-metadata" #, github: "jekyll/github-metadata", branch: "no-cache-drop"
  gem "jekyll-avatar"
  # gem "jekyll-remote-theme"
  gem "jemoji"
  gem "jekyll-mentions"
  # gem "jekyll-relative-links"
  # gem "jekyll-optional-front-matter"
  # gem "jekyll-readme-index"
  # gem "jekyll-default-layout"
  # gem "jekyll-titles-from-headings"
  gem "listen"
  gem "activesupport"
  gem "jekyll-swiss"
end 

# If you have any other plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-algolia", 
    :git => 'https://github.com/luispuerto/jekyll-algolia.git', 
    :branch => 'develop'
  gem 'html-proofer' , '~> 3'  
  gem "jekyll-archives"
  gem "jekyll-include-cache"
  # gem "rake" # disable because I don't use it.
  gem 'jekyll', '~> 4.0' #, github: "jekyll/jekyll" # allow to build Jekyll faster.
  gem "liquid-c" # allow to build Jekyll faster.
end