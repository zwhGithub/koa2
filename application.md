# Hello Koa

#### 首先看一下原生node和koa分别是如何创建一个server

```javaScript
/**
*原生Node创建server
*/
const http = require('http');
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});
server.listen(3000);

/**
*koa创建server
*/
const Koa = require('koa');
const app = new Koa();
app.use(ctx => {
  ctx.body = 'Hello Koa';
});
app.listen(3000);
```

> **看一下源码中koa究竟都做了什么**

```javaScript
//1.首先创建了一个Application的类，继承了Emitter类，然后暴露出去。
module.exports = class Application extends Emitter {
  constructor() {
    super();
  }
}

//2.类下面有一个listen方法，创建http服务
 listen(...args) {
    debug('listen');
    const server = http.createServer(this.callback());
    return server.listen(...args);
 }
```

>  **在Hello Koa中，引入这个类，然后实例化这个类，在使用下面的listen即搭建起了一个简单的koa服务。**

 

##### 然后讲一下初始化koa的时候，这个类的下面有一个use方法，也就是我们添加中间件的方法

```javascript
  use(fn) {
      //如果use里面传的不是方法，直接错误抛出去
    if (typeof fn !== 'function') throw new TypeError('middleware must be a function!');  
      //如果是星号函数，用一个库转成async await方法(去兼容koa1的中间件)
    if (isGeneratorFunction(fn)) {
      deprecate('Support for generators will be removed in v3. ' +
        'See the documentation for examples of how to convert old middleware ' +
        'https://github.com/koajs/koa/blob/master/docs/migration.md');
      fn = convert(fn); //转async await方法
    }
    debug('use %s', fn._name || fn.name || '-');
    this.middleware.push(fn); //koa把所有的中间件都放在一个数组里面
    return this;
  }
```

如代码所示，koa把所有的中间件放在一个数组里面，然后koa2也是兼容generator函数，为了兼容koa1的中间件。



##### callback方法Koa处理请求的方法。

```javascript

  callback() {
    const fn = compose(this.middleware); //组装中间件

    if (!this.listeners('error').length) this.on('error', this.onerror);

    const handleRequest = (req, res) => {
      const ctx = this.createContext(req, res);
      return this.handleRequest(ctx, fn);
    };

    return handleRequest;
  }

```
> 接下来就是看一下callback中compose如何处理了中间价和中间件的执行机制
>
> ##### [点击查看](./middleware.html)

