---
layout: home
title: "Mehmet Aydın - Kişisel Blog"
---

{% for post in paginator.posts %}
  <a href="{{ root_url }}{{ post.url }}"><h3 class="entry-title">{{ post.title }}</h3></a>
{% endfor %}
