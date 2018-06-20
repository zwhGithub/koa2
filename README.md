# Koa源码分析

![image-20180528181625044](./img/koa2.jpeg)

### 1. Why Koa ?
>koa 是由 Express 原班人马打造的，致力于成为一个更小、更富有表现力、更健壮的 Web 框架。 使用 koa 编写 web 应用，通过组合不同的 generator，可以免除重复繁琐的回调函数嵌套， 并极大地提升错误处理的效率。koa 不在内核方法中绑定任何中间件， 它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。


- async await写法，让异步更加优雅。（**代码是写给人看的，顺便让机器执行而已**）
- 良好的抽象，简洁的api，简单、吊炸天的中间件机制。
- 强大的社区，GayHub上数不胜数的中间件。

### 2. 小试牛刀之源码文件一览

> npm上down下koa之后打开koa/lib文件夹可以看到四个文件，足以证明koa框架是多么简洁。（不过koa框架依赖了很多node_modules）

![image-20180528181625044](./img/1527502768570.jpg)
- application.js 入口文件，也是骨架文件，创建一个服务。
- context.js app 的 context 对象, 传入中间件的上下文对象。
- request.js app 的请求对象，包含请求相关的一些属性。
- response.js app 的响应对象，包含响应相关的一些属性。

#### 大致总的分为三章来分享一下koa源码 

[1. 第一章 Hellow Koa](https://mp.weixin.qq.com/s/PiI4PTzUBNLPm2cKd71lyw)
