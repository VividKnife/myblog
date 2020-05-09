---
title: "难测对象模式 - Clean Architecture Part 10"
date: 2019-04-04 20:31:30
tags:
  - 构架
categories:
  - 读书
  - Clean Architecture
---


## 展示者和难测对象模式（Presenters and Humble Objects）

将系统中难以测试的部分取出来，而使得其他逻辑易于测试。

例如说，UI就是难以测试的部分，而展示者（Presenters）则把系统中心的数据转化为UI可以直接用来现实的格式。这样我们就可以通过测试展示者来测试系统的行为。

```
Application –> Presenter –> ViewModel –> View
```


## Data Access Object（DAO）

DAO是一个包含所有业务逻辑所需数据读写方法的接口。这个接口将被用例使用，而由数据库的模块implement。

## 服务Proxy

当你的系统需要使用其他网络服务的时候，最好的方法是创建一个Proxy的类来隔离耦合。这个Proxy的类会将系统提供的数据转化为其他网络服务接受的数据格式。这样我们就可以直接mock这个Proxy的类来进行测试了。

## 部分隔离

在很多情况下严格执行界限分离的构架非常的昂贵。
- 严格的界限分离需要多写很多代码
- 需要额外的工作去部署
- 你有可能并不需要分离这部分的模块

* 你可以使用部分隔离的方法，按照界限分离的方式开发，但是只放在同一个模块中。
* Strategy Pattern
<img src="/clean-architecture/clean-architecture-part10/sp.png" width="500" height="300" title="Strategy Pattern">
* Facade Pattern
<img src="/clean-architecture/clean-architecture-part10/fp.png" width="500" height="300" title="Facade Pattern">

## 入口程序模块（The Main Component）

* 入口程序依赖于所有的部分，它知道所有模块的存在。
* 最细节的模块，需要创建所有的工厂类，模块等。
* 为高级系统加载所有内容，然后将控制权交给它。
* 当你把入口程序作为整个构架之外的插件来看待的话，系统配置的问题就会容易解决。

