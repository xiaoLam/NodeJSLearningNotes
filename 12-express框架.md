### 认识Web框架
+ 在前面我们学习了使用http内置模块来搭建Web服务器, 为什么还要使用框架呢?
  - 原生http在进行很多处理的时候, 会较为复杂
  - 比如URL判断, Method判断, 参数处理, 逻辑代码处理等, 都需要我们自己来处理和封装
  - 并且所有的内容都放在一起, 会非常的混乱
+ 目前在Node中比较流行的Web服务器框架是express, koa
+ express早于koa出现, 并且在Node社区中迅速流行起来
  - 我们可以基于express快速, 方便地开发自己的Web服务器
  - 并且可以通过一些实用工具和中间件来扩展自己的功能
+ 重点: Express整个框架的核心就是中间件, 理解了中间件其他一切都非常简单.中间件本质上是传递给express的回调函数.

### Express安装
+ express的使用过程有两种方式
  - 方式一: 通过express提供的脚手架, 直接创建一个应用的骨架, 终端命令行输入 express 项目名称 , 按下回车键即可创建
    + 安装脚手架
    + npm install -g express-generator
    + 创建项目
    + express express-demo
    + 安装依赖
    + npm install
    + 启动项目
    + node bin/www
  - 方式二: 从零搭建自己的express应用结构
    + npm init -y

### Express的基本使用

### 认识中间件
+ Express是一个路由和中间件的Web框架, 它本身的功能是非常少的
  - express应用程序本质上是一系列中间件函数的调用
+ 中间件是什么
  - 中间件的本质是传递给express的一个回调函数
  - 这个回调函数接收三个参数
    - 请求对象(request对象)
    - 响应对象(response对象)
    - next函数(在express中定义的用于执行下一个中间件的函数)
+ 中间件可以执行哪些任务?
  - 执行任何代码
  - 更改请求(request)和响应(response)对象
  - 结束请求-响应周期(返回数据)
  - 调用栈中的下一个中间件
+ 如果当前中间件功能没有结束请求-响应周期,则必须调用next()函数将控制权传递给下一个中间件功能,否则,请求将被挂起

### 应用中间件-自己编写
+ 如何将一个中间件应用到我们的应用程序中呢?
  - express主要提供了两种方式: app/router.use 和 app.router.methods
  - methods指的是常用的请求方式, 比如app.get和app.post等, 其实还有app.all, 但是不常用

### 应用中间件-body解析
+ 并非所有的中间件都需要我们从零取编写
  - express中有内置一些帮助我们完成对request解析的中间件
  - registry仓库中也有很多可以帮助我们开发的中间件
+ 在客户端发送post请求的时, 会将数据放到body中
  - 客户端可以通过请求json的方式传递
    + 此时使用 app.use(express.json()) 中间件即可完成数据解析
  - 客户端通过form表单的方式传递
    + 此时使用 app.use(express.urlencoded({ extend: true })) 中间件即可完成解析
      - extend是一个布尔值, 有两个值ture和false
      - 为true的时候, 表示使用第三方库qs解析
      - 为false的时候, 表示使用Node内置模块querystring解析

### express响应数据
+ express响应数据的常用方法又以下几种
+ end方法
  - 类似于http中的response.end方法, 用法是一致的
+ json方法
  - json方法中可以传入很多的类型: object, array, string, boolean, number, null等, 都会被转换为json格式返回
+ status方法
  - 该方法用于返回设置的状态码
+ 更多的响应方式: https://www.expressjs.com.cn/4x/api.html

### express的路由
+ 如果我们将所有的代码逻辑都写在app中, 那么app会变得越来越复杂
  - 一方面完整的Web服务器包含非常多的处理逻辑
  - 另一方面有些处理逻辑其实是一个整体, 我们应该将它们放在一起: 比如对users相关的处理
    + 获取用户列表
    + 获取某一个用户信息
    + 创建一个新的用户
    + 删除一个用户
    + 更新一个用户
+ 我们可以使用express.Router来创建一个路由处理程序
  - 一个Router实例拥有完整的中间件和路由系统
  - 因此, 它也被称为迷你应用程序(mini-app)

### 静态资源服务器
+ 部署静态资源我们可以选择很多方式
  - Node也可以作为静态资源服务器, 并且express给我们提供了方便部署静态资源的方法
  - 直接以使用中间件的方式调用 express.static("需要部署的项目根目录") 即可完成部署
  

