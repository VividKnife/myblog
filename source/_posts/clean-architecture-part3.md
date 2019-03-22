---
title: Clean Architecture Part3
date: 2019-03-21 21:50:20
tags:
  - 设计原则
categories:
  - 读书
  - Clean Architecture
---

## 模块(Components)

``` 
Components are the units of deployment. 
They are the smallest entities that can be deployed as part of a system.
```

模块就是可部署的最小单位。不同的模块组合起来构成一个系统。

设计优良的模块可以做到**独立的部署和独立的开发**。

## 模块聚合(Components Cohesion)

那么如果决定一个类应该属于哪个模块呢？

### 模块聚合的三个原则

#### 1. REP 复用发布等价原则(The Reuse/Release Equivalence Principle)

```
The granule of reuse is the granule of release.
```

复用的部分就是发布的部分。  

* 一个可复用的模块(包，库，API)，对它的修改必须兼容已有的功能，当用户升级到新版本的时候，不会影响到原系统的正常运作。

* 每次新的发布，应该有一个规范的发布，包括版本号，类库的说明等等。以便用户查阅和升级。
  
#### 2. CCP (The Common Closure Principle)
```
Gather into components those classes that change for the same reasons and at the same times. 
Separate into different components those classes that change at different times and for different reasons.
```
将会同时并且因为同样的原因进行修改的类放入一个模块中。
将会在**不同时期**因为**不同的原因**进行修改的类放入不同的模块中。

#### 3. CRP(The Common Reuse Principle)
```
Don’t force users of a component to depend on things they don’t need.
```

不要强制用户依赖于一些他们不会使用的东西。-> 不要依赖于你不需要的东西。

**一个模块不应该由于其不依赖的代码而需要重新编译/测试/发布**

<img src="/clean-architecture-part3/components-cohesion-principles.png" width="500" height="300" title="Cochesion principles tension">