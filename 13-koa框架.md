### 认识koa
+ 前面我们已经学习了expresss, 另外一个非常流行的Node Web服务器框架就是koa
+ koa官方的介绍
  - koa: next generation web framework for node.js
  - koa: node.js的下一代web框架
+ 事实上, koa是express同一个团队开发的一个新的Web框架
  - 目前团队的核心开发中TJ的主要精力也在维护Koa, express已经交给团队维护了
  - koa旨在为Web应用程序和API提供更小, 更丰富和更强大的能力
  - 相对于express具有更强的异步处理能力
  - koa的核心代码只有1600+行, 是一个更加轻量级的框架, 我们可以根据需要安装和使用中间件

### Koa初体验
+ koa注册中间件提供了两个参数
+ ctx: 上下文(context)对象
  - koa并没有像express一样, 将req和res分开, 而是将它们作为ctx的属性
  - ctx代表一次请求的上下文对象
  - ctx.request: 获取请求对象
  - ctx.response: 获取响应对象
+ next: 本质是一个dispatch, 类似于express的next

### koa中间件
+ koa通过创建的app对象, 注册中间件只能通过use方法
  - koa并没有提供method的方式来创建中间件
  - 也灭有提供path中间件来匹配路径
  - 也不可以通过连续注册的方式来注册中间件
+ 如果真实开发中需要实现将路径path和method分离
  - 方式一: 根据 ctx.request.path 和 ctx.request.method 用if判断来实现
  - 方式二: 使用第三方路由中间件

### koa第三方路由的使用
+ koa官方并没有给我们提供路由的库, 我们可以选择第三方库: koa-router
  - npm install koa-router
+ 先封装一个 user.router.js的文件
+ 在app中将router.routes() 注册为中间件
+ 注意: allowedMethods用于判断某一个method是否支持
  - 如果我们请求正常的请求, 将正常返回数据
  - 如果请求put, deletd, patch没有的请求, 则会报错 Method Not Allowed; 状态码: 405
  - 如果请求link, copy, lock没有的请求, 则会报错 Not Implemented; 状态码: 501

### 参数解析: parmas和query
+ koa本身没有解析url的功能
+ 可以利用 koa-router第三方库 来实现

### 参数解析: json和urlencoded
+ koa本身没有解析json和urlencoded的功能
+ 可以利用koa-bodyparser来实现解析body的功能

### 参数解析: form-data
+ koa本身没有解析 form-data的功能
+ 可以利用koa-multer来实现

### 图片文件的上传
+ 利用koa-multer来实现

### koa中数据的响应
+ 输出结果: body将响应主体设置为以下之一
  - string: 字符串数据
  - Buffer: Buffer数据
  - Stream: 流数据
  - Object/ Array: 对象或者数组(会自动转换为JSON格式)
  - null: 不输出任何内容
  - 如果response.status尚未设置, Koa会自动将状态设置未200或者204
+ 注意, ctx.response.body 和 ctx.body 可以达到相同的效果
  - 原因是, 本质上ctx.body 会调用 ctx.response.body, 只是做了一层代理

### koa中的错误处理方式
+ koa中的错误处理是通过 ctx.app.emit("error", new Error(), ctx) 发送错误事件来实现的
``` js
const Koa = require("koa");

const app = new Koa();

const USER_DOSE_NOT_LOGIN = "user does not login";

app.use((ctx, next) => {
  const isLogin = false;
  if (!isLogin) {
    // 通过发送错误信息事件的方式处理错误
    ctx.app.emit("error", new Error(USER_DOSE_NOT_LOGIN), ctx);
  } else {
    ctx.body = "登陆成功";
  }
})

// 在这里监听错误事件
app.on("error", (err, ctx) => {
  let errMessage = "";
  let errStatus = 400;
  switch (err.message) {
    case USER_DOSE_NOT_LOGIN:
      errMessage = err.message;
      errStatus = 401;
      break;
    default:
      errMessage = "NOT FOUND"
      errStatus = 404;
      break;
  }

  ctx.body = errMessage;
  ctx.status = errStatus;
})

app.listen(8000, () => {
  console.log("错误处理服务器启动成功");
})
```

### koa洋葱模型
+ 两层理解含义
  - 中间件处理代码的过程
  - Response返回body执行
+ 也就是koa执行中间件的过程是从第一个中间件开始执行到最后一个中间件, 遇到next() 就执行下一个中间件的代码, 在最后一个中间件执行完毕后, 就返回上一个中间件执行剩下的代码, 在所有中间件代码都执行完毕之后再返回结果.