---
layout: default
title: Welcome to my Technical Page
---


I am a new Ham Radio operator, as of December 2025. I am in the process of tinkering, making mistakes, and trying to rediscover a lot of information that experienced hams throughout the ages already know. I am doing this as a learning exercise and will attempt to keep decent documentation along the way.

---


# Technical Articles

{% assign sorted_pages = site.pages | sort: "date" | reversed %}

{% for page in sorted_pages %}
  {% if page.category == "article" %}
* {% if page.date %}**{{ page.date | date: "%b %d, %Y" }}**: {% endif %}[{{ page.title }}]({{ page.url | relative_url }})
  {% endif %}
{% endfor %}