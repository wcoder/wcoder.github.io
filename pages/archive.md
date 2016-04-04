---
layout: page
title: Архив
permalink: /archive/
---

[По тегам]({{% site.url %}}/tags/)

## Заметки ({% site.posts.size %})

{% for post in site.posts %}
- {{ post.date | date: "%d.%m.%Y" }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
