---
title: "Git"
layout: archive
permalink: categories/git
author_profile: true
sidebar:
    nav: "sidebar-category"
---


{% assign posts = site.categories.Git %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}