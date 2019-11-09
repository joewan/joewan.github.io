---
layout: post
title:  "免费建站程序搜藏"
categories: Programmer
tags: Nav
author: 西夏
description: 收集整理的免费建站程序，包括BBS、CMS、WiKi等各种类型建站系统。
---

{% for site_builder in site.data.nav.sitebuilder %}
<h3>{{ site_builder.type }}</h3>
{% for _site in site_builder.sites %}
<ul>
    <li><a href="{{ _site.url}}" target="_blank">{{ _site.title }}</a> 
    {% if _site.description %}
    - <small>{{ _site.description }}</small>
    {% endif %}
    </li>
</ul>
{% endfor %}
{% endfor %}


<!-- 后面是文章参考资料 -->
<br/>
<hr/>
### 参考资料：

1，[百度经验-国内外免费PHP开源建站程序一览（最全）][baidu-jingyan]

<!-- 文章插图和超链接 -->
[baidu-jingyan]: http://jingyan.baidu.com/article/49711c6156fb68fa441b7c2b.html