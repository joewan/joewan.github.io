---
layout: post
title: cocos creator插件开发
categories: study
tags: nodejs
author: 东邪
description: cocos creator插件开发
---

### 在给cocos creator开发插件的过程中，需要遍历目录，获取脚本文件的所在目录，cocos creator的错误日志不显示，写代码很不方便。于是想到直接用node运行代码，代码的大部分逻辑先实现完成，然后在cocos creator中慢慢调试。

首先就是需要遍历prefabs的目录，列出需要遍历的prefab文件。

现在需要配置prefabs的目录，插件需要运行在不同的环境，因而需要配置相对路径。可以相对工程目录来配置，Editor.projectPath = Editor.Project.path。用node执行JS脚本时，没办法获取Editor.projectPath，于是就打算获取js脚本所在的目录，相对这个目录去写代码。不过这个好像有点多此一举，插件可能会安装在全局目录，也可能安装在项目目录。因为在node中执行脚本只会在开会环境中用到，因此我们在开发脚本过程中，prefab的资源目录可以写死。下面是采用path获取js脚本所在目录的代码实现：

{% highlight javascript %}
const path = require("path");
console.log(__dirname);
console.log(". = %s", path.resolve("."));
console.log("__dirname = %s", path.resolve(__dirname));
{% endhighlight %}


