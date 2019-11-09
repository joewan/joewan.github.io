---
layout: post
title: Unreal Engine 4 代码分析——RHI模块
categories: Game
tags: Game
author: 东邪
description: unreal engine 学习
---

RHI模块包含：

与RHI Profiling相关的接口：

+ FGPUProfiler：Unreal在Profiling方面做的很细致到位。
+ FGPUTiming：(Holds information if this platform's GPU allows timing. FGPUTiming is a static class. )
+ FGPUProfilerEventNode
+ FGPUProfilerEventNodeFrame

RHI的接口：

+ IRHICommandContext：DynamicRHI.h
+ FDynamicRHI
+ IDynamicRHIModule

##### FRHIResource

FRHIResource采用引用计数进行资源管理，其实现用到的TLockFreePointerList以及FthreadSafeCounter可以继续深入了解一下（TODO-001）。

###### State Blocks：

+ FRHISamplerState：FRHIResource
+ FRHIRasterizeState：FRHIResource
+ FRHIDepthStencilState：FRHIResource
+ FRHIBlendState：FRHIResource

###### Shader bindings

+ FRHIVertexDeclaration：FRHIResource
+ FRHIBoundShaderState: FRHIReource

###### Shaders

+ FRHIShader：FRHIResource
+ FRHIVertexShader：FRHIShader
+ FRHIHullShader：FRHIShader
+ FRHIDomainShader：FRHIShader
+ FRHIPixelShader：FRHIShader
+ FRHIGeometryShader：FRHIShader
+ FRHIComputeShader：FRHIShader

###### Buffers

+ FRHIUniformBufferLayout：The layout of a uniform buffer in memory。
+ FRHIUniformBuffer：FRHIResource
+ FRHIIndexBuffer：FRHIResource
+ FRHIVertexBuffer：FRHIResource
+ FRHIStructuredBuffer：FRHIResource

###### Textures

+ FLastRenderTimeContainer
+ FRHITexture：FRHIResource
+ FRHITexture2D：FRHITexture
+ FRHITexture2DArrat：FRHITexture
+ FRHITexture3D：FRHITexture
+ FRHITextureCube：FRHITexture
+ FRHITextureReference：FRHITexture

###### Misc

+ FRHIRenderQuery：FRHIResource
+ FRHIViewPort：FRHIResource