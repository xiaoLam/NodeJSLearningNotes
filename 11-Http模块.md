### Web服务器
+ 什么是Web服务器
  - 当应用程序(客户端)需要某一个资源的时候, 可以向一台服务器, 通过http请求获取这个资源; 提供资源的这个服务器, 就是一个Web服务器
+ 目前有很多开源的web服务器: Nginx, Apache(主要传输静态资源), Apache Tomcat(主要传输静态和动态资源), Node.js

### Web服务器初体验(创建一个简单的服务器
+ 首先引入http模块, 然后通过createServer来创建服务器对象
  - http.createServer会返回服务器对象
  - 底层其实直接使用new Server对象
+ 创建Server对象的时候会传入一个回调函数, 在这个回调函数被调用的时候会传入两个参数
  - 第一个参数 req: request请求对象, 包含请求相关信息
  - 第二个参数 res: response响应对象, 包含服务器要发送给客户端的信息
+ 监听主机和端口号
  - Server通过listen方法来开启服务器, 并且在某一个主机和端口上监听网络请求
    + 也就是当我们通过ip: port的方式发送到我们监听的web服务器上时
    + 我们就可以对其进行相关的处理
  - listen方法有三个参数
    + 第一个参数为端口号port: 可以不传, 系统会默认分配端口, 后续项目中我们会写入到环境变量中
    + 第二个参数为主机号host: 通用长传入loaclhost, ip地址127.0.0.1, 或者ip地址0.0.0.0, 默认为0.0.0.0
      + localhost: 本质上是一个域名, 通常情况下最终会被解析成127.0.0.1
      + 127.0.0.1: 回环地址(Loop Back Address), 表达的意思其实是我们主机自己发出去的包, 直接被自己接收
        - 正常的数据库经常 应用层 - 传输层 - 网络层 - 数据链路层 - 物理层
        - 而回环地址, 是在网络层直接被获取到了, 是不会经常数据链路层和物理层的
        - 比如我们监听127.0.0.1的时候, 在同一个网段下的主机中, 通过ip地址是不能访问的
      + 0.0.0.0:
        - 监听IPV4上所有的地址, 再根据端口找到不同的应用程序
        - 比如我们监听0.0.0.0时, 在同一个网段下的主机中, 通过ip地址是可以访问的
    - 第三个参数为回调函数: 服务器启动成功时的回调函数

### request对象
+ 在客户端向服务器端发送请求的时候, 会携带很多信息, 比如:
  - 本次请求的URL, 服务器需要根据不同的URL进行不同的处理
  - 本次请求的请求方式, 比如GET, POST请求传入的参数和处理的方式是不一样的
  - 本次请求的headers种携带的一些信息, 比如客户端信息, 客户端接收数据的格式, 客户端支持的编码格式等
  - 等等...
+ 这些信息, Node会帮助我们封装到request对象中, 我们可以直接处理这个request对象

### URL的处理
+ 客户端在发送请求的时候, 会请求不同的数据, 此时服务器端需要根据不同的请求地址, 作出不同的响应

### URL的解析
+ 如果客户端在发送请求的时候的url携带了其他的参数
+ 此时的url会变得很复杂
+ 此时我们可以使用内置模块url对url进行解析
+ 然后我们又发现解析出来的url中的query又是一个复杂的数据
+ 此时我们可以使用内置模块querystring来对query进行解析

``` js
const http = require('http')

// 引入url模块
const url = require("url")

// 引入querystring模块
const qs = require("querystring")

const server = http.createServer((req, res) => {
  // request对象种的url最基本用法
  // if(req.url === "/login") {
  //   res.end("欢迎回来")
  // } else if (req.url === "/user") {
  //   res.end("登陆成功")
  // } else {
  //   res.end("请求错误, 请检查")
  // }

  // 实际上url是很复杂的
  // 此时我们需要借助一个交url的模块帮助解析从客户端传输到服务器的url
  console.log(url.parse(req.url));
  const { pathname, query } = url.parse(req.url);
  console.log(pathname, query);

  // 此时我们又发现传输过来的query又是很复杂的, 实际上我们可以通过字符串切割的方式来获取到需要的数据
  // 为了方便操作, 我们可以使用querystring模块来对处理query
  console.log(qs.parse(query));
  const { username, password } = qs.parse(query);
  console.log(username, password);

  res.end("请求成功")
})

server.listen(8888, () => {
  console.log("服务器启动成功");
})
```

### method的处理
+ 在Restful规范(设计风格)中, 我们对于数据的增删改查一个通过不同的请求方式
  - GET: 查询数据
  - POST: 新建数据
  - PATCH: 更新数据
  - DELETE: 删除数据
+ 所以, 我们可以通过判断不同的请求方式进行不同的处理
  - 比如创建一个用户
  - 请求接口为/users
  - 请求方式为POST请求
  - 携带数据username和password

``` js
const http = require("http")
const url = require("url")
const qs = require("querystring")

// 创建一个web服务器
const server = http.createServer((req, res) => {
  const { pathname } = url.parse(req.url)
  if (pathname === "/login") {
    if (req.method === "POST") {
      // 如果要获取到post请求中的body中的数据
      // 需要使用监听req中的data事件
      // 这是因为post请求中传输过来的数据是使用流的方式传输的
      // 所以注意这里传输过来的data是buffer格式的
      // 可以使用setEncoding方法来设置编码
      req.setEncoding("utf-8")
      req.on("data", (data) => {
        console.log(data, typeof data); // 注意这里的data是字符串格式的
        // 为了更好的处理传输过来的data, 使用JSON.parse() 将JSON字符串转换为对象格式
        const { username, password } = JSON.parse(data)
        console.log(username, password);
      })
      res.end("请求成功")
    }
  }
  // res.end("请求成功")
})

server.listen(8888, () => {
  console.log("服务器启动成功");
})
```

### headers属性
+ 在requert对象中的header中包含很多有用的信息, 客户端会默认传递过来一些信息
+ content-type: 这次请求携带的数据类型
  - application/json 表示是一个json类型
  - text/plain 表示是一个文本类型
  - application/xml 表示是一个xml类型
  - multipart/form-data表示是上传文件
+ content-length: 文件的大小和长度
+ keep-alive:
  - http 是基于TCP协议的, 但是通常在进行一次请求和响应结束后立刻终端
  - 在http1.0中, 如果想要继续保持链接
    - 浏览器需要在请求头中添加 connection: keep-alive
    - 服务器需要在响应头中添加: connection: keey-alive
    - 当客户端再次请求后, 就会使用同一个链接, 直到一方中断链接
  - 在http1.1中, 所有链接默认是connection:keep-alive的
    - 不同的web服务器会有不同的保持keep-alive的时间
    - Node中默认是5s钟
+ accept-encoding: 告知服务器, 客户端支持的文件压缩格式, 比如js文件可以使用gzip编码, 对应.gz文件
+ accept: 告知服务器, 客户端可以接收文件的格式类型
+ user-agent: 客户端相关信息

### response对象返回响应结果
+ 如果我们希望给客户端返回响应的结果数据, 可以通过两种方式
  - write方法: 这种方式是直接返回数据, 但是并没有关闭流
  - end方法: 这种方式是写出最后的数据, 并且在写出后会关闭流
+ 如果服务器在返回响应数据后没有调用end方法, 客户端会一直等待结果
  - 所以客户端在发送网络请求时, 都会设置超时时间

### response对象返回状态码
+ Http状态码(Http Status Code) 是用于表示Http响应状态的数字代码
  - Http状态码非常多, 可以根据不同的情况, 给客户端返回不同的状态码
+ 设置状态码常见的的方式有两种

``` js
const http = require("http")

const server = http.createServer((req, res) => {
  // 设置响应码有两种方式
  // 方式一: 直接设置
  // req.statusCode = 400;

  // 方式二: 与响应head一起设置
  res.writeHead(400)

  res.end("hello world")
})

server.listen(8888, () => {
  console.log("服务器启动成功");
})
```

### response对象响应头文件
+ Header设置Content-Type有很重要的作用
  - 该属性决定了浏览器根据服务器返回的数据做出什么样的处理方式
+ 常见的设置响应头文件的方式有两种
``` js
const http = require("http")

const server = http.createServer((req, res) => {
  // 设置响应的Header
  // 设置方式一: 直接设置
  // res.setHeader("Content-type", "text/plain");

  // 设置方式二: 通过writeHead方法设置
  res.writeHead(200, {
    "Content-type": "text/plain"
  })

  res.end("hello world")
})

server.listen(8888, () => {
  console.log("服务器启动成功");
})
```

### http模块发送网络请求
+ 在http中也是可以发送网络请求的, 可以用于反向代理
+ 实际开发中并不会使用原生的http模块来进行网络请求, 而是使用axios库来实现
+ axios库可以在浏览器中使用, 也可以在Node中使用
  - 在浏览器中使用, axios是基于xhr来实现的
  - 在Node中, axios是基于http内置模块来实现的

