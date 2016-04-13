---
title: "Navigation"
permalink: /docs/navigation/
excerpt:
sidebar:
  title: "v3.0"
  nav: docs
modified: 2016-04-13T15:54:02-04:00
---

{% include base_path %}

## Masthead

The masthead links use a "priority plus" design pattern. Meaning, show as many navigation items that will fit horizontally with a toggle to reveal the rest.

To define these links add titles and URLs under the `main` key in `_data/navigation.yml`:

```yaml
main:
  - title: "Quick-Start Guide"
    url: /docs/quick-start-guide/

  - title: "Posts"
    url: /year-archive/

  - title: "Categories"
    url: /categories/

  - title: "Tags"
    url: /tags/

  - title: "Pages"
    url: /page-archive/

  - title: "Collections"
    url: /collection-archive/

  - title: "External Link"
    url: https://google.com
```

Which will give you a responsive masthead similar to this:

![priority plus masthead animation]({{ base_path }}/images/mm-priority-plus-masthead.gif)

**ProTip:** Put the most important links first so they're always visible and not hidden behind the **menu toggle**.
{: .notice--info}

## Breadcrumbs (Beta)

Enable breadcrumb links to help visitors better navigate deeply structure sites. Because of the fragile method of implementing them they don't always produce accurate links reliably. For best results:

1. Use a category based permalink structure e.g. `permalink: /:categories/:title/`
2. Manually create pages for each category or use a plugin like [jekyll-archives](https://github.com/jekyll/jekyll-archives) to auto-generate them. If these pages don't exist breadcrumb links to them will be broken.

![breadcrumb navigation example]({{ base_path }}/images/mm-breadcrumbs-example.jpg)

```yaml
breadcrumbs: true  # disabled by default
```

Breadcrumb start link text and separator character can both be changed in `_data/ui-text.yml`.

```yaml
breadcrumb_home_label : "Home"
breadcrumb_separator  : "/"
```

For breadcrumbs that resemble something like `Start > Blog > My Awesome Post` you'd apply these settings:

```yaml
breadcrumb_home_label : "Start"
breadcrumb_separator  : ">"
```