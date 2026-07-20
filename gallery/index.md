---
layout: page
title: Photos
---

Photos I have taken.

{% assign galleries = site.pages | where_exp: "p", "p.url contains '/gallery/'" | where: "layout", "post" | sort: "date" | reverse %}
{%- for gallery in galleries %}
- [{{ gallery.title }}]({{ gallery.url | relative_url }})
{%- endfor %}
