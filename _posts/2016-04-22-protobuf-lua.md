---
layout: post
title: Protobuf与LUA
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