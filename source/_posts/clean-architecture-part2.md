---
title: Clean Architecture Part 2
date: 2019-03-19 14:11:32
tags:
  - Design Principle
categories:
  - 读书
  - Clean Architecture
---

## 单一责任原则(The Single Responsibility Principle)

```
“A module should have one, and only one, reason to change.”
```

Never change a same source file for different reasons, for different purpose.

## 开闭原则(The Open Closed Principle)

```
“A software artifact should be open for extension but closed for modification.”
```

* 系统里的每个模块之间的关系一定要是单向的(unidirectional)
* 如果你不想因为B模块的改动而去修改A模块，那么B模块应该依赖于A模块。
* 软件构架的根本就是把一个系统分割为不同的模块，并且保护核心逻辑组件不会因为低层的细节模块的改动而被修改。

## 里氏替换原则(Liskov Substitution Principle)

```
What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T.
```

当你在使用一个类的时候，你可以放心的使用而不需要关心其衍生类的不同。所有的衍生类必须符合使用者所期待的基类的行为。

## 接口隔离原则(Interface Segregation Principle)

```
Clients should not be forced to depend upon interfaces that they do not use.
```
<img src="/clean-architecture-part2/lsp-bad.png" width="500" height="300" title="Bad Design">


在这个设计里，user1只会使用op1但是却要依赖于整个ops。当任意一个op的方法改动时，所有user被迫从新编译和部署。

<img src="/clean-architecture-part2/lsp-good.png" width="500" height="300" title="Good Design">

在这个新的设计里，每个user只依赖于自己的方法，并且ops保留了原有的所有功能。

## 依赖倒置原则(Dependency Inversion Principle)

```
A. High level modules should not depend upon low level modules. Both should depend upon abstractions.

B. Abstractions should not depend upon details. Details should depend upon abstraction.
```
A. 高层模块不应该依赖于低层模块，二者都应该依赖于抽象。

B. 抽象不应该依赖于具体实现细节，而具体实现细节应该依赖于抽象。

<img src="/clean-architecture-part2/dip.png" width="500" height="300" title="DIP">

在这个例子中，高层的Application只依赖于抽象的Service和抽象的ServiceFactory。当Factory的具体实现和Service的具体实现发生改动的时候，Application并不需要发生变化。
