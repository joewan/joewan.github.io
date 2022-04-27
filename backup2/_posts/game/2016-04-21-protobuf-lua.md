---
layout: post
title: Protobuf与LUA
categories: Game
tags: Game
author: 东邪
description: unreal engine 学习
---

### protoc-gen-lua

[protoc-gen-lua](https://github.com/sean-lin/protoc-gen-lua) Google's Protocol Buffers project, ported to Lua

"Protocol Buffers" is a binary serialization format and technology, released to the open source community by Google in 2008.

There are various implementations of Protocol Buffers and this is for Lua.

protoc-gen-lua包含2个重要接口：

+ SerializeToString
+ ParseFromString

需要在lua中发送协议时：

	require "person_pb"

	-- Serialize Example
	local msg = person_pb.Person()
	msg.id = 100
	msg.name = "foo"
	msg.email = "bar"
	local pb_data = msg:SerializeToString()
	
需要解析从服务器发送的消息时：

	require "person_pb"
	
	-- Parse Example
	local msg = person_pb.Person()
	msg:ParseFromString(pb_data)
	print(msg.id, msg.name, msg.email)
	
目前项目中采用的C++解析protobuf消息，目前只需要在解析的地方进行HOOK和拦截即可，为了兼容现有的系统，消息同时在C++和LUA中进行接管。帧同步或战斗相关的服务器消息，需要在LUA中过滤掉，尽可能减少无用解析带来的消耗。

### protoc-gen-lua安装

WTF! 安装和配置protoc-gen-lua相当操蛋，花了我40分钟。
遇到endian.h文件在mac上找不到的错误，采用xcode-select --install安装xcode command tools，结果竟然失败了。最后不得不去苹果官网下载安装文件才好。

下面是编译pb.c文件需要用的编译选项：

	CFLAGS=-I/usr/local/include
	-I/usr/include
	-std=gnu99
	-D_ALLBSD_SOURCE
	LDFLAGS=-L/usr/local/lib -llua


### python版本protobuf安装

1.下载protobuf 2.6.1

2.解压，编译，安装

	$tar zxvf protobuf-2.5.0.tar.gz
	$cd protobuf-2.6.1 
	$./configure 
	$make 
	$make check 
	$make install

3.安装protobuf的python模块

	$cd ./python 
	$python setup.py build 
	$python setup.py test 
	$python setup.py install

4.安装完成，验证Linux命令 

	protoc –version

5.验证Python模块是否被正确安装 

	python 
	>>>import google.protobuf 
	如果没有报错，说明安装正常。
	
回顾一下为啥安装protoc-gen-lua so麻烦？

概念多，容易混淆，对新人不友好。例如protoc-gen-lua、pbc、protoc、protobuf等。
protoc是protobuf用于根据*.proto文件生成不同语言格式协议的工具。protoc XXX_out=./ person.proto，就是生成XXX语言的protobuf的协议文件，它会依赖protoc-gen-XXX。为了生成lua格式的protobuf协议，就需要protoc-gen-lua，所以需要建立1个软连接指向protoc-gen-lua/plugin/protoc-gen-lua。protoc-gen-lua需要依赖python版本的protobuf，首先就需要编译安装protoc，紧接着需要安装protobuf的python模块，这样protoc-gen-lua就能使用了，命令如下：

	protoc lua_out=./ person.proto

### pbc

这货是干啥用的？[pbc](https://github.com/cloudwu/pbc)

PBC is a google protocol buffers library for C without code generation.

	struct pbc_rmessage * m = pbc_rmessage_new(env, "tutorial.Person", slice);
	printf("name = %s\n", pbc_rmessage_string(m , "name" , 0 , NULL));
	printf("id = %d\n", pbc_rmessage_integer(m , "id" , 0 , NULL));
	printf("email = %s\n", pbc_rmessage_string(m , "email" , 0 , NULL));
	
	int phone_n = pbc_rmessage_size(m, "phone");
	int i;
	
	for (i=0;i<phone_n;i++) {
	    struct pbc_rmessage * p = pbc_rmessage_message(m , "phone", i);
	    printf("\tnumber[%d] = %s\n",i,pbc_rmessage_string(p , "number", i ,NULL));
	    printf("\ttype[%d] = %s\n",i,pbc_rmessage_string(p, "type", i, NULL));
	}
	
	pbc_rmessage_delete(m);
	
	Message API
	You can use wmessage for encoding , and rmessage for decoding.