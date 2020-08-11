---
title: 阻断与非阻断I/O
date: 2020-08-11 23:02:30
tags:
  - I/O
categories:
  - 工作原理
---

## I/O 基础

简单介绍一下I/O的基本工作方式

1. 你的程序通过“syscall()”请求操作系统的内核去做一个I/O操作。
1. 一个"syscall()"是阻断的，你的程序需要等待内核返回才能继续执行你的代码。
1. 内核执行底层的I/O操作，大多在硬件层面，执行时间根据的硬件的速度而变化。

{% asset_img syscall.png Syscall %}

<!-- more -->

## 阻断型I/O

大多语言，比如说Java，Python的I/O调用的阻断型的，你的程序需要等待I/O返回之后才能继续后面的工作。当有高并发的需求的时候，大量的线程需要进行大量的上下文切换。就会导致浪费大量CPU资源。

## 非阻断型I/O

Nodejs的独特的事件循环使得他可以方便的运行非阻断型I/O。每一个I/O请求都会作为一个消息发送在事件循环中，Nodejs会继续执行其他代码直到这个I/O请求结束之后通过callback返回结果。

由于这个独特的设计是的Nodejs在处理高并发，I/O为主的程序中发挥极大的功效。当传统的网络服务器(线程型)能处理4000并发的时候，Nodejs可以处理1百万并发，和大约60万并发websockets的连接。

但是由于Nodejs基于V8单线程的原因，他不适合处理高运算量的代码，一个高运算量的循环就可以使得整个程序瘫痪。

基于Nodejs的这些特性，以下是一些比较适合他的应用场景：

* 聊天软件
* 读写NoSQL数据库的API(MongoDB)
* 队列/消息请求
* 数据流
* 代理(Proxy)



更多阅读：  
[Why the Hell Would You Use Node.js
](https://medium.com/the-node-js-collection/why-the-hell-would-you-use-node-js-4b053b94ab8e)    
[Server-side I/O Performance: Node vs. PHP vs. Java vs. Go](https://www.toptal.com/back-end/server-side-io-performance-node-php-java-go)