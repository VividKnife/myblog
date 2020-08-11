---
title: 进程，线程和协程
date: 2020-08-11 21:02:30
tags:
  - process
  - thread
  - goroutine
categories:
  - 工作原理
---

## 进程(process)
进程拥有独立的内存，寻址空间和寄存器。进程是直接被内核(kernel)管理的，一般每一个程序都拥有自己的线程。他们相互独立确保不会干扰彼此的工作。

在一个多进程的计算机中，一个CPU需要同时运行多个进。这是通过中断当前进程，然后保存当前进程中的内存，寄存器，PC(program counter), 变量表等，然后让下一个进程来运行，我们称之为上下文切换(context switch)。上下文切换是相对消耗运算量的，尤其是有很多进程并发的时候，频繁的上下文切换将会占用大量的CPU资源。

## 线程(thread)

线程是一种轻量级进程，线程直接可以共享内存，但执行不同的代码，所以各自有自己的栈内存和寄存器等。

线程一般由应用软件来创建，线程之间的上下文切换也是类似于进程，唯一的区别是会保留虚拟内存。

## 协程(coroutine/goroutine)

进程和线程都是内核(kernel)级别的中断和上下文切换。他们都有在大量多线程/进程的情况中消耗CPU资源的问题。

协程将内核级别的上下文切换转换到了应用程序内。这样做的好处的通过应用程序的协调，避免的大量的内核上下文切换。而且很多协程中都使用自愿退出的模式，避免的大量的需要上下文切换的情况。

例如：RxJava, Kotlin coroutines, Go(goroutines)


更多阅读：
[Cooperative vs. Preemptive: a quest to maximize concurrency power](https://medium.com/traveloka-engineering/cooperative-vs-preemptive-a-quest-to-maximize-concurrency-power-3b10c5a920fe)

