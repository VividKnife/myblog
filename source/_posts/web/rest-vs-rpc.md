---
title: REST VS RPC
date: 2020-09-10 21:02:30
tags: 
  - 网络
categories:
  - 网络
---


网上很多关于REST和RPC的区别讨论，但是大多都关注于他们格式，例如：  
`RPC: POST /createUser` vs `REST: POST /user`

其实他们的核心不同是如何discovery peers， performance和代码风格

## Discovery Peers & Performance

通常，RPC使用在一个系统内部的不同微服务。 一般来说需要一个注册服务或者平台来让消费者发现生产者。当他们交流的时候可以直接host to host连接。再加上RPC可以拥有自己定制的交流协议，这样可以大大的降低使用RPC的延迟。

REST 一般大量用于对外部的Public API。 需要在服务器前面架设一个网关和负载均衡。这样可以方便的暴露一个单独的网址，但是会增加延迟。使用REST可以更方便的对GET请求进行Cache。例如CDN Cache。


## 代码风格

使用PRC使得不同的微服务之间更加耦合，由于很容易添加一个新的RPC方法，经常会导致RPC服务臃肿。大多情况下使用同一种语言和框架。

REST 更加注重的自己的Schema，ACTION + Resource，使得API设计更加稳定。而且编程语言和框架也没有特殊要求。