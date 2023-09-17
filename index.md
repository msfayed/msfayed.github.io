---
layout: default
title: Mohammed Fayed Blog
description: A playground for my experiments
---

# My Posts
<ul>
  {% for post in site.posts %}
  <li>
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    {{ post.date | date: "%-d %B %Y"}}
  </li>
  {% endfor %}
</ul>

## Tags
{% for tag in site.tags %}
<h3>{{ tag[0] }}</h3>
<ul>
  {% for post in tag[1] %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
{% endfor %}
