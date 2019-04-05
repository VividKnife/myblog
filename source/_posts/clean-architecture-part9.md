---
title: 干净的构架 - Clean Architecture Part 9
date: 2019-04-03 21:02:39
tags:
  - 构架
categories:
  - 读书
  - Clean Architecture
---

这篇我们将介绍这本书最核心的概念 - 干净构架。

首先一个干净的构架应该拥有以下几点特征：
* 不依赖于任何框架，数据库，UI，服务器
* 在没有UI，数据库，服务器的情况下可以测试

<img src="/clean-architecture-part9/clean-architecture.png" width="500" height="300" title="CleanArchitecture">

<!-- more -->
```
Source code dependencies must point only inward, toward higher-level policies.
```
在这个图中，在圈靠外的模块应该依赖于内部的模块。

## Entities （实体）

实体包含着最核心的业务逻辑。它不应该由于外部的改动而变化。

## Use Case （用例）

用例会直接使用和控制实体。它满足着系统主要的功能。

## Interface Adapters （接口适配器）

接口适配器的主要工作是连接**业务逻辑模块**和**外部的细节模块**，它把数据转化为便于两者使用的模型。例如： 把SQL数据库的数据提取出来转化为用例可以使用的结构， 并且将用例返回的数据转化为便于UI显示的结构。

## Frameworks And Drivers （框架和引擎）

这是干净构架中最靠外的部分。一个干净的构架应该让自己业务逻辑完全不依赖于这些工具。这样，当一些工具无法满足这个系统的需求的时候，我们可以轻易的更换这些工具。

## 跨越界限

大多情况下，在跨越界限的时候，控制的方向都是向内的，例如**UI**控制**接口适配器**，**接口适配器**调用**用例**，**用例**再操作**实体**。但是我们要通过**依赖倒置原则**来保证*代码的依赖是外的*。

## 跨越界限的数据

跨越界限的数据，其结构一定要越简单越好。并且这个数据结构的定义应该居住在靠内的模块中。

## 例子
注意在以下例子中，所有跨越边界的依赖都是向内的。

<img src="/clean-architecture-part9/web.png" width="500" height="300" title="web" />

