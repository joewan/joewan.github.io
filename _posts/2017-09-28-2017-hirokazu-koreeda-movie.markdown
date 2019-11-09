---
layout: movie
title:  "是枝裕和导演电影四部"
categories: Movie
tags: Movie
author: 西夏
description: 四部是枝裕和导演的电影，四部很暖心的电影。
---

{% for movie in site.data.movie._2017_hirokazu_koreeda %}
<div class="subject clearfix">
    <div class="mainpic">
        <img src="{{ movie.post_photo }}"/>
    </div>
    <div class="info">
        <span class='title'>{{ movie.title }}<small>({{ movie.year }})</small></span><br/>
        <span class='pl'>导演</span>: {{ movie.director }}<br/>
        <span class='pl'>主演</span>: {{ movie.actors }}<br/>
        <span class="pl">类型:</span> {{ movie.type }}<br/>
        <span class="pl">制片国家/地区:</span> {{ movie.area }}<br/>
        <span class="pl">片长:</span> {{ movie.duration }}<br/>
        <span class="pl">豆瓣:</span> <a href="{{ movie.douban_url }}" target="_blank">{{ movie.rate }}分({{ movie.people }}人评价)</a><br>
    </div>
    <div class="content">
        <span class='title' style="color:transparent;"></span><br/>
        <span class='pl'>西夏说</span>: <br/>
        <span>{{ movie.comments }}</span>
    </div>
</div>
{% unless forloop.last %}
---
{% endunless %}
{% endfor %}


<!-- 后面是文章参考资料 -->
<br/>
### 参考资料：

1，[豆瓣网-电影详情页-布局][douban]

<!-- 文章插图和超链接 -->
[douban]: https://movie.douban.com/
