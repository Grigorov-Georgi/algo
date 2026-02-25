---
layout: default
title: Algorithms
permalink: /algorithms-list/
---

# Algorithms

{% assign items = site.algorithms | sort: "title" %}
{% if items.size > 0 %}
{% for item in items %}
- [{{ item.title }}]({{ item.url | relative_url }}){% if item.difficulty %} - {{ item.difficulty }}{% endif %}
{% endfor %}
{% else %}
No algorithm notes yet. Add one in `_algorithms/`.
{% endif %}
