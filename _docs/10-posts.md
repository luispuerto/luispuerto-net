---
title: "Working with Posts"
permalink: /docs/posts/
excerpt:
sidebar:
  title: "v3.0"
  nav: docs
modified: 2016-04-13T15:54:02-04:00
---

{% include base_path %}

Posts are stored in the `_posts` directory and named according to the `YEAR-MONTH-DAY-title.MARKUP` format as per [the usual](https://jekyllrb.com/docs/posts/).

**Recommended Front Matter Defaults:**

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

Adding the above to `_config.yml` will assign the `single` layout and enable: *author profile*, *reading time*, *comments*, *social sharing links*, and *related posts*, for all posts.

**ProTip:** Remember to write unique `excerpt` descriptions for each post for improved SEO and archive listings.
{: .notice--info}