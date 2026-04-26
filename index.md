---
layout: default
title: Home
---


# [Welcome to my Technical Page](https://hatandspecs.github.io/natale_github_page/)

I am a new Ham Radio operator, as of December 2025. I am in the process of tinkering, making mistakes, and trying to rediscover a lot of information that experienced hams throughout the ages already know. I am doing this as a learning exercise and will attempt to keep decent documentation along the way.

---


# Technical Articles

| Date | Article Title |
| :--- | :--- |
{% for page in site.pages %}
  {% if page.category == "article" %}
| {{ page.date | date: "%b %d, %Y" }} | [{{ page.title }}]({{ page.url | relative_url }}) |
  {% endif %}
{% endfor %}