---
layout: post
title: Unreal Engine 4 代码分析——OpenGLDrv模块
categories: Game
tags: Game
author: 东邪
description: unreal engine 学习
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
+ FOpenGLTextureFormat
+ FOpenGLRenderQuery

枚举类型：

+ EOpenGLCurrentContext


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

+ FOpenGLDynamicRHI::FOpenGLDynamicRHI()
+ FOpenGLDynamicRHI::Init()
+ PlatformInitOpenGL：OpenGLDrvPrivate.h
+ PlatformCreateOpenGLDevice：OpenGLDrvPrivate.h
+ PlatformOpenGLCurrentContext：OpenGLDrvPrivate.h
+ PlatformOpenGLContextValid：OpenGLDrvPrivate.h
+ PlatformGlGetError：OpenGLDrvPrivate.h
+ PlatformGetWindow：OpenGLDrvPrivate.h
+ PlatformRenderingContextSetup：OpenGLDrvPrivate.h
+ PlatformGetBackbufferDimensions：OpenGLDrvPrivate.h

构造函数内面使用文件静态函数InitRHICapabilitiesForGL。
将不同平台的OpenGL实现typedef为FOpenGL，例如：

+ typedef FAndroidES31OpenGL FOpenGL; FOpenGLES31、FOpenGLBase
+ typedef FAndroidOpenGL FOpenGL; FOpenGLES2、FOpenGLBase
+ typedef FHTML5OpenGL FOpenGL; FOpenGLES2、OpenGLBase
+ typedef FIOSOpenGL FOpenGL; FOpenGLES2、FOpenGLBase
+ typedef FWindowsOpenGL FOpenGL; FOpenGL4、FOpenGLES31、FOpenGLBase
+ typedef FMacOpenGL FOpenGL; FOpenGL3、FOpenGLBase
+ typedef FLinuxOpenGL FOpenGL; FOpenGL4、FOpenGLBase

从上面的信息可以总结出FOpenGLBase是所有平台共用的接口，是平台实现必须遵守的规范，修改FOpenGLBase接口时，所有平台都会做出相应的修改。针对OpenGL规范，分别实现了OpenGLES2、OpenGLES31、OpenGL3、OpenGL4。不同平台可以实现不同的规范，也可以同时支持多种规范，当某一种规范修改接口之后，基于这种规范的平台实现就需要作出对应的调整。也可以实现单一平台独有的接口，例如只在FIOSOpenGL添加FuncA，依然可以采用FOpenGL::FuncA去调用该函数，但是不影响其他基于FOpenGLES2的实现。随着时间的推移大家觉得FuncA足够好用，就可以将FuncA提升为OpenGLES2的接口，原先的FIOSOpenGL可以不做任何调整。这种方式可以达到接口在不同平台之间平滑变化的目的，而不用每次都修改基类，这样所有平台子类每次都需要作调整，工作量会较大，而且当单个人并不是对所有平台都熟悉的时候，会异常麻烦，也会引入不稳定性和集中的测试工作量。


##### OpenGLDrv.cpp

#### OpenGLCommands.cpp

#### OpenGLTexture.cpp

##### OpenGLViewport.cpp

##### OpenGLVertexBuffer.cpp

##### OpenGLIndexBuffer.cpp

#### OpenGLState.cpp

#### OpenGLQuery.cpp

#### OpenGLUAV.cpp


