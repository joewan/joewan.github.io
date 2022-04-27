---
layout: post
title: CryEngine Source Code
categories: Game
tags: Game
author: 东邪
description: CryEngine
---

#### ICryUnknown
接口文件以I开头。

##### IUnKnown接口的定义：
IUnKnown是一个接口。 所有COM接口都继承IUnKnown。IUnKnown的定义在WIN32 SDK中的UNKNWN头文件中。

	///IUnKnown的定义
	interface IUnKnown
	{
		virtual HRESULT __stdcall QueryInterface(const IID& iid,
			void **ppv)=0;
		virtual ULONG __stdcall AddRef()=0;
		virtual ULONG __stdcall Release()=0;
	}

##### IUnKnown接口的作用：
COM定义的每一个接口都必须从IUnknown继承过来，其原因在于IUnknown接口提供了两个非常重要的特性:生存期控制和接口查询。客户程序只能通过接口与COM对象进行通信,虽然客户程序可以不管对象内部的实现细节,但它要控制对象的存在与否。IUnknown接口是所有COM接口的根。

#### Common目录结构

增加文件夹macro，itf