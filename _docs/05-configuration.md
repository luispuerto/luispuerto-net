---
title: "Configuration"
permalink: /docs/configuration/
excerpt:
sidebar:
  title: "v3.0"
  nav: docs
modified: 2016-04-13T15:54:02-04:00
---

{% include base_path %}

Settings that affect your entire site can be changed in [Jekyll's configuration file](https://jekyllrb.com/docs/configuration/): `_config.yml`, found in the root of your project.

**Note:** for technical reasons, `_config.yml` is NOT reloaded automatically when used with `jekyll serve`. If you make any changes to this file, please restart the server process for them to be applied.
{: .notice--warning}

Take a moment to look over the configuration file included with the theme. Light comments have been added to provide examples and defaults values for most variables. Detailed explanations of each can be found below.

## Site Settings

### Site Locale

`site.locale` is used to declare the primary language for each web page within the site.

*Example:* `locale: "en-US"` sets the `lang` attribute for the site to *United States* flavor of English, while `en-GB` would be for the `United Kingdom` style of English. Country codes are optional and the shorter `locale: "en"` is also acceptable. To find your language and country codes check this [reference table](https://msdn.microsoft.com/en-us/library/ee825488(v=cs.20).aspx). 

Properly setting the locale is important for associating localized text found in the **UI Text** data file. For more information on that see below.

### Site Title

The name of your site. Is used throughout the theme in places like masthead and `<title>` tags.

*Example:* `title: "My Awesome Site"`

You also have the option of customizing the character used in SEO-friendly page titles.

*Example:* `title_separator: "|"` would produce page titles like `Sample Page | My Awesome Site`.

### Site Name

Used to assign a site author. Don't worry, you can assign different authors on specific posts, pages, or collections if you desire.

*Example:* `name: "Michael Rose"`.

**ProTip:** If you want to get crafty with your YAML you can use [anchors](http://www.yaml.org/spec/1.2/spec.html#id2785586) to reuse values. For example `foo: &var "My String"` allows you to reuse `"My String"` elsewhere in `_config.yml` like so... `bar: *var`. You'll see a few examples of this in the provided Jekyll config.
{: .notice--info}

### Site Description

Fairly obvious. `site.description` describes the site. Used predominantly in meta descriptions as part of SEO efforts.

*Example:* `description: "A flexible Jekyll theme for your blog or site with a minimalist aesthetic."`

### Site URL

The base hostname and protocol for your site. If you're hosting with GitHub Pages this will be something like `url: "http://github.io.mmistakes"`, or for self-hosting `url: "https://mademistakes.com"`.

**Note:** It's important to remember that when testing locally you need to change this. Ideally you'd use [multiple config files](https://mademistakes.com/articles/using-jekyll-2016/#environments-and-configurations) with `bundle exec jekyll serve --config _config.yml,_config.dev.yml` to apply development override settings. Simply commenting out the line works as well `# url: "http://mmistakes.github.io"`. Just remember to uncomment it before pushing or else you'll have broken assets and links all over the place!
{: .notice--warning}

**ProTip:** GitHub serves pages over `http://` and `https://` so to take advantage of that go protocol-less like so `url: "//github.io.mmistakes"`.
{: .notice--info}

### Site Base URL

This little option causes all kinds of confusion in the Jekyll community. If you're not hosting your site as a GitHub Pages Project or in a subfolder (eg: `/blog`), then don't mess with it.

In the case of the Minimal Mistakes demo site it's hosting on GitHub Pages at <https://mmistakes.github.io/minimal-mistakes>. To correctly set this base path I'd use `url: "https://mmistakes.github.io"` and `baseurl: "/minimal-mistakes"`.

For more information on how to properly use `site.url` and `site.baseurl` as intended by the Jekyll maintainers, check [Parker Moore's post on the subject](https://byparker.com/blog/2014/clearing-up-confusion-around-baseurl/).

**Note:** When using `baseurl` remember to include it as part of your path when testing your site locally. Values of `url: ` and `baseurl: "/blog"` would make your local site visible at `http://localhost:4000/blog` and not `http://localhost:4000`.
{: .notice--warning}

### Site Default Teaser Image

To assign a fallback teaser image used in modules like the "**Related Posts**" module, place a graphic in the `/images/` directory and add the filename to `_config.yml` like so:

```yaml
teaser: "500x300.png"
```

This image can be overridden at anytime by applying the following to a document's YAML Front Matter.

```yaml
header:
  teaser: my-awesome-post-teaser.jpg
```

<figure>
  <img src="{{ base_path }}/images/mm-teaser-images-example.jpg" alt="teaser image example">
  <figcaption>Teasers images as shown in the grid archive view for related posts.</figcaption>
</figure>

### Breadcrumb Navigation (Beta)

Enable breadcrumb links to help visitors better navigate deeply structure sites. Because of the fragile method of implementing them they don't always produce accurate links reliably. For best results:

1. Use a category based permalink structure e.g. `permalink: /:categories/:title/`
2. Manually create pages for each category or use a plugin like [jekyll-archives][jekyll-archives] to auto-generate them. If these pages don't exist breadcrumb links to them will be broken.

![breadcrumb navigation example]({{ base_path }}/images/mm-breadcrumbs-example.jpg)

```yaml
breadcrumbs: true  # disabled by default
```

Breadcrumb start link text and separator character can both be changed in the UI Text data file.

### Reading Time

Enable estimated reading time snippets with `read_time: true` in YAML Front Matter. 200 has been set as the default words per minute, which can be changed by adjusting `words_per_minutes: 200` in `_config.yml`.

![reading time example]({{ base_path }}/images/mm-read-time-example.jpg)

Instead of adding YAML Front Matter to each document, apply as a default in `_config.yml`. To enable the **reading time** snippet for all posts:

```yaml
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      read_time: true
```

If you add `read_time: false` to a post's YAML Front Matter it will override the default and "reading time" for just that post.

### Comments

Commenting for [**Disqus**](https://disqus.com/), [**Facebook**](https://developers.facebook.com/docs/plugins/comments), and **Google+** are built into the theme. First set the comment provider you'd like to use: 

|               | Comment Provider  |
| ------------- | ----------------  |
| `disqus`      | Disqus            |
| `facebook`    | Facebook Comments |
| `google-plus` | Google+ Comments  |
| `custom`      | Other             |

Then add `comments: true` to each document you want comments visible on.

Instead of adding YAML Front Matter to each document, apply as a default in `_config.yml`. To enable comments for all posts:

```yaml
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      comments: true
```

If you add `comments: false` to a post's YAML Front Matter it will override the default and disable comments for just that post.

##### Disqus

To use Disqus you'll need to create an account and get a [shortname](https://help.disqus.com/customer/portal/articles/466208-what-s-a-shortname-). Once you have one update `_config.yml` to:

```yaml
comments:
  provider: "disqus"
  disqus:
    shortname: "your-disqus-shortname"
```

##### Facebook Comments

To enable Facebook Comments choose how many comments you'd like visible per post and the color scheme of the widget.

```yaml
comments:
  provider               : "facebook"
  facebook:
    appid                : # optional
    num_posts            : # 5 (default)
    colorscheme          : # "light" (default), "dark"
```

##### Other Comment Providers

To use another provider not included with the theme set `provider: "custom"` then add their embed code to `_includes/comments-providers/custom.html`.

### SEO, Social Sharing, and Analytics Settings

All optional, but a good idea to take the time setting up to activate various tools for optimizing your site for search engines and link sharing.

#### Google Search Console

Formerly known as [Google Webmaster Tools](https://www.google.com/webmasters/tools/), add your [verification code](https://support.google.com/analytics/answer/1142414?hl=en) like so: `google_site_verification: "yourVerificationCode"`.

**Note:** You likely won't have to do this if you verify site ownership through **Google Analytics** instead.
{: .notice--warning}

#### Bing Webmaster Tools

There are several ways to [verify site ownership](https://www.bing.com/webmaster/help/how-to-verify-ownership-of-your-site-afcfefc6) --- the easiest adding an authentication code to your config file.

Copy and paste the string inside of `content`:

```html
<meta name="msvalidate.01" content="0FC3FD70512616B052E755A56F8952D" />
```

Into `_config.yml`

```yaml
bing_site_verification: "0FC3FD70512616B052E755A56F8952D"
```

#### Alexa

To [claim your site](http://www.alexa.com/siteowners/claim) with Alexa add the provided verification ID `alexa_site_verification: "yourVerificationID"`.

#### Yandex

To verify site ownership copy and paste the string inside of `name`:

```html
<meta name='yandex-verification' content='2132801JL' />
```

Into `_config.yml`

```yaml
yandex_site_verification: "2132801JL"
```

#### Twitter Cards and Facebook Open Graph

To improve the appearance of links shared from your site to social networks like Twitter and Facebook be sure to configure the following.

##### Site Twitter Username

Twitter username for the site. For documents that have custom author Twitter accounts assigned in YAML Front Matter or a data file, those will be attributed as a **creator** in the Twitter Card. 

For example if my site's Twitter account is @mmistakes-theme I would add the following to `_config.yml`

```yaml
twitter:
  username: "mmistakes-theme"
```

And if I assign `@mmistakes` as an author account it will appear in the Twitter Card along with `@mmistakes-theme` attributed as a creator or document being shared.

**Note**: You need to [apply for Twitter Cards](https://dev.twitter.com/docs/cards) and validate they're working on your site before they will begin showing up.
{: .notice--warning}

##### Facebook Open Graph

If you have Facebook ID or publisher page add them:

```yaml
facebook:
  app_id:  # A Facebook app ID
  publisher:  # A Facebook page URL or ID of the publishing entity
```

While not part a part of Open Graph, you can also add your Facebook username for use in the sidebar and footer.

```yaml
facebook:
  username: "michaelrose"  # ref. page https://www.facebook.com/michaelrose
```

**ProTip:** To debug Open Graph data use [this tool](https://developers.facebook.com/tools/debug/og/object?q=https%3A%2F%2Fmademistakes.com) to test your pages. If content changes aren't reflected you will probably have to hit the **Fetch new scrape information** button to refresh.
{: .notice--info}

##### Open Graph Default Image

For documents who don't have a `header.image` assigned in their YAML Front Matter, `site.og_image` will be used as a fallback. Your logo, icon, or avatar are something else that is meaningful. Just make sure it is place in the `/images/` folder, a minimum size of 120px by 120px, and less than 1MB in file size. *The image will be cropped to a square on all platforms.*

```yaml
og_image: "site-logo.png"
```

<figure>
  <img src="{{ base_path }}/images/mm-twitter-card-summary-image.jpg" alt="Twitter Card summary example">
  <figcaption>Example of a image placed in a Summary Card.</figcaption>
</figure>

Documents who have a `header.image` assigned in their YAML Front Matter will appear like this when shared on Twitter and Facebook.

<figure>
  <img src="{{ base_path }}/images/mm-twitter-card-summary-large.jpg" alt="page shared on Twitter">
  <figcaption>Shared page on Twitter with header image assigned.</figcaption>
</figure>

<figure>
  <img src="{{ base_path}}/images/facebook-share-example.jpg" alt="page shared on Facebook">
  <figcaption>Shared page on Facebook with header image assigned.</figcaption>
</figure>

##### Include your social profile in search results

Use markup on your official website to add your [social profile information](https://developers.google.com/structured-data/customize/social-profiles#adding_structured_markup_to_your_site) to the Google Knowledge panel in some searches. Knowledge panels can prominently display your social profile information.

```yaml
social:
  type :  # Person or Organization (defaults to Person)
  name :  # If the user or organization name differs from the site's name
  links:
    - "https://twitter.com/yourTwitter"
    - "https://facebook.com/yourFacebook"
    - "https://instagram.com/yourProfile"
    - "https://www.linkedin.com/in/yourprofile"
    - "https://plus.google.com/your_profile"
```

#### Analytics

Analytics is disabled by default. To enable globally select one of the following:

|                    | Analytics Provider                                              |
| ------------------ | ------------------                                              |
| `google`           | [Google Standard Analytics](https://www.google.com/analytics/)  |
| `google-universal` | [Google Universal Analytics](https://www.google.com/analytics/) |
| `custom`           | Other analytics providers                                       |

For Google Analytics add your Tracking Code:

```yaml
analytics:
  provider: "google-universal"
    tracking_id: "UA-1234567-8"
```

To use another provider not included with the theme set `provider: "custom"` then add their embed code to `_includes/analytics-providers/custom.html`.

## Site Author

Used as the defaults for defining what appears in the author sidebar.

![author sidebar example]({{ base_path }}/images/mm-author-sidebar-example.jpg)

**Note:** For sites with multiple authors these values can be overridden post by post with custom YAML Front Matter and a data file. For more information on how that see below.
{: .notice--info}

```yaml
author:
  name   : "Your Name"
  avatar : "bio-photo.jpg"  # placed in /images/
  bio    : "My awesome biography constrained to a sentence or goes here."
  email  : # optional
  uri    : "http://your-site.com"
```

Social media links are all optional, include the ones you want visible. In most cases you just need to add the username. If you're unsure double check `_includes/author-profile.html`.

## Reading Files

Nothing out of the ordinary here. `include` and `exclude` may be the only things you need to alter.

## Conversion and Markdown Processing

Again nothing out of the ordinary here as the theme adheres to the defaults used by GitHub Pages. [**Kramdown**](http://kramdown.gettalong.org/) for Markdown conversion, [**Rouge**](http://rouge.jneen.net/) syntax highlighting, and incremental building disabled.

## Front Matter Defaults

To save yourself time setting [Front Matter Defaults](https://jekyllrb.com/docs/configuration/#front-matter-defaults) for posts, pages, and collections is the way to go. Sure you can assign layouts and toggle settings like **reading time**, **comments**, and **social sharing** in each file, but that's not ideal.

Using the `default` key in `_config.yml` you could set the layout and enable author profiles, reading time, comments, social sharing, and related posts for all posts.

```yaml
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true
```

Pages Front Matter defaults can be scoped like this:

```yaml
defaults:
  # _pages
  - scope:
      path: ""
      type: pages
    values:
      layout: single
```

And collections like this:

```yaml
defaults:
  # _foo
  - scope:
      path: ""
      type: foo
    values:
      layout: single
```

And of course any default value can be overridden by settings in a post, page, or collection file. All you need to do is specify the settings in the YAML Front Matter. For more examples be sure to check out the demo site's [`_config.yml`]({{ site.gh_repo }}/blob/gh-pages/_config.yml).

## Outputting

The default permalink style used by Minimal Mistakes is `permalink: /:categories/:title/`. If you have a post named `2016-01-01-my-post.md` with `categories: foo` in the YAML Front Matter, Jekyll will generate `/foo/my-post/index.html`.

**Note:** If you plan on enabling breadcrumb links --- including category names in permalinks is a big part of how those are created.
{: .notice--warning}

### Paginate

If [using pagination](https://github.com/jekyll/jekyll-paginate) on the homepage you can change the amount of posts shown with:

```yaml
paginate: 5
```

You'll also need to include some modified Liquid to properly use the paginator, which you can find in the **Layouts** section under [Home Page]({{ base_path }}/docs/layouts/#home-page).

### Timezone

Set the time zone for site generation. This sets the TZ environment variable, which Ruby uses to handle time and date creation and manipulation. Any entry from the [IANA Time Zone Database](http://en.wikipedia.org/wiki/List_of_tz_database_time_zones) is valid. The default is the local time zone, as set by your operating system.

```yaml
timezone: America/New_York
```

## Plugins

When hosting with GitHub Pages a small [set of gems](https://pages.github.com/versions/) have been whitelisted for use. The theme uses a few of them which can be found under `gems`. Additional settings and configurations are documented in the links below.

| Plugin                             | Description |
| ------                             | ----------- |
| [jekyll-paginate][jekyll-paginate] | Pagination Generator for Jekyll. |
| [jekyll-sitemap][jekyll-sitemap] |Jekyll plugin to silently generate a sitemaps.org compliant sitemap for your Jekyll site. |
| [jekyll-gist][jekyll-gist] | Liquid tag for displaying GitHub Gists in Jekyll sites. |
| [jekyll-feed][jekyll-feed] | A Jekyll plugin to generate an Atom (RSS-like) feed of your Jekyll posts. |
| [jemoji][jemoji] | GitHub-flavored emoji plugin for Jekyll. |

[jekyll-paginate]: https://github.com/jekyll/jekyll-paginate
[jekyll-sitemap]: https://github.com/jekyll/jekyll-sitemap
[jekyll-gist]: https://github.com/jekyll/jekyll-gist
[jekyll-feed]: https://github.com/jekyll/jekyll-feed
[jemoji]: https://github.com/jekyll/jemoji

If you're hosting elsewhere then you don't really have to worry about that and are free to include whatever [Jekyll plugins](https://jekyllrb.com/docs/plugins/) you desire.

## Archive Settings

Minimal Mistakes ships with support for taxonomy (category and tag) pages. GitHub Pages hosted sites get strip down Liquid only approach while those hosting elsewhere can use plugins like [**jekyll-archives**][jekyll-archives] to generate these pages automatically.

[jekyll-archives]: https://github.com/jekyll/jekyll-archives

The default `type` is set to use Liquid.

```yaml
categories:
  type: liquid
  path: /categories/
tags:
  type: liquid
  path: /tags/
```

Which would create category and tag links in the breadcrumbs and page meta like: `/categories/#foo` and `/tags/#foo`.

**Note:** for these links to resolve category and tag index pages need to exist at [`/categories/index.html`]({{ site.gh_repo }}/blob/gh-pages/_pages/category-archive.html) and [`/tags/index.html`]({{ site.gh_repo }}/blob/gh-pages/_pages/tag-archive.html). Examples with the necessary Liquid code can be taken from the demo site.
{: .notice--warning}

If you have the luxury of using Jekyll Plugins then [**jekyll-archives**][jekyll-archives] will make your life much easier as category and tag pages are created for you.

Change `type` to `jekyll-archives` and apply the following [configurations](https://github.com/jekyll/jekyll-archives/blob/master/docs/configuration.md):

```yaml
categories:
  type: jekyll-archives
  path: /categories/
tags:
  type: jekyll-archives
  path: /tags/
jekyll-archives:
  enabled:
    - categories
    - tags
  layouts:
    category: archive-taxonomy
    tag: archive-taxonomy
  permalinks:
    category: /categories/:name/
    tag: /tags/:name/
```

**Note:** The `archive-taxonomy` layout used by jekyll-archives is provided with the theme and can be found in the `_layouts` folder.
{: .notice--info}

## HTML Compression

If you care at all about performance (and really who doesn't) compressing the HTML files generated by Jekyll is a good thing to do.

Now if you're hosting with GitHub Pages there aren't many options afforded to you unless commit compiled files to your repo instead. Thankfully there is some Liquid wizardry you can use to strip whitespace and comments.

There's a variety of configurations and cavets to using the `compress` layout, so be sure to read through the [documentation](http://jch.penibelst.de/) if you decide to make changes to anything, but here's the settings I've found work well:

```yaml
compress_html:
  clippings: all
  ignore:
    envs: development  # disable compression in dev environment
```

**Caution:** Inline JavaScript comments can cause problems with `compress.html`, so be sure to `/* comment this way */` and avoid `// these sorts of comments`.