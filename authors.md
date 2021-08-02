---
layout: page
title: Authors
permalink: /authors/
sitemap:
  priority: 0.7
---
{% for author in site.authors %}
* [author.title ({{ author.name }})]({{ site.baseurl }}/authors/{{ author.name }})
{% endfor %}

