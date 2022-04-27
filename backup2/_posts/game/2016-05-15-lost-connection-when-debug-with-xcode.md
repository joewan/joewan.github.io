---
layout: post
title: 捉虫记——Lost connection when debug with xcode
categories: Game
tags: Game
author: 东邪
description: unreal engine 学习
---

苦逼的程序员总被一个个bug这折磨的死去活来。项目很快就要上付费公测了，依然没有找到这个问题的症结所在，记录一下解决这个问题的心路历程。

公司同事用iPhone体验游戏的时候，不停地报魔王关闪退（项目一直不在持续开发，也不能通过二分法看稳定是哪个版本引入的）。可恨的是，iOS版本刚升级过了，这个闪退一直都不上报，程序员看不到堆栈，就变成瞎子咯，眼前一片黑，亚历山大呀。

这个时候灵机一动，为什么不连着xcode打游戏呢。只要出一次，就能看到堆栈咯。于是就让测试连着mac体验游戏，基本能做到10分钟闪退一次。出乎意料的是，游戏闪退的时候，会显示“Lost Connection”，顿时感觉好绝望。在网上搜索了一下，发现了好多难兄难弟，心里好受了不少～～哈哈。

特征1:iOS 8.x闪退较多。

### StackOverflow
在StackOverflow上看到上面这篇文章：
[iOS app crashes, xcode says 'Lost connection to X's iPhone' when debugging](http://stackoverflow.com/questions/26020832/ios-app-crashes-xcode-says-lost-connection-to-xs-iphone-when-debugging)
问题可以基本定性：较短时间进行了大量的内存分配。

有2个思路：

+ 1.用instruments观察游戏内存分配申请；
+ 2.接管游戏的所有内存分配和释放（重载全局new）；
+ 3.模拟闪退，实现测试程序，单次在堆上分配小内存10万次；
