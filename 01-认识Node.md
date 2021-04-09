### 什么是node
+ 官方定义:
  - Node.js是一个基于V8 JavaScript引擎的JavaScript运行环境
+ 这个定义是很笼统的
  - 首先什么是JavaScript的运行环境呢?
  - 为什么JavaScript需要特别的运行环境呢?
  - 什么是JavaScript引擎呢?
  - 什么是V8?

### 浏览器内核
+ 浏览器是如何运行JavaScript代码的?
+ 不同的浏览器由不同的浏览器内核组成
  + Gecko: 早期Netscape和Mozilla Firefox浏览器使用
  - Trident: 微软开发, IE浏览器使用, 但是现在Edge浏览器已经转向Blink了
  - Webkit: 苹果基于KHTML开发, 开源的, 用于Safari, Google Chrome之前也使用
  - Blink: 是Webkit的一个分支, Google开发, 目前应用于Google Chrome, Edge, Opera等
+ 事实上, 浏览器内核指的就是浏览器的排版引擎
  - 排版引擎, 也称为浏览器引擎, 页面渲染引擎或者样板引擎

### 渲染引擎工作过程
+ 页面渲染的这个执行过程中, 当HTML解析的时候遇到JavaScript标签, 浏览器会停止解析HTML, 而去加载和执行JavaScript代码
+ 为什么不直接异步加载执行JavaScript代码, 而是样停止执行HTML代码呢?
  - 这个是因为JavaScript代码可以操作DOM
  - 所以浏览器希望将HTML解析的DOM和JavaScript操作之后的DOM放在一起来生成最终的DOM树, 而不是频繁的去生成新的DOM树
+ 而JavaScript代码就是由浏览器中的JavaScript引擎来执行的

### JavaScript引擎
+ 为什么需要JavaScript引擎
  - 实际上JavaScript代码无论是交给浏览器还是Node去执行, 最后都是被CPU执行的
  - 但是CPU只认识特定的指令集, 这个指令集实际上就是机器语言
  - 所以我们需要JavaScript引擎将JavaScript代码翻译成CPU指令来执行
+ 比较常见的JavaScript引擎
  - SpiderMonkey: 第一款JavaScript引擎, 由Brendan Eich开发(JavaScript作者)
  - Chakra: 微软开发, 用于IT浏览器
  - JavaScriptCore: WebKit中的JavaScript引擎, 苹果公司开发
  - V8: Google开发的强大的JavaScript引擎, Chrome牛逼之处

### WebKit内核
+ WebKit内核实际上是由两个部分来组成的
  - WebCore: 负责HTML解析, 布局, 渲染等等相关的工作
  - JavaScriptCore: 解析, 执行JavaScript代码
+ 小程序中编写的JavaScript代码就是由JavaScriptCore执行的

### V8引擎
+ 官方对V8引擎的定义
  - V8是用C++编写的Google开源高性能JavaScript和WebAssembly引擎, 它用于Chrome和Node.js等
  - V8引擎实现了ECMAScript和WebAssembly(另外一门前端语言), 并在Window7或者更高版本, macOS和使用x64, IA-32, ARM或MIPS处理器的Linux系统上运行
  - V8可以独立运行, 也可以嵌入到任何C++应用程序中
+ V8引擎的强大之处在于
  - 它可以将一个多次调用的函数(一般来说是会转换为字节码运行), 标记为热点函数, 这个热点函数会转换为优化的机器码, 机器码的运行比字节码更快, 更符合计算机的运行规则
  - 如果, 热点函数中的参数类型发生了变化, 这个热点函数又会被还原为原来的函数
  - 第二个强大的原因是, V8的内存回收模块

### 回顾: Node.js是什么
+ 通过上面的解析, 已经很容易地理解Node.js是什么了
+ Node.js是基于V8引擎来执行JavaScript代码, 但是不仅仅只有V8引擎, Nodejs还有很多其他的模块来进行其他的操作的(比如文件系统的读取写入, 网络IO, 加密, 压缩解压文件等操作)

### 浏览器和Node.js的区别
+ 浏览器不仅仅有解析JavaScript代码的模块, 还有解析渲染HTML/CSS的模块
+ Node.js就没有解析和渲染HTML/CSS的模块
+ Node.js和浏览器共有的模块是中间层和它们的操作系统的模块


### NodeJS的架构


### JavaScript代码的执行
+ 一个js文件, 里面存放JavaScript代码, 如何去执行它呢?
+ 目前我们知道有两种方式去执行它
  - 将代码交给浏览器去执行
  - 将代码载入到node环境中执行
+ 如果我们希望把代码交给浏览器去执行
  - 需要通过让浏览器加载, 解析html代码, 所以需要创建一个html文件
  - 在html文件中通过script标签, 来引入这个js文件
  - 当浏览器遇到script标签的时候, 就会根据src加载, 并执行对应的js代码
+ 如果我们希望把js文件交给node来执行
  - 首先电脑上需要安装nodeJS环境, 安装过程中会自动配置环境变量
  - 然后就可以通过终端命令node js文件名的方式来载入和执行对应的js文件
