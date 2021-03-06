---
layout: post
title: Unreal Engine 4 代码分析一
categories: Game
tags: Game
author: 东邪
description: unreal engine 学习
---

### Unreal Engine简介

### Unreal Engine的构建系统与依赖管理

### Unreal Engine模块

熟悉系统之前先了解一下引擎目录：

+ Binaries
+ Build
+ Config
+ Documentation 阅读文档是理解Unreal的重要途径。需要相当关注UnrealBuildSystem文档，对细分和理解Unreal依赖管理有启发意义。
+ Extras
+ Plugins
+ Programs
+ Shaders
+ Source 代码目录

Unreal的源码目录包括四个部分：引擎源码（Runtime），编辑器（Editor），周边工具（Programs）以及Developer（？）。其中Runtime是整个引擎的基石，包含整个引擎的各个功能模块。

+ Core
Runtime中最核心的就当属Core了，Core是我们熟悉、掌握Unreal的重中之重。Core包含了UObject、Async、串行化、Stats、、Containers、日志（Logging）、数学库、HAL和GenericPlatform、i18n等。数学库包含专门针对SSE、NEON、FPU的优化。Core给Unreal提供了C++标准缺失的反射特性，极大地提高了开发效率。
+ HAL(Hardware Abstact Layer)
+ RHI(Render Hardware Interface)

每个功能模块目录下面一般都包含Private和Public2个目录，这给各个模块提供了适当的可见性、透明性。

阅读Unreal源码主要是想了解先进商业引擎的方方面面，对游戏引擎有更全面的了解。正好借这个契机，了解计算机图形学渲染实现。Unreal的渲染实现是Renderer模块，Renderer依赖RenderCore模块、ShaderCore模块、UtilityShaders模块以及RHI模块，RHI是提供了显卡硬件驱动的抽象接口，显卡驱动包括OpenGL实现（OpenGLDrv、OpenGLES）、DirectX实现（DX11RHI、DX12RHI）。Renderer模块以RHI为基础实现了各种高级效果，例如大气、雾、全局光照、贴花渲染、阴影、环境反射、Deferred Shading等。


熟悉系统之前熟悉系统的接口定义。

+ ModuleManager与IModuleInterface Unreal在模块化方面做的很好，那么Unreal 4中有多少模块呢？搜索“public IModuleInterface”即可，我搜索的结果：298 matches across 296 files。庖丁解牛，逐渐去分析这298个模块，滴水穿石，终究会有一天我们会把这个大东西搞透彻、弄明白。后续blog的文章标题不愁了——Unreal Engine分析之XX模块。
+ IDynamicRHIModule 在源码中搜索“public IDynamicRHIModule”，有6处实现：FEmptyDynamicRHIModule、FMetalDynamicRHIModule、FNullDynamicRHIModule、FOpenGLDynamicRHIModule、FD3D11DynamicRHIModule、FD3D12DynamicRHIModule，DynamicRHIModule与DynamicRHI一一对应。
+ FDynamicRHI 通过在源码中搜索“public FDynamicRHI”，发现6处DynamicRHI实现：FEmptyDynamicRHI、FMetalDynamicRHI FD3D12DynamicRHI、FD3D11DynamicRHI、FOpenGLDynamicRHI FNullDynamicRHI。问题来了：FEmptyDynamicRHI与FNullDynamicRHI什么区别呢？实质上NullDrv是一种IDynamicRHIModule的实现。
+ IRHICommandContext

### RHI(Render Hardware Interface)


计划从OpenGLDrv模块以及其基于OpenGLES的FOpenGLDynamicRHI来熟悉Unreal的渲染流程，熟悉商业引擎移动平台的渲染实现。

首先是DynamicRHI的跨平台创建，使用DynamicRHI的平台需要先实现PlatformCreateDynamicRHI接口：

	DynamicRHI.h
	/**
	 *	Each platform that utilizes dynamic RHIs should implement this function
	 *	Called to create the instance of the dynamic RHI.
	 */
	FDynamicRHI* PlatformCreateDynamicRHI();
	

OpenGLDrv模块依赖的模块：Core、CoreUObject、Engine、RHI、enderCore、ShaderCore以及UtilityShaders，模块内部私有包含ImageWrapper。


	FOpenGLDynamicRHIModule：
	class FOpenGLDynamicRHIModule : public IDynamicRHIModule
	
	FOpenGLDynamicRHIModule与OpenGLDrv进行关联，后续可以通过字符串“OpenGLDrv”来创建和访问该模块：
	IMPLEMENT_MODULE(FOpenGLDynamicRHIModule, OpenGLDrv);
	
	FOpenGLDynamicRHIModule使用：
	IDynamicRHIModule* DynamicRHIModule = &FModuleManager::LoadModuleChecked<IDynamicRHIModule>(TEXT("OpenGLDrv"));

