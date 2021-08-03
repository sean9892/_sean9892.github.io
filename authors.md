---
layout: page
title: Authors
permalink: /authors/
sitemap:
  priority: 0.7
---
{% assign modiauthors = site.authors %}
{% for author in modiauthors | sort:'title' %}
* [{{author.title}} ({{ author.name }})]({{ site.baseurl }}/authors/{{ author.name }})
{% endfor %}
