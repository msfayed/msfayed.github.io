---
layout: default
title: Mohammed Fayed Blog
description: A playground for my experiments
---

# My Posts
<ul>
  {% for post in site.posts %}
  <li>
    <h4><span class="date">{{ post.date | date: "%-d %B %Y"}}</span> <a href="{{ post.url }}">{{ post.title }}</a></h4>
    
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
