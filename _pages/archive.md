---
title: "Archive"
layout: single
permalink: /archive/
author_profile: true
header:
  overlay_image: https://torange.biz/photofx/1/8/light-sepia-toned-old-dark-frame-forest-road-autumn-1949.jpg
---

## By Categories by Size

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

## By Tags by Size

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

