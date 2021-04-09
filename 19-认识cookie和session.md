### 登陆凭证
+ 为什么需要登陆凭证呢?
  - 在Web开发中, 使用最多的协议式http协议, 但是http协议是一个无状态协议
+ 什么式无状态协议?
  - 比如说, 我们登陆了一个网站, 在登陆的时候需要输入用户名和密码
  - 登陆成功后, 我们以用户的身份访问其他数据和资源, 还是通过http请求去访问的
  - 但是对于服务器来说, 每一次请求都是一个单独的请求, 和之前的请求没有任何关系, 所以服务器不会认可我们的用户身份
  - 这个就是http的无状态, 也就是服务器不知道你上一步做了什么, 所以我们需要由登陆凭证

### 认识cookie
+ Cookie(复数形态Cookies), 又称为"小甜饼", 类型为"小型文本文件", 某些网站为了辨别用户身份而存储在用户本地终端(Client Side)上的数据
  - 浏览器会在特定的情况下携带上cookie来发送请求, 我们可以通过cookie来获取到一些信息
+ Cookie总是保存至客户端中, 按照在客户端中的存储位置, Cookie可以分为内存Cookie和硬盘Cookie
  - 内存Cookie由浏览器维护, 保存至内存中, 浏览器关闭的时候Cookie就会消失, 其存在的时间是短暂的
  - 硬盘Cookie保存在硬盘中, 有一个过期时间, 用户手动清理或者过期时间到了, 才会被清理
+ 如何判断一个Cookie是内存Cookie还是硬盘Cookie?
  - 没有设置过期时间, 默认情况下Cookie是内存Cookie, 其在关闭浏览器的时候会自动删除
  - 有设置过期时间, 并且过期时间不为0或者负数的cookie为硬盘cookie, 需要手动或者到期的时候才会被删除

### cookie常见的属性
+ cookie的生命周期
  - 默认情况下cookie是内存cookie, 也称为会话cookie, 也就是在浏览器关闭的时候会被自动删除
  - 我们可以通过设置expire或者max-age来设置过期时间
    - expire: 设置的是Date.toUTCString(), 设置格式是: exoires= date-in-GMTString-format;
    - max-age: 设置过期的秒钟, max-age=max-age-in-seconds
+ cookie的作用域: 允许cookie发送给哪些URL
  - Domain: 指定哪些主机可以接受cookie
    + 如果不指定, 默认就是origin, 不包括子域名
    + 如果指定了Domain, 则包含子域名, 例如: 如果设置Doamin=mozila.org,则cookie也包含在子域名中(如developer.mozila.org)
  - Path: 指定主机下哪些路径可以接收cookie
    + 例如: 设置Path=/docs, 则以下地址都会匹配
      + /docs
      + /doce/Web/
      + /doce/Web/HTTP

### 客户端中设置cookie
+ js直接设置和获取cookie
``` js
// 获取cookie
console.log(document.cookie)

// 设置内存cookie
document.cookie = "name=xiaoLam;"

// 设置硬盘cookie
document.cookie = "name=somin;max-age=5;"

// 如何删除cookie, 本质是找到对应的cookie, 将其的max-age设置为0
document.cookie = "name=somin;max-age=0;"
```

### 服务器中设置cookie
+ Koa中默认支持直接操作cookie
``` js
// 如何在服务器端设置cookie
// 使用cookies.set(name, value, option对象)方法即可设置
// 该方法需要传入三个参数, 前两个参数为键值对, 第三个参数为option对象可以设置cookie的一些属性
testRouter.get("/set", (ctx, next) => {
  ctx.cookies.set("name", "xiaoLam", {
    // 注意这里的maxAge对应的是毫秒数
    maxAge: 5 * 1000
  })
  ctx.body = "你的cookie已经设置好了"
})

// 如何在服务器端获取到cookie
// 使用cookies.get(name)方法即可获取
testRouter.get("/get", (ctx, next) => {
  const value = ctx.cookies.get("name");
  ctx.body = `你的cookie为${value}`;
})
```

### Session是基于cookie来实现的
+ 在Koa中, 我们需要借助koa-session来实现session认证
``` js
const Session = require("koa-session");
// 在koa中使用session需要借助第三方库 koa-session
// 实际上, session是借助cookie来实现的
// session只是把cookie中设置的value值使用base64进行了一层加密
// 所以session主要是解决cookie传递的使用使用明文传递的缺点
// 创建session的配置
const session = Session({
  // key用于设置cookie的name值
  key: "sessionid",
  // 这里的maxAge也是对应毫秒的
  maxAge: 60 * 1000,
  // 通过设置signed的值来设置是否使用前面, 为true(默认)的时候不可以修改对应的value值, 设置为false的时候可以进行修改
  signed: true
}, app);
// 使用keys进行加严操作
app.keys = ["aaa"];
app.use(session);


testRouter.get("/set", (ctx, next) => {
  const id = 110;
  const name = "xiaoLam";

  // 通过ctx.session来设置value
  ctx.session.user = {
    id,
    name
  }

  ctx.body = "session 设置成功";
})

// 如何在服务器端获取到session
testRouter.get("/get", (ctx, next) => {
  // 直接通过ctx.sesstion 来获取对应的cookie
  ctx.body = ctx.session.user
})
```