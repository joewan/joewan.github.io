---
layout: post
title: Unreal Engine 4 代码分析——OpenGLDrv模块
---

OpenGLDrv实质上是基于OpenGL的DynamicRHIModule。
OpenGLDrv.h里面，头文件包含顺序：从底层逐渐到上层，最后是包含本模块的头文件。

OpenGLDrv Profiling的实现：

+ FOpenGLBufferedGPUTiming：继承自FRenderResource
+ FOpenGLDisjointTimeStampQuery：继承自FRenderResource
+ FOpenGLEventNode：A single perf event node, which tracks information about a appBeginDrawEvent/appEndDrawEvent range。
+ FOpenGLEventNodeFrame
+ FOpenGLGPUProfiler

OpenGLDrv渲染相关的实现：

+ FOpenGLDynamicRHI
+ FOpenGLDynamicRHIModule


OpenGLDrv的主要头文件：

+ OpenGLDrv.h
+ OpenGL.h
+ OpenGLState.h
+ OpenGLUtil.h
+ OpenGLResource.h
+ OpenGLShaderResource.h
+ OpenGL3.h OpenGL4.h OpenGLES2.h OpenGL31.h

FOpenGLDynamicRHI函数：

+ FOpenGLDynamicRHI::Init()：OpenGLDevice.cpp