---
layout: post
date: 2025-08-24
title: Vienna
---

With some more pictures of New York taken this year.

{% for file in site.static_files %}
{% if file.path contains '/assets/images/california' %}
![{{ file.name }}]({{ file.path }})
{% endif %}
{% endfor %}

