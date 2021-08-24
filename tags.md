---
layout: page
title: Tags
permalink: /tags/
sitemap:
  priority: 0.7
---

**태그는 `[ctrl + f]` 후에 영어로 검색!**  
**띄어쓰기는 `earth-science` 이렇게!**

{% for tag in site.tags %}
* [{{ tag.name }}]({{ site.baseurl }}/tags/{{ tag.name }})
{% endfor %}

