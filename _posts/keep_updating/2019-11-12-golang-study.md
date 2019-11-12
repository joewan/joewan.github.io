---
layout: post
title: golang学习(golangchina.com)
categories: golang
tags: golang
author: 东邪
description: golang学习
---

### 在阅读golang源码的时候，发现一些奇怪的语法看不懂，其中一个就是golang中的“...”。
“...”其实是golang的一种语法糖，它的第一个用法是用于函数有多个不定的参数的情况，可以接受多个不确定数量的参数。
第二个用法是slice可以被打散进行传递。

{% highlight golang %}
func testFunc(args ...string) {
	for _, v := args {
		fmt.Println(v)
	}
}
{% endhighlight %}
