---
title: Writing
permalink: /writing/
---

<ul class="listing">
  {% for post in site.posts %}
    <li>
      <span class="fine">{{ post.date | date: site.theme_config.date_format }}</span>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.blurb %}<br /><span class="fine">{{ post.blurb }}</span>{% endif %}
    </li>
  {% endfor %}
</ul>

<p class="fine"><a href="{{ '/feed.xml' | relative_url }}">rss</a></p>
