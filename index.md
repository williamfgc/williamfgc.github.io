---
header: default
layout: default
title: williamfgc's Posts
---

# {{ page.title }}

## [About Me]({{ "/about.html" }})

{% for tag in site.tags %}
  <h1>{{ tag[0] }}</h1>
  <ul>
    {% for post in tag[1] %}
      <h2>
        <a href="{{ post.url }}">
          {{ post.title }}
        </a>
        <time datetime="{{ post.date | date: "%Y-%m-%d" }}">
          <small>{{ post.date | date_to_long_string }}</small>
        </time>
      </h2>
      
    {% endfor %}
  </ul>
{% endfor %}

