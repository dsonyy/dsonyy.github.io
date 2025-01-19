---
layout: default
title: Blog
permalink: /blog
---

# Recents

{% for post in site.posts limit: 3 %}
<article class="post-preview">
  <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
  <div class="post-meta">
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time>
  </div>
  <div class="post-excerpt">
    {{ post.excerpt }}
  </div>
</article>
{% endfor %}

# Posts

<div class="posts-list">
{% for post in site.posts %}
  <div class="post-item">
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </div>
{% endfor %}
</div>
