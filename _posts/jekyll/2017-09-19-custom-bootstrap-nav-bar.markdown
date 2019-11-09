---
layout: post
title:  "更改Bootstrap导航栏颜色"
categories: Programmer
tags: Bootstrap
author: 西夏
description: 新增自定义CSS样式，然后修改导航栏，引用该样式。
---

###  1，修改CSS

在项目CSS中添加如下代码，修改其中的**`background-color`**的颜色值。

{% highlight css %}
.navbar-custom {
    background-color:#28323C;
    color:#ffffff;
    border-radius:0;
}

.navbar-custom .navbar-nav > li > a {
    color:#fff;
}

.navbar-custom .navbar-nav > .active > a {
    color: #ffffff;
    background-color:transparent;
}

.navbar-custom .navbar-nav > li > a:hover,
.navbar-custom .navbar-nav > li > a:focus,
.navbar-custom .navbar-nav > .active > a:hover,
.navbar-custom .navbar-nav > .active > a:focus,
.navbar-custom .navbar-nav > .open >a {
    text-decoration: none;
    background-color: #e74c3c;
}

.navbar-custom .navbar-brand {
    color:#eeeeee;
}
.navbar-custom .navbar-toggle {
    background-color:#eeeeee;
}
.navbar-custom .icon-bar {
    background-color:#28323C;
}
{% endhighlight %}

<br/>
###  2，修改html样式引用

然后在导航栏中引用刚才添加的样式。

{% highlight html %}
<div class="container-fluid">
    <div class="row-fluid">
        <div class="navbar navbar-custom navbar-fixed-top" role="navigation">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="{{ site.baseurl }}/">{{ site.name }}</a>
            </div>
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav">
                    <li class="active"><a href="{{ site.baseurl }}/">首页</a></li>
                    <li class="active"><a href="{{ site.baseurl }}/archive.html">归档</a></li>
                    <li class="active"><a href="{{ site.baseurl }}/categories.html">分类</a></li>
                    <li class="active"><a href="{{ site.baseurl }}/tags.html">标签</a></li>
                    <li class="active"><a href="{{ site.baseurl }}/about.html">关于</a></li>
                </ul>
            </div>
        </div>
    </div>
</div>
{% endhighlight %}

<!-- 后面是文章参考资料 -->
<br/>
### 参考资料：

1，[stackoverflow-change-navbar-color-in-twitter-bootstrap-3][stackoverflow]

<!-- 文章插图和超链接 -->
[stackoverflow]: https://stackoverflow.com/questions/18529274/change-navbar-color-in-twitter-bootstrap-3
