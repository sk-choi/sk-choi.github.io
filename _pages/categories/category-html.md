---
title: "HTML"
layout: archive
permalink: categories/html
author_profile: true
sidebar:
    nav: "sidebar-category"
---


{% assign posts = site.categories.HTML %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
