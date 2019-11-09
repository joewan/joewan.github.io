---
layout: post
title:  "程序猿网址导航"
categories: Programmer
tags: Nav
author: 西夏
description: 收集整理的常用程序猿站点。
---

{% for programmer in site.data.nav.programmer %}
<h2>{{ programmer.type }}</h2>
{% for _site in programmer.sites %}
<ul>
    <li><a href="{{ _site.url}}" target="_blank">{{ _site.title }}</a> 
    {% if _site.description %}
    - <small>{{ _site.description }}</small>
    {% endif %}
    </li>
</ul>
{% endfor %}
{% endfor %}