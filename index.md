---
header: default
layout: default
title: williamfgc's Posts
---

# {{ page.title }}

## [About Me]({{ "/about.html" }})

<ul class="post-list">
  {% for post in site.posts %}
    
      <h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        <time datetime="{{ post.date | date: "%Y-%m-%d" }}"><small>{{ post.date | date_to_long_string }}</small></time>
      </h2>
    
  {% endfor %}
</ul>


