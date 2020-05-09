---
title: 软件构架 - Clean Architecture Part 5
date: 2019-03-24 15:41:33
tags:
  - 构架
categories:
  - 读书
  - Clean Architecture
---

## 什么是软件构架？

传统物理工程的构架，构架师们只需要将这个工程从框架到细节设计出来，然后由工人施工，产品就完成了。 
而软件与硬件最大的不同是, 软件需要持续的开发，修改Bug， 添加新功能，调整已有的功能等。
所以说软件的构架并不仅仅是设计满足功能需求的软件，更重要的是为其后续的生命周期中的
**开发**，**维护**，**发布**以及**扩展**提供保障。
<!-- more -->
### 功能

软件的构架最重要也时最先考虑的当然是它的功能了。在一个优秀的系统中，它的功能的行为一定是一目了然的，开发人员不必去寻找行为，因为这些行为将是系统顶层可见的高级元素。这些元素将是在架构中具有突出位置的类或函数或模块，并且它们将具有清楚描述其功能的名称。

### 运行

一个系统的设计必须满足其运行时的需求。如果一个系统需要1万每秒的用户请求，那这个系统在构架的时候就需要考虑到这一点。在连接不同的模块时将可能性开放出来，允许其进行网络连接或者并发连接。

### 开发

软件的构架应该让开发更加简单和高效。用低耦合的模块组成的系统，可以让开发者独立的对一个模块开发，减少与其他小组沟通的时间。

### 维护

软件在维护的过程中，比如修复一个bug反而增加了其他更难修复的bug，增加了维护的成本。优秀的构架通过将系统分离成组件，并通过稳定的接口隔离这些组件，来减少维护的成本。

### 发布

当你在工作过程中发现发布占领了很大比重的时候，那么这个系统就有设计缺陷。例如一个滥用微服务的构架中，部署这些微服务花掉了大量的时间。这个时候反而将一些模块变成一个服务的子模块更有效。

### 扩展

优秀的软件一定时易于扩展，随着时间的推移，软件的功能会增加，改变，甚至替换。

所有的软件都可以被分割为两个部分，**规则**和**细节**。规则部分包含了所有的业务逻辑和过程。这个规则才是系统的价值所在。

架构师的目标是为系统创建一个形状，将规则视为系统中最重要的元素，同时使细节与该规则无关。这样就可以延迟和推迟有关这些细节的决定。

当你在开发系统的高层规则的时候请在**不考虑**以下细节的情况下进行：

* 数据库的类型
* 服务器的类型
* API的模型
* Dependency Injection的框架