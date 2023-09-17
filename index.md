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
      {{ post.time | date: "%-d %B %Y"}}
    </li>
  {% endfor %}
</ul>
