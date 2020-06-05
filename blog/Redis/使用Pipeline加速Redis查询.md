---
sidebar: auto
title: '使用Pipeline加速Redis查询'
---

# 使用Pipeline加速Redis查询

[原文: Using pipelining to speedup Redis quries](https://redis.io/topics/pipelining)

## Request/Response protocol and RTT

Redis采用客户端/服务端模型，即Request/Response protocol。

这意味着一个请求包括两个部分：

- 客户端发送查询请求至服务端。
- 服务端执行查询命令并返回响应。

如果需要执行多个命令时就会存在多个往返延时(RTT)。

请求示例：

- Client: INCR X
- Server: 1
- Client: INCR X
- Server: 2
- Client: INCR X
- Server: 3
- Client: INCR X
- Servre: 4

## Redis pipelining

可以通过客户端批量发送多个请求的方式加速查询。

请求示例：

- Client: INCR X
- Client: INCR X
- Client: INCR X
- Client: INCR X
- Server: 1
- Server: 2
- Server: 3
- Server: 4

**注意：**服务端需要使用内存存储查询结果，然后批量返回。因此要合理设置每一批请求的查询数量。

## It's not just a matter of RTT

每一次查询都会涉及昂贵的`read()`、`write()`系统调用，从用户态切换至内核台，上下文切换也是一种性能消耗。

## Pipelining VS Scripting

TODO

## Appendix: Why are busy loops slow even on the loopback interface

即使所用Redis进程同处一个物理机也可存在缓慢操作。查询请求被写入至loopback interface缓冲中，等待Redis Server读取，而Redis Server进程需要被内核进行调度。此时可能被系统调用阻塞。

