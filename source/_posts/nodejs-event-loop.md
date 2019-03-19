---
title: Javascript 异步和事件循环
date: 2019-03-18 21:02:30
tags:
  - nodejs
  - javascript
  - 异步
categories:
  - javascript
  - 工作原理
---


## Javascript 异步和事件循环(Event Loop)

### 单线程

我们常说“JavaScript是单线程的”。

所谓单线程，是指在JS引擎中负责解释和执行JavaScript代码的线程只有一个。不妨叫它 **主线程**。

但是实际上还存在其他的线程。例如：处理AJAX请求的线程、处理DOM事件的线程、定时器线程、读写文件的线程(例如在Node.js中)等等。这些线程可能存在于JS引擎之内，也可能存在于JS引擎之外，在此我们不做区分。不妨叫它们 **工作线程**。

### 同步与异步

**同步(Sync)**: 如果在调用函数的时候，当这个函数返回时，你已经拿到了预期的结果，那么这个函数就是同步的。

``` javascript
console.log('Hello, world!');
```

在一些主流的语言比如说C, C++, Java, Python 等，大部分情况下都是同步程序。即使在进行网络访问的时候。

``` java
User user = client.getUser('Bob');
// 程序将会这这里等待结果的返回。
```

**异步(Async)**: 如果在调用函数的时候，当这个函数返回时，你并不能拿到你想要的结果，而且需要在将来通过一定的方法获得，那么这个函数就是异步的。

``` javascript
fs.readFile('foo.txt', 'utf8', function(err, data) {
    console.log(data);
});
// 这个函数将会直接返回并继续执行后面的代码
console.log('我刚刚请求了一个文件！');
//在上面的代码中，我们希望通过fs.readFile函数读取文件foo.txt中的内容，并打印出来。
//但是在fs.readFile函数返回时，我们期望的结果并不会发生，而是要等到文件全部读取完成之后。如果文件很大的话可能要很长时间。
```

异步在java中也有使用。

``` java
Feature<User> featureUser = asyncClient.getUser('Bob');
// 程序并不会进行等待，可以立刻做一些其他的事情

User user = featureUser.get();
```

### 异步的执行过程

主线程发起一个异步请求，相应的工作线程接收请求并告知主线程已收到(异步函数返回)；主线程可以继续执行后面的代码，同时工作线程执行异步任务；工作线程完成工作后，通知主线程；主线程收到通知后，执行一定的动作(调用回调函数)。

在Javascript中，通知主线程是由消息队列和事件循环(Event Loop)完成的。

```
工作线程将回调函数放到消息队列，主线程通过事件循环过程去取回调函数。
```

### 事件循环

之所以称之为事件循环，是因为它经常按照类似如下的方式来被实现：
``` javascript
while (queue.waitForMessage()) {
  queue.processNextMessage();
}
```
如果当前没有任何消息，queue.waitForMessage() 会同步地等待消息到达。

值得一提的是事件循环中取得的消息将会被同步的执行，每一个消息完整的执行后，其它消息才会被执行。

{% asset_img event-loop.png Event Loop %}
