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

### FOpenGLDynamicRHI
想了解Unreal的OpenGL相关的渲染知识，需要熟悉FOpenGLDynamicRHI的函数、RHI模块、RenderCore以及ShaderCore模块。OpenGLDynamicRHI的函数体分散在较多CPP文件里面（一个头文件对应多个CPP文件）：

#### OpenGLDevice.cpp

+ FOpenGLDynamicRHI::Init()


##### OpenGLDrv.cpp

#### OpenGLCommands.cpp

#### OpenGLTexture.cpp

##### OpenGLViewport.cpp

##### OpenGLVertexBuffer.cpp

##### OpenGLIndexBuffer.cpp

#### OpenGLState.cpp

#### OpenGLQuery.cpp

#### OpenGLUAV.cpp


