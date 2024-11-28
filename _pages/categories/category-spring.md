---
title: "Spring"
layout: archive
permalink: categories/spring
author_profile: true
sidebar:
    nav: "sidebar-category"
---


{% assign posts = site.categories.Spring %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}