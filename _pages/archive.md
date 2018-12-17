---
title: "Archive"
layout: single
permalink: /archive/
author_profile: true
header:
  overlay_image: /assets/images/pages/header-archive.jpg
  caption: '*A virgin forest of White Pine* from [page 623](https://archive.org/stream/americanforests15natiuoft/#page/606/mode/1up) of "[American forests](https://archive.org/details/americanforests15natiuoft)". Source: [Flickr](https://www.flickr.com/photos/internetarchivebookimages/18146675355/).'
  overlay_filter: 0.3 
toc: true
toc_label: "Index"
toc_icon: "archive"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
---

<p style="margin: 0 0 0 0">
  <blockquote style="font-size: xx-large; margin: 0 0 0 0; font-weight: bold;">
  In that moment, it seemed like a good idea.
  </blockquote>
<p style="margin: 0 0 0 0; text-align: right; font-size: small; padding-right: 2em">
  - <a href="https://en.wikipedia.org/wiki/Ignacio_Escolar" style="text-decoration: none; color: black">Ignacio Escolar</a></p>
</p>

## By Year & Month

<ul class="taxonomy__index">
  {% assign postsInYear = site.posts | group_by_exp: 'post', 'post.date | date: "%Y"' %}
  {% for year in postsInYear %}
    <li>
      <a href="/archive/{{ year.name }}">
        <strong>{{ year.name }}</strong> <span class="taxonomy__count">{{ year.items | size }}</span>
      </a>
      <ul class="subtaxonomy__index">
      {% assign postsInMonth = year.items | group_by_exp: 'post', 'post.date | date: "%B %m"' %}
         {% for month in postsInMonth %}
         <li>
      <a href="/archive/{{ year.name | slugify  }}/{{ month.name | slice: -2, 2 | slugify }}">
        <strong>{{ month.name | truncatewords: 1, "" }}</strong> <span class="taxonomy__count">{{ month.items | size }}</span>
      </a>
      </li>
  {% endfor %}
      </ul>
    </li>
  {% endfor %}
</ul>

## Categories 

### By Size

{% assign categories_max = 0 %}
{% for category in site.categories %}
  {% if category[1].size > categories_max %}
    {% assign categories_max = category[1].size %}
  {% endif %}
{% endfor %}

<ul class="taxonomy__index">
  {% for i in (1..categories_max) reversed %}
    {% for category in site.categories %}
      {% if category[1].size == i %}
        <li>
          <a href="/archive/categories/{{ category[0] | slugify }}">
            <strong>{{ category[0] }}</strong> <span class="taxonomy__count">{{ i }}</span>
          </a>
        </li>
      {% endif %}
    {% endfor %}
  {% endfor %}
</ul>

### Alphabetically

{% assign categories = "" | split:"" %}
{% for c in site.categories %}
  {% assign categories = categories | push: c[0] %}
{% endfor %}

<ul class="taxonomy__index">
  {% assign sorted_categories = categories | sort_natural %}
    {% for c in sorted_categories %}
      <li>
        <a href="/archive/categories/{{ c | slugify }}">
          <strong>{{ c }}</strong> <span class="taxonomy__count">{{ site.categories[c].size }}</span>
        </a>
      </li>
  {% endfor %}
</ul>

## Tags

<p class="tag__cloud exclude-from-search" style="text-align: center;">
  {% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
  {% assign tags = site_tags | split:',' | sort %}
  {% include tagcloud.html %}
  </p>

### By Size

{% assign tags_max = 0 %}
{% for tag in site.tags %}
  {% if tag[1].size > tags_max %}
    {% assign tags_max = tag[1].size %}
  {% endif %}
{% endfor %}

<ul class="taxonomy__index">
  {% for i in (1..tags_max) reversed %}
    {% for tag in site.tags %}
      {% if tag[1].size == i %}
        <li>
          <a href="/archive/tags/{{ tag[0] | slugify }}">
            <strong>{{ tag[0] }}</strong> <span class="taxonomy__count">{{ i }}</span>
          </a>
        </li>
      {% endif %}
    {% endfor %}
  {% endfor %}
</ul>

### Alphabetically

{% assign tags = "" | split:"" %}
{% for t in site.tags %}
  {% assign tags = tags | push: t[0] %}
{% endfor %}

<ul class="taxonomy__index">
  {% assign sorted_tags = tags | sort_natural %}
    {% for t in sorted_tags %}
      <li>
        <a href="/archive/tags/{{ t | slugify }}">
          <strong>{{ t }}</strong> <span class="taxonomy__count">{{ site.tags[t].size }}</span>
        </a>
      </li>
  {% endfor %}
</ul>

