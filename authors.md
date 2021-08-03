---
layout: page
title: Authors
permalink: /authors/
sitemap:
  priority: 0.7
---
<script>
const modifiedsiteauthors = site.authors;
arr.sort(function(a,b){
	return a.title<b.title;
});
{% for author in modifiedsiteauthors %}
* [{{author.title}} ({{ author.name }})]({{ site.baseurl }}/authors/{{ author.name }})
{% endfor %}
</script>