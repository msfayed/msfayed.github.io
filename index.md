---
layout: default
title: Mohammed Fayed Blog
description: A playground for my experiments
---

# My Posts
<ul>
  {% for post in site.posts %}
  <li>
    <h4><span class="date">{{ post.date | date: "%Y-%m-%d "}}</span> <a href="{{ post.url }}">{{ post.title }}</a></h4>
    
  </li>
  {% endfor %}
</ul>

## [Tags](tags.md)

