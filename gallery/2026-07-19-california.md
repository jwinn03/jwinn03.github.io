---
layout: post
date: 2026-07-19
title: California 2026
---

With some more pictures of New York taken this year.

{% for file in site.static_files %}
{% if file.path contains '/assets/images/california' %}
![{{ file.name }}]({{ file.path }})
{% endif %}
{% endfor %}

