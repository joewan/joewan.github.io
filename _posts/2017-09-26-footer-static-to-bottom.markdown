---
layout: post
title:  "将footer固定到页面底部"
categories: Programmer
tags: Html
author: 西夏
description: 将footer固定到页面底部，页面撑不满屏幕时，固定到屏幕底部。
---

###  1，代码如下

{% highlight html %}
<!doctype html>
<html>
<head>
<meta charset="utf-8">
    <title>不足一屏时，页面底部位于浏览器底部</title>
    <style type="text/css">
    * { 
        margin:0; 
        padding:0;
    }

    html, body {
        height:100%;
    }

    .wrap { 
        position:relative;
        min-height:100%;
    }

    .header {
        height:50px;
        background-color:#28323C;
    }

    .content {
        padding-bottom:50px;
        /*height:2000px; 做超过一屏时使用*/
    }

    .footer {
        position:absolute;
        bottom:0;
        left:0;
        width:100%;     /*绝对定位使后宽度100%*/
        min-height:50px; 
        background-color:#28323C;
    }
    </style>
</head>

<body>
    <div class="wrap">
        <div class="header">header</div>
        <div class="content"></div>
        <div class="footer">footer</div>
    </div>
</body>
</html>
{% endhighlight %}

<br/>
###  2，说明

原始[参考资料][link1]中的footer设置的是**`height:50px`**，这里更改为了**`min-height:50px`**，避免了有可能的footer内部子内容高度大于footer高度而出现的bug。
**本博客的footer就是使用这一方法实现的，有兴趣的可直接查看源码**。

<!-- 后面是文章参考资料 -->
<br/>
### 参考资料：

1，[舞动青春-不足一屏时，页面底部位于浏览器底部（用CSS解决）][link1]

<!-- 文章插图和超链接 -->
[link1]: http://www1.qdfuns.com/blog-5400161-5398103.html
