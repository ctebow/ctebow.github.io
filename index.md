---
title: Caden Tebow
hide_title: true
---

Short intro paragraph — one or two sentences on who you are and what you
work on. This is the first thing a visitor reads, so keep it concrete.

## Selected projects

{% include project_list.html limit=3 %}

[all projects &rarr;]({{ '/projects/' | relative_url }})

## Recent writing

<ul class="listing">
  {% for post in site.posts limit: 3 %}
    <li>
      <span class="fine">{{ post.date | date: site.theme_config.date_format }}</span>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
