---
layout: post
title: Unreal Engine 4 代码分析——RHI模块
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