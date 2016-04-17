---
layout: post
title: Unreal Engine 4 代码分析一
---

### Unreal Engine简介

### Unreal Engine的构建系统与依赖管理

### Unreal Engine模块

Unreal的源码目录包括四个部分：引擎源码（Runtime），编辑器（Editor），周边工具（Programs）以及Developer（？）。其中Runtime是整个引擎的基石，包含整个引擎的各个功能模块。

+ Core  
Runtime中最核心的就当属Core了，Core是我们熟悉、掌握Unreal的重中之重。Core包含了UObject、Async、串行化、Stats、、Containers、日志（Logging）、数学库、HAL和GenericPlatform、i18n等。数学库包含专门针对SSE、NEON、FPU的优化。Core给Unreal提供了C++标准缺失的反射特性，极大地提高了开发效率。
+ HAL(Hardware Abstact Layer)
+ RHI(Render Hardware Interface)

每个功能模块目录下面一般都包含Private和Public2个目录，这给各个模块提供了适当的可见性、透明性。
