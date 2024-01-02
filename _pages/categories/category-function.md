---
title: "자주 쓰는 함수"
layout: archive
permalink: categories/function
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories.function %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}