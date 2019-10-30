# [Luis Puerto](https://luispuerto.net) Source Code

[![Netlify Status](https://api.netlify.com/api/v1/badges/e747c978-bb75-4e89-baaa-d7e709185b5b/deploy-status)](https://app.netlify.com/sites/mystifying-haibt-8d1e50/deploys)
[![Build Status](https://img.shields.io/travis/luispuerto/luispuerto.net/master?logo=travis)](https://travis-ci.com/luispuerto/luispuerto.net)
[![Jekyll](https://img.shields.io/badge/jekyll-4.0.0-blue.svg?logo=jekyll)][Jekyll]
[![GitHub](https://img.shields.io/github/license/luispuerto/rants.luispuerto.net?label=code%20license&logo=open-source-initiative&color=#3DA639)][LICENSE]
[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/content%20license-CC%20BY--NC--SA%204.0-lightgrey?logo=creative-commons)][LICENSE4CONTENT]
[![Tip Michael via PayPal](https://img.shields.io/badge/PayPal-tip%20mmistakes-green.svg?logo=paypal)](https://www.paypal.me/mmistakes)

[![blog feed](https://img.shields.io/badge/feed-blog-yellow?style=social&logo=rss)][feed]
[![Twitter Follow](https://img.shields.io/twitter/follow/lpuerto?style=social)][twitter]
[![linkedin](https://img.shields.io/badge/linkedin-connect-blue?style=social&logo=linkedin)][linkedin]

This is the source code of my personal site [luispuerto.net][]. It's built using the [Jekyll][] and [Netlify][]. I use the [Minimal Mistakes][mmistakes-template] template from [Michael Rose][mmistakes-web] as a base and I've added some changes from my _own vintage_. Please feel free to fork my work since I would really happy to see that it helps others to build also their sites.  

<sub>**NB**: I've archived **my work** in this repo previous to October 27, 2018 [here][site-back]. You can know more about the reasons **why** I've done that in this [blog post](http://luispuerto.net/blog/2018/11/05/cutting-down-the-size-of-your-repo/).</span>

**Table of Contents**
<!-- MarkdownTOC -->

- [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
- [Built With](#built-with)
    - [Jekyll Plugins & Gems :gem:](#jekyll-plugins--gems-gem)
    - [Icons + Demo Images:](#icons--demo-images)
    - [Other:](#other)
- [How to use?](#how-to-use)
- [Contributing](#contributing)
- [Versioning](#versioning)
- [Authors](#authors)
- [License](#license)
- [Acknowledgments](#acknowledgments)

<!-- /MarkdownTOC -->

## Getting Started

You can get a copy of this repo running the following in your terminal: 

```shell
$ git clone https://github.com/luispuerto/luispuerto.net.git
```

### Prerequisites

The only prerequisite to run this blog locally is to have Jekyll on your machine. 

```shell
$ gem install jekyll bundler
```

Of course to be able to run Jekyll you need to have several other things on your system, like Ruby :gem:. Please check the [Jekyll docs][JekyllDocs] for more info about prerequisites. 

## Built With

This site is build using the following software and services. 

- [Jekyll][] - The static website generator I'm using to build my side. 
- [Minimal Mistakes][mmistakes-template] - The Jekyll template I'm using. 
- [Netlify][] - Where I'm building and hosting my site. 
- [TravisCI][] - I'm performing some continuous integration tasks over here, like for example index the site for [Algolia][] when I build it from master.  

### Jekyll Plugins & Gems :gem:

Since I've not publishing the site on GitHub pages and I'm building this page by other means, I have the freedom of using additional plugins and more granular control over what plugins I use on this site. These are the most important gems I'm using, but you can check the complete list on my [gem file][gem file].

- [jekyll-algolia][] - I'm using [Algolia][] as a search engine on the site, so this gem comes in handy to index the site every time I push a new version to master.  
- [jekyll-archives][] - I use this gem to create the [archives][luis-archives] of my site on a more compressive way. 
- [jekyll-include-cache][] - To be able to cache part some includes that repeat themselves in most of the pages. 
- [jemoji][] - The World without emojies is much more boring :man_shrugging:. 
- [kramdown][] - Markdown to HTML converter. 

### Icons + Demo Images:

Some of the icons and images come from the following sites:

- [The Noun Project][TheNounProject] - Icons for everything. 
- [Font Awesome][FontAwesome] - Vector icons and social logos.
- [Unsplash][] - Free images. 

### Other:

I use the additional services, snippets and JavaScript in this site. 

- [Algolia][] - I use their service on this site for searching and indexing the site. 
- [Jekyl Codex][JekylCodex] - I've used some snippets of code from this site for some solutions. Like for example open in a new window when you click in a link to a outside page. 
- [jQuery][] - jQuery is a JavaScript library designed to simplify HTML DOM tree traversal and manipulation, as well as event handling, CSS animation, and Ajax.
- [Susy][] - Susy is a lightweight grid-layout engine for Sass. 
- [Breakpoint][] - Breakpoint makes writing media queries in Sass simpler. 
- [Magnific Popup][MagnificPopup] - Responsive lightbox for images. 
- [FitVids.JS][] - Plugin for fluid width video embeds. 
- [GreedyNav.js][] - JS to handle the burger :hamburger: menu. 
- [Smooth Scroll][SmoothScroll] - A lightweight script to animate scrolling to anchor links.
- [Gumshoe][] - A simple vanilla JS scrollspy script.
- [jQuery throttle / debounce][JqueryThrottle/Debounce] - Allows you to rate-limit your functions in multiple useful ways.
- [Lunr][] - In site search engine
- [Bigfootjs][] - A better way to handle footnotes. 

## How to use? 

You can get a full and compressive documentation if you hit the [official Minimal Mistakes documentation][mmistakes-docs]. In this section thought I'll try to summarize mainly how to write post in the blog, how to do some updates, how to use some of the changes I've introduced, like more utility classes and so. 

## Contributing

[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-v1.4%20adopted-ff69b4.svg)](code-of-conduct.md)

Please read [CONTRIBUTING.md][] for details about how to contribute and the process of submitting a pull request to this repo. Note that we have subscribe to the Contributor Covenant as our code of conduct while participating in this community. Please read [CODE_OF_CONDUCT.md][] if you don't know about it. 

I more than happy to accept contributions to the code and even more to the content —like for example if you encounter a typo or something like that or you are a native English speaker and something you're reading isn't quite right. Even more if you think there is a mistake somewhere.

Please contribute.  

## Versioning

I don't use any versioning system —and I don't know if it's appropriate for a project like a blog— and in general I follow the versioning system used for the [Minimal Mistakes][mmistakes-template] template, which is [SemVer][]. You can check the available versions taking a look to the [tags on this repository][repo-tags]. 

## Authors

- **Michael Rose** - _Initial work and template creator_ - [@mmistakes][mmistakes-gh]
- **Luis Puerto** - _Maintainer and author of this fork and the blog that it hosts_ - [@luispuerto][luispuerto-gh]

See also the list of [contributors][] who participated in this project either on my side or in [mmistakes' ones][mmistakes-contri]. 

## License

Following Michael Rose, I've continued to license the code of this project under the MIT License —see the [LICENSE][LICENSE] file for details. However, the content of the website is license under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International license ——see the [LICENSE4CONTENT][] file for details. 

Minimal Mistakes template uses also a series of additional software and plugins, and I've also added some to this fork. You can see the licenses of those in the [LICENSE4VENDORS][] file. 

## Acknowledgments

I would like to specially acknowledge Michael Rose work creating the template I'm using that it's awesome. And of course all the [contributors to Minimal Mistakes template][mmistakes-contri]. 


[Jekyll]: https://jekyllrb.com
[LICENSE]: LICENSE
[LICENSE4CONTENT]: LICENSE4CONTENT.txt
[feed]: https://luispuerto.net/feed.xml
[twitter]: https://twitter.com/lpuerto
[linkedin]: https://www.linkedin.com/in/lpuerto
[luispuerto.net]: https://luispuerto.net
[Netlify]: https://netlify.com
[mmistakes-template]: https://mmistakes.github.io/minimal-mistakes/
[mmistakes-web]: https://mademistakes.com
[site-back]: https://github.com/luispuerto/luispuerto.net.back
[JekyllDocs]: https://jekyllrb.com/docs/
[TravisCI]: https://travis-ci.com
[Algolia]: https://www.algolia.com
[gem file]: Gemfile
[jekyll-algolia]: https://github.com/algolia/jekyll-algolia
[jekyll-archives]: https://github.com/jekyll/jekyll-archives
[luis-archives]: https://luispuerto.net/archive/
[jekyll-include-cache]: https://github.com/benbalter/jekyll-include-cache
[jemoji]: https://github.com/jekyll/jemoji
[kramdown]: https://github.com/gettalong/kramdown
[TheNounProject]: https://thenounproject.com
[FontAwesome]: http://fontawesome.io/
[Unsplash]: https://unsplash.com/
[JekylCodex]: https://jekyllcodex.org
[jQuery]: http://jquery.com/
[Susy]: http://susy.oddbird.net/
[Breakpoint]: http://breakpoint-sass.com/
[MagnificPopup]: http://dimsemenov.com/plugins/magnific-popup/
[FitVids.JS]: http://fitvidsjs.com/
[GreedyNav.js]: https://github.com/lukejacksonn/GreedyNav
[SmoothScroll]: https://github.com/cferdinandi/smooth-scroll
[Gumshoe]: https://github.com/cferdinandi/gumshoe
[JqueryThrottle/Debounce]: http://benalman.com/projects/jquery-throttle-debounce-plugin/
[Lunr]: http://lunrjs.com
[Bigfootjs]: http://www.bigfootjs.com
[CONTRIBUTING.md]: CONTRIBUTING.md
[SemVer]: http://semver.org/
[repo-tags]: https://github.com/luispuerto/luispuerto.net/tags
[mmistakes-gh]: https://github.com/mmistakes
[luispuerto-gh]: https://github.com/luispuerto
[contributors]: https://github.com/luispuerto/luispuerto.net/graphs/contributors
[mmistakes-contri]: https://github.com/mmistakes/minimal-mistakes/graphs/contributors
[LICENSE4VENDORS]: LICENSE4VENDORS.md
[CODE_OF_CONDUCT.md]: CODE_OF_CONDUCT.md
[mmistakes-docs]: https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/
